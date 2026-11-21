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