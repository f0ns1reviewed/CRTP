# CRTP: Hands-on 1

```
• Enumerate following for the dollarcorp domain:
  - Users
  - Computers
  - Domain Administrators
  - Enterprise Administrators
```
Index: 

  1.  [Enumerate Users](#enumerate-users)
  
  2.  [Enumerate Computers](#enumerate-computers)
  
  3.  [Enumerate Domain Administrators](#enumerate-domain-administrators)
  
  4.  [Enumerate Administrators](#enumerate-administrators)
  
## Enumerate Users
Users Enumeration:
```
PS C:\Users\student162> Get-ADUser -FIlter * -Properties * | select SamAccountName, DistinguishedName, Description

SamAccountName DistinguishedName                                              Description
-------------- -----------------                                              -----------
Administrator  CN=Administrator,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local  Built-in account for administering the...
Guest          CN=Guest,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local          Built-in account for guest access to t...
krbtgt         CN=krbtgt,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local         Key Distribution Center Service Account
mcorp$         CN=mcorp$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
US$            CN=US$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
ecorp$         CN=ecorp$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
sqladmin       CN=sql admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
websvc         CN=web svc,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
srvadmin       CN=srv admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
appadmin       CN=app admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
svcadmin       CN=svc admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local      Account to be used for services which ...
testda         CN=test da,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local        Not what the name implies ;)
mgmtadmin      CN=mgmt admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
ciadmin        CN=ci admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
sql1admin      CN=sql1 admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
studentadmin   CN=studentadmin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student161     CN=student161,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student162     CN=student162,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student163     CN=student163,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student164     CN=student164,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student165     CN=student165,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student166     CN=student166,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student167     CN=student167,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student168     CN=student168,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student169     CN=student169,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student170     CN=student170,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student171     CN=student171,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student172     CN=student172,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student173     CN=student173,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student174     CN=student174,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student175     CN=student175,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student176     CN=student176,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student177     CN=student177,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student178     CN=student178,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student179     CN=student179,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
student180     CN=student180,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control161user CN=Control161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control162user CN=Control162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control163user CN=Control163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control164user CN=Control164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control165user CN=Control165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control166user CN=Control166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control167user CN=Control167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control168user CN=Control168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control169user CN=Control169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control170user CN=Control170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control171user CN=Control171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control172user CN=Control172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control173user CN=Control173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control174user CN=Control174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control175user CN=Control175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control176user CN=Control176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control177user CN=Control177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control178user CN=Control178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control179user CN=Control179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Control180user CN=Control180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support161user CN=Support161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support162user CN=Support162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support163user CN=Support163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support164user CN=Support164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support165user CN=Support165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support166user CN=Support166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support167user CN=Support167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support168user CN=Support168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support169user CN=Support169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support170user CN=Support170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support171user CN=Support171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support172user CN=Support172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support173user CN=Support173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support174user CN=Support174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support175user CN=Support175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support176user CN=Support176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support177user CN=Support177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support178user CN=Support178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support179user CN=Support179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Support180user CN=Support180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN161user     CN=VPN161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN162user     CN=VPN162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN163user     CN=VPN163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN164user     CN=VPN164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN165user     CN=VPN165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN166user     CN=VPN166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN167user     CN=VPN167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN168user     CN=VPN168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN169user     CN=VPN169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN170user     CN=VPN170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN171user     CN=VPN171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN172user     CN=VPN172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN173user     CN=VPN173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN174user     CN=VPN174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN175user     CN=VPN175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN176user     CN=VPN176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN177user     CN=VPN177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN178user     CN=VPN178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN179user     CN=VPN179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
VPN180user     CN=VPN180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
```
Interesting description:

```
 Get-ADUser -FIlter * -Properties * | select SamAccountName, DistinguishedName, Description | ?{ $_.Description -ne $null } | select  -ExpandProperty Description
Built-in account for administering the computer/domain
Built-in account for guest access to the computer/domain
Key Distribution Center Service Account
Account to be used for services which need high privileges.
Not what the name implies ;)
```

## Enumerate Computers

Computers enumeration:
```
Get-ADComputer -Filter * -Properties * | Select SamAccountName, DistinguishedName, Description

SamAccountName  DistinguishedName                                                        Description
--------------  -----------------                                                        -----------
DCORP-DC$       CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-ADMINSRV$ CN=DCORP-ADMINSRV,OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-APPSRV$   CN=DCORP-APPSRV,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-CI$       CN=DCORP-CI,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-MGMT$     CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-MSSQL$    CN=DCORP-MSSQL,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-SQL1$     CN=DCORP-SQL1,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STDADMIN$ CN=DCORP-STDADMIN,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD161$   CN=DCORP-STD161,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD162$   CN=DCORP-STD162,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD163$   CN=DCORP-STD163,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD164$   CN=DCORP-STD164,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD165$   CN=DCORP-STD165,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD166$   CN=DCORP-STD166,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD167$   CN=DCORP-STD167,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD168$   CN=DCORP-STD168,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD169$   CN=DCORP-STD169,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD170$   CN=DCORP-STD170,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD171$   CN=DCORP-STD171,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD172$   CN=DCORP-STD172,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD173$   CN=DCORP-STD173,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD174$   CN=DCORP-STD174,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD175$   CN=DCORP-STD175,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD176$   CN=DCORP-STD176,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD177$   CN=DCORP-STD177,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD178$   CN=DCORP-STD178,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD179$   CN=DCORP-STD179,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
DCORP-STD180$   CN=DCORP-STD180,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

```

## Enumerate Domain Administrators
Enumerate Domain Administrators:

```
Get-ADGroup -Filter { name -like '*Domain Admin*'}


DistinguishedName : CN=Domain Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
GroupCategory     : Security
GroupScope        : Global
Name              : Domain Admins
ObjectClass       : group
ObjectGUID        : 7d766421-bcf7-40b1-a970-17da0bedb489
SamAccountName    : Domain Admins
SID               : S-1-5-21-719815819-3726368948-3917688648-512

```
Members of Domain Admin Group:
```
Get-ADGroup -Filter { name -like '*Domain Admin*'} -Properties * | select -ExpandProperty Members
CN=svc admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Administrator,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
```
## Enumerate Administrators
Foreach Group that contains the Admin word, detect the members:
```
Get-ADGroup -Filter { name -like '*Admin*'} -Properties * | foreach-Object {$_.Name;  $_ | select -ExpandProperty Members}
Administrators
CN=Enterprise Admins,CN=Users,DC=moneycorp,DC=local
CN=Domain Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Administrator,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Hyper-V Administrators
Storage Replica Administrators
Domain Admins
CN=svc admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Administrator,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
Key Admins
DnsAdmins
CN=srv admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
```
Administrators groups, has different domains memebers:

dollarcorp.moneycorp.local members:
```
Get-ADObject -SearchBase 'CN=Domain Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local' -Filter * -Properties * | select Member

Member
------
{CN=svc admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local, CN=Administrator,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local}
```
On root domain moneycorp.local:

```
Get-ADObject -SearchBase 'CN=Enterprise Admins,CN=Users,DC=moneycorp,DC=local' -Server moneycorp.local -FIlter * -Properties * | select Member

Member
------
{CN=Administrator,CN=Users,DC=moneycorp,DC=local}
```


