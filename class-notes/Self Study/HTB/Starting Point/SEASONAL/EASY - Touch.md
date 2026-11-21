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