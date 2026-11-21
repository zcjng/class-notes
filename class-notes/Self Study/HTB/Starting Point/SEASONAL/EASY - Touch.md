PORT     STATE SERVICE
135/tcp  open  msrpc
3389/tcp open  ms-wbt-server
5985/tcp open  wsman
8443/tcp open  https-alt


![[Pasted image 20261121041701.png]]



for mysqld.exe it runs as LocalSystem

The analogy to `setuid` is useful only at a high level: the code is running inside a host that already has an identity. On Windows, the precise rule is:

> **The loaded function starts with the access token of the process that loaded it.**

For the UDF case:

- The **database account** determines whether MySQL permits operations such as registering or invoking the UDF.
- The **Windows service account** determines what the native code can access once it runs.
- The **account that copied the DLL** does not transfer its identity to the DLL.

That is why service-account enumeration matters. If `mysqld.exe` runs as `LocalSystem`, a successful UDF invocation may have very broad local privileges. If it runs as `NETWORK SERVICE`, the code begins with that more restricted token. A UDF does not magically turn a database account into `LocalSystem`.


The service definition is the anchor because it answers three different questions at once:

- **What runs?** The executable path identifies the actual `mysqld.exe`.
- **Who runs it?** The service account tells us the host process token.
- **How is it started?** Arguments may point to the real configuration file, such as `--defaults-file=...`.

This prevents a common failure in automated enumeration: treating a tool’s mention of “MySQL,” “bin,” or “writable” as the conclusion. Tools such as WinPEAS or PowerUp can produce leads, but the service definition is what ties the lead to the process that actually runs.

The discovery chain is therefore:

service name→executable command line→service account→configuration pathservice name→executable command line→service account→configuration path

Only after that chain do directory permissions become meaningful. A writable directory unrelated to the running service is just noise.


The configuration file is the bridge between the service and MySQL’s own behavior. It can tell us things such as:

- the configured `plugin_dir`;
- restrictions on file import/export;
- plugin or component locations;
- custom paths that differ from the installation directory.

It answers **“where and under what application settings might MySQL load or write?”** It does not answer:

- whether your Windows account can write there;
- whether the service actually reaches a load operation;
- whether the resulting code would have useful privileges.

Those require ACL and process evidence. In other words, `my.ini` identifies a candidate path; it does not certify an exploit.

For the UDF route, this is why `plugin_dir` matters: the library must be placed where MySQL’s UDF loading mechanism expects it. The fact that `mysqld.exe` lives in a `bin` directory does not make `bin` the UDF destination.


An ACL answers **who may perform which operation on which object**. It does not answer who runs the service.

For the output shown:

- `BUILTIN\Users:(RX)` on `plugin` means your group can read and traverse that directory, but the output does not grant modification.
- `BUILTIN\Users:(M)` on `bin` means your group has Modify permission there, subject to the effective ACL and inheritance details.

That gives us a crucial separation:

|Evidence|What it establishes|
|---|---|
|`my.ini` points to `plugin`|Where MySQL expects plugins|
|`(RX)` on `plugin`|You can inspect/traverse it, not simply copy into it|
|`(M)` on `bin`|You may be able to alter files there|
|`SERVICE_START_NAME`|Which Windows token hosts the service|

The writable `bin` directory is therefore a lead, not a UDF solution. To make it relevant, we need a loading mechanism that actually uses a file from `bin`.


The branch is not determined by the word “MySQL.” It is determined by which mechanism’s prerequisites are supported by the evidence.

There are two different situations:

- **UDF branch:** the configured plugin location is writable or otherwise usable, the database account has the relevant privileges, and the server can register and invoke the library.
- **DLL branch:** a directory used by a privileged process is writable, and there is evidence that the process requests a library in a way that can resolve to that directory.

In the specific evidence set from the prediction, the ordinary UDF placement route is blocked by the non-writable `plugin_dir`, so DLL loading is the better next hypothesis. But it remains a hypothesis because we have not shown the load trigger.

Conversely, if the plugin directory were writable and the database account could register a UDF, the UDF branch would be the more direct investigation—even if `bin` were also writable. The map’s fork means **compare prerequisites**, not “always choose DLL for a writable `bin`.”


A DLL and a SQL function live in different naming systems. The UDF declaration creates the bridge between them.

Conceptually:

```
native library export:     sys_evalSQL-visible function:      sys_eval
```

The `SONAME` tells MySQL which library contains the implementation. The SQL function name tells MySQL what name a query will use. Later:

```
SELECT sys_eval('whoami');
```

asks MySQL to call that registered native entry point inside `mysqld.exe`.

This requires the library to be suitable for MySQL’s expected interface: the expected export must exist, and its calling convention and return type must be compatible. Registering a random DLL does not make every function inside it SQL-callable, and registration does not mean every export executes automatically.

The UDF chain is therefore:

library in usable location→registration→SQL invocation→native code in mysqld.exelibrary in usable location→registration→SQL invocation→native code in mysqld.exe


Your reasoning named all four relevant conditions, but the submitted selection omitted the fourth. The missing distinction is:

- **UDF execution viability:** can MySQL load and invoke the native function?
- **Privilege-escalation viability:** does the hosting MySQL process have useful Windows privileges?

A UDF can execute successfully while providing no elevation if `mysqld.exe` runs as a low-privileged account. Therefore, for the goal we are studying, all four are required:

1. usable placement;
2. database permission to register/invoke;
3. compatible native export;
4. useful service-account privileges.

The filename is irrelevant except as a search clue.



The DLL branch is different from the UDF branch.

In the UDF branch, MySQL is explicitly told to load a named library through its SQL/application mechanism.

In the DLL-loading branch, the process itself requests a library—often by a name such as `helper.dll`. The operating-system loader then resolves that request according to the applicable search rules. The reasoning chain is:

process requests a library name→loader searches applicable locations→attacker-controlled copy is selected→code runs inside the requesting processprocess requests a library name→loader searches applicable locations→attacker-controlled copy is selected→code runs inside the requesting process

A writable executable directory matters only if it is one of the locations the loader can select **for that particular request**. The directory does not cause a load, and the filename does not grant privileges.

This is why “`bin` is writable” is weaker evidence than “the privileged process requests `helper.dll` by name, and the loader can resolve that request to the writable `bin` copy.”