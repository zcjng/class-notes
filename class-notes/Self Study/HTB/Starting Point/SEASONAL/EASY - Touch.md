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


The DLL branch needs four independent links:

1. **Request:** the process asks for the relevant library.
2. **Resolution:** the request uses search-based resolution rather than an absolute path or another protected mechanism.
3. **Control and trigger:** you can place the candidate where the loader may search, and you can cause the process to reach that load.
4. **Privilege:** the requesting process has a useful Windows token.

Tools can suggest these links, but each must be tied to the actual process. For example:

- a static import or dependency list may suggest a requested DLL name;
- process tracing or controlled service restart may show whether the request occurs;
- ACL inspection establishes whether the candidate location is writable;
- service enumeration establishes the host account.

A writable `bin` directory plus a privileged service is therefore not enough. The missing question is always: **what exact library request causes the service to load our file?**

If the request never occurs, the candidate DLL is inert. If it occurs but resolves to a protected absolute path, the candidate is not selected. If it selects the file but the process is low-privileged, execution may succeed without elevation.

The final skill is to classify each mechanism independently instead of letting one interesting fact prove everything.

## The evidence model

|Question|UDF branch|DLL branch|
|---|---|---|
|Where does the library come from?|A location MySQL uses for UDFs|A location the Windows loader may search|
|What causes loading?|UDF registration and invocation|A process load request and a reproducible trigger|
|What permissions are needed?|Database privileges plus file-placement ability|File replacement ability plus applicable loader resolution|
|What determines OS access?|The `mysqld.exe` service token|The requesting process’s token|
|What proves the path?|Compatible export is registered and invoked|Requested name resolves to the controlled copy|

This also gives a disciplined way to interpret automated enumeration:

- **Lead:** “MySQL exists,” “`bin` is writable,” or “a DLL name looks interesting.”
- **Supported hypothesis:** the service, path, permissions, trigger, and process account all line up.
- **Confirmed execution:** controlled validation shows the intended library was loaded or the intended function was invoked.

For the HTB path you pasted, each action has an evidentiary role:

1. Locate the actual MySQL service and its account.
2. Connect to the database and establish database-level authorization.
3. Ask MySQL for its configured `plugin_dir`.
4. Inspect that directory’s effective ACL.
5. Place a compatible UDF library only if the placement and loading prerequisites are supported.
6. Register the exported native function under a SQL name.
7. Invoke it and interpret the result under the Windows service account.

The database password gets you database authorization. It does **not** itself grant Windows administrator rights. The privilege transition, if one occurs, comes from native code executing inside a MySQL process whose Windows token has more access than the original account.

The DLL branch follows the same evidence discipline but replaces “plugin directory + UDF registration” with “library request + loader resolution.” That is why the two branches merge only at the end: both must answer the same final question—

> **What exact process loads what exact code, under what exact Windows identity, and what evidence proves it?**


The teaching arc is complete.

You now have the core reasoning model:

1. **A file on disk does nothing until a process loads or calls it.**
2. **Native code starts with the host process’s Windows token**, not the account that copied it.
3. **The service definition** identifies the real executable, account, and configuration arguments.
4. **`my.ini`** identifies MySQL-specific paths such as `plugin_dir`; it does not grant write permission.
5. **ACLs** determine what your account can actually modify.
6. **UDF abuse** requires a usable plugin location, database authorization, a compatible export, registration, invocation, and a useful MySQL service token.
7. **DLL hijacking** requires a writable searched location, an actual library request, a reproducible load trigger, and a useful host token.
8. A writable `bin` directory is a **lead**, not proof.
9. RootPEAS and similar tools generate leads; manual validation establishes the mechanism.

Applied to your HTB write-up:

- If `plugin_dir` is writable and the database permissions support UDF registration, follow the **UDF branch**.
- If only the executable directory is writable, do **not** copy a UDF there and assume it works. Investigate the **DLL branch** by proving an actual searched library request.
- If neither branch has all its prerequisites, classify the result as leads only—not an exploit path.



You could ask Shift:

- What does this endpoint appear to do?
    
- Which parameters are user-controlled?
    
- Are there authentication checks?
    
- Are there similar endpoints?
    
- What security tests would be reasonable here?
    

Then you investigate those hypotheses yourself.
