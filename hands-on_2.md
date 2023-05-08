# CRTP: Hands-On 2

```
Enumerate following for the dollarcorp domain:
  List all the OUs
  List all the computers in the StudentMachines OU.
  List the GPOs
  Enumerate GPO applied on the StudentMachines OU.
```

Index:

  1. [List all OUs](#list-all-ous)
  2. [List all computers in StudentMachines OU](#list-all-computers-in-studentmachines-ou)
  3. [List the GPOs](#list-the-gpos)
  4. [Enumerate GPOs in StudentMachines](#enumerate-gpos-in-studentmachines)

## List all OUs

OrganizationalUinits on DOmain dollarcorp.moneycorp.local:
```
Get-ADOrganizationalUnit -Filter * | select DistinguishedName, Name

DistinguishedName                                         Name
-----------------                                         ----
OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local Domain Controllers
OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local    StudentMachines
OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local          Applocked
OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local            Servers
```

## List all computers in StudentMachines OU

Computers on StudentMachines OU:

```
Get-ADComputer -SearchBase 'OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local' -Filter *  | select DNSHostName

DNSHostName
-----------
dcorp-stdadmin.dollarcorp.moneycorp.local
dcorp-std161.dollarcorp.moneycorp.local
dcorp-std162.dollarcorp.moneycorp.local
dcorp-std163.dollarcorp.moneycorp.local
dcorp-std164.dollarcorp.moneycorp.local
dcorp-std165.dollarcorp.moneycorp.local
dcorp-std166.dollarcorp.moneycorp.local
dcorp-std167.dollarcorp.moneycorp.local
dcorp-std168.dollarcorp.moneycorp.local
dcorp-std169.dollarcorp.moneycorp.local
dcorp-std170.dollarcorp.moneycorp.local
dcorp-std171.dollarcorp.moneycorp.local
dcorp-std172.dollarcorp.moneycorp.local
dcorp-std173.dollarcorp.moneycorp.local
dcorp-std174.dollarcorp.moneycorp.local
dcorp-std175.dollarcorp.moneycorp.local
dcorp-std176.dollarcorp.moneycorp.local
dcorp-std177.dollarcorp.moneycorp.local
dcorp-std178.dollarcorp.moneycorp.local
dcorp-std179.dollarcorp.moneycorp.local
dcorp-std180.dollarcorp.moneycorp.local
```

## List the GPOs

```
Get-DomainGPO | select  displayname, gpcmachineextensionnames

displayname                       gpcmachineextensionnames
-----------                       ------------------------
Default Domain Policy             [{35378EAC-683F-11D2-A89A-00C04FBBCFA2}{53D6AB1B-2488-11D1-A28C-00C04FB94F17}][{827D319E-6EAC-11D2-A4EA-00C...
Default Domain Controllers Policy [{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}]
Applocker                         [{35378EAC-683F-11D2-A89A-00C04FBBCFA2}{62C1845D-C4A6-4ACB-BBB0-C895FD090385}{D02B1F72-3407-48AE-BA88-E8213...
Servers                           [{35378EAC-683F-11D2-A89A-00C04FBBCFA2}{D02B1F72-3407-48AE-BA88-E8213C6761F1}][{827D319E-6EAC-11D2-A4EA-00C...
Students                          [{35378EAC-683F-11D2-A89A-00C04FBBCFA2}{D02B1F72-3407-48AE-BA88-E8213C6761F1}][{827D319E-6EAC-11D2-A4EA-00C...
```

## Enumerate GPOs in StudentMachines

GPOs on Students:

```
Get-DomainGPO -Identity Students -Properties *


flags                    : 0
displayname              : Students
gpcmachineextensionnames : [{35378EAC-683F-11D2-A89A-00C04FBBCFA2}{D02B1F72-3407-48AE-BA88-E8213C6761F1}][{827D319E-6EAC-11D2-A4EA-00C04F79F83A}
                           {803E14A0-B4FB-11D0-A0D0-00A0C90F574B}]
whenchanged              : 11/15/2022 5:48:32 AM
versionnumber            : 6
name                     : {7478F170-6A0C-490C-B355-9E4618BC785D}
cn                       : {7478F170-6A0C-490C-B355-9E4618BC785D}
usnchanged               : 45959
dscorepropagationdata    : 1/1/1601 12:00:00 AM
objectguid               : 0076f619-ffef-4488-bfdb-1fc028c5cb14
gpcfilesyspath           : \\dollarcorp.moneycorp.local\SysVol\dollarcorp.moneycorp.local\Policies\{7478F170-6A0C-490C-B355-9E4618BC785D}
distinguishedname        : CN={7478F170-6A0C-490C-B355-9E4618BC785D},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
whencreated              : 11/15/2022 5:46:19 AM
showinadvancedviewonly   : True
usncreated               : 45927
gpcfunctionalityversion  : 2
instancetype             : 4
objectclass              : {top, container, groupPolicyContainer}
objectcategory           : CN=Group-Policy-Container,CN=Schema,CN=Configuration,DC=moneycorp,DC=local

```
Applayed GPOs on every studentMachines computers:

```
Get-ADComputer -SearchBase 'OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local' -Filter *  -Properties * | select name | Foreach-Object {$_.name; Get-DomainGPO -ComputerIdentity $_.name 2> $null | select displayname | FT}
DCORP-STDADMIN

displayname
-----------
Students
Default Domain Policy


DCORP-STD161

displayname
-----------
Students
Default Domain Policy


DCORP-STD162

displayname
-----------
Students
Default Domain Policy


DCORP-STD163

displayname
-----------
Students
Default Domain Policy


DCORP-STD164

displayname
-----------
Students
Default Domain Policy


DCORP-STD165

displayname
-----------
Students
Default Domain Policy


DCORP-STD166

displayname
-----------
Students
Default Domain Policy


DCORP-STD167

displayname
-----------
Students
Default Domain Policy


DCORP-STD168

displayname
-----------
Students
Default Domain Policy


DCORP-STD169

displayname
-----------
Students
Default Domain Policy


DCORP-STD170

displayname
-----------
Students
Default Domain Policy


DCORP-STD171

displayname
-----------
Students
Default Domain Policy


DCORP-STD172

displayname
-----------
Students
Default Domain Policy


DCORP-STD173

displayname
-----------
Students
Default Domain Policy


DCORP-STD174

displayname
-----------
Students
Default Domain Policy


DCORP-STD175

displayname
-----------
Students
Default Domain Policy


DCORP-STD176

displayname
-----------
Students
Default Domain Policy


DCORP-STD177

displayname
-----------
Students
Default Domain Policy


DCORP-STD178

displayname
-----------
Students
Default Domain Policy


DCORP-STD179

displayname
-----------
Students
Default Domain Policy


DCORP-STD180

displayname
-----------
Students
Default Domain Policy

```
