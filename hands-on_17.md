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
```
