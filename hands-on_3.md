CRTP: Hands-on 3

```
Enumerate following for the dollarcorp domain:
  – ACL for the Domain Admins group
  – All modify rights/permissions for the studentx
```

Index:

  1. [ACL Domain Admins](#acl-domain-admins)
  2. [Permissions studentx](#permissions-studentx)
  3. [Find Interesting permissions](#find-interesting-permissions)


## ACL Domain Admins

```
PS C:\Users\student162> Get-DomainObjectAcl | select -expandProperty ObjectDN  | Get-Unique | % {$_;Get-Acl AD:\$_  | select -ExpandProperty Access | ?{$_.IdentityReference -like '*Domain Admins*'} | FT}
DC=dollarcorp,DC=moneycorp,DC=local

                                                              ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited
                                                              --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   -----------
CreateChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False


OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD175,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD175,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD176,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD176,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD177,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD177,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD178,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD178,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD179,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD179,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD180,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD180,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STDADMIN,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STDADMIN,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD161,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD161,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None

CN=DCORP-STD162,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD162,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD163,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD163,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD164,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD164,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD165,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD165,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD166,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD166,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD167,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD167,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD168,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD168,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD169,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD169,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD170,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD170,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD171,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD171,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD172,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD172,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD173,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD173,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-STD174,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-STD174,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-ADMINSRV,OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-ADMINSRV,OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-MSSQL,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-MSSQL,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-SQL1,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-SQL1,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-APPSRV,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-APPSRV,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-CI,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=Windows Virtual Machine,CN=DCORP-CI,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None

CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=ecorp$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None


CN=sql admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=web svc,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=srv admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=app admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=svc admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=test da,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=mgmt admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=krbtgt,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=ci admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Domain Computers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=sql1 admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Domain Controllers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Cert Publishers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Domain Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Domain Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Domain Guests,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Group Policy Creator Owners,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=RAS and IAS Servers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Allowed RODC Password Replication Group,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Denied RODC Password Replication Group,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Read-only Domain Controllers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Cloneable Domain Controllers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Protected Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Key Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DnsUpdateProxy,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=studentadmin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student161,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=mcorp$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None


CN=student162,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student163,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student164,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student165,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student166,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student167,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student168,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student169,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student170,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student171,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student172,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student173,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student174,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student175,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student176,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None

CN=student177,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student178,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student179,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=student180,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=US$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None


CN=Control170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Control180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Support180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=VPN180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Administrator,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Guest,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Computers,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local

                                                              ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited
                                                              --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   -----------
CreateChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False


CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 4c164200-20c0-11d0-a768-00aa006e0529 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967953-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None f3a64788-5306-11d1-a9c5-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 72e39547-7b18-11d1-adef-00c04fd8d5cd 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 3e0abfd0-126a-11d0-a060-00aa006c33ed 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None bf967950-0de6-11d0-a285-00aa003049e2 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 5f202010-79a5-11d0-9020-00c04fc2d4cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None
                 Self            None 9b026da6-0d3c-465c-8bee-5199d7165cba 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins        True             None             None


CN=RID Set,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DFSR-LocalSettings,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Domain System Volume,CN=DFSR-LocalSettings,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=SYSVOL Subscription,CN=Domain System Volume,CN=DFSR-LocalSettings,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                              ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited
                                                              --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   -----------
CreateChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False


CN=eurocorp.local,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\Domain Admins       False             None             None
        WriteProperty            None 736e4812-af31-11d2-b7df-00805f48caeb 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow dcorp\Domain Admins       False             None             None


CN=Server,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=moneycorp.local,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=RID Manager$,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=@,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=A.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=B.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=C.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=D.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=E.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=F.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=G.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=H.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=I.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=J.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=K.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=L.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


DC=M.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=WinsockServices,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=RpcServices,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=FileLinks,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=ObjectMoveTable,CN=FileLinks,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Default Domain Policy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=AppCategories,CN=Default Domain Policy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Meetings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN={0D1CC23D-1F20-4EEE-AF64-D99597AE2A6E},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN=Machine,CN={0D1CC23D-1F20-4EEE-AF64-D99597AE2A6E},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
                                                                                           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN=User,CN={0D1CC23D-1F20-4EEE-AF64-D99597AE2A6E},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
                                                                                           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN={308279C1-FFB6-4D52-948C-660B07AC77FB},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN=Machine,CN={308279C1-FFB6-4D52-948C-660B07AC77FB},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
                                                                                           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN=User,CN={308279C1-FFB6-4D52-948C-660B07AC77FB},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
                                                                                           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...

CN={7478F170-6A0C-490C-B355-9E4618BC785D},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN=Machine,CN={7478F170-6A0C-490C-B355-9E4618BC785D},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
                                                                                           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN=User,CN={7478F170-6A0C-490C-B355-9E4618BC785D},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
                                                                                           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner     Descendents 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...
                                 CreateChild, Self, WriteProperty, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN=User,CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
     CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN=Machine,CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
     CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN={6AC1786C-016F-11D2-945F-00C04fB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner     Descendents 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...
                                 CreateChild, Self, WriteProperty, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN=User,CN={6AC1786C-016F-11D2-945F-00C04fB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
     CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN=Machine,CN={6AC1786C-016F-11D2-945F-00C04fB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                                ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityRefere
                                                                                                                                                                                                                              nce
                                                                                --------------------- --------------- ----------                           -------------------                  ----------- ----------------- --------------
     CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...
CreateChild, DeleteChild, Self, WriteProperty, DeleteTree, Delete, GenericRead, WriteDacl, WriteOwner             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domai...


CN=RAS and IAS Servers Access Check,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=File Replication Service,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Dfs-Configuration,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Content,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=SYSVOL Share,CN=Content,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Topology,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=DCORP-DC,CN=Topology,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=AdminSDHolder,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=ComPartitions,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=ComPartitionSets,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False ContainerInherit             None


CN=PolicyTemplate,CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins
                                                                                      GenericAll             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=SOM,CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False ContainerInherit             None


CN=PolicyType,CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins
                                                                                      GenericAll             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=WMIGPO,CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins
                                                                                      GenericAll             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6E157EDF-4E72-4052-A82A-EC3F91021A22,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=ab402345-d3c3-455d-9ff7-40268a1099b6,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=bab5f54d-06c8-48de-9b87-d78b796564e4,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=f3dd09dd-25e8-4f9c-85df-12d6d2f2f2f5,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=2416c60a-fe15-4d7a-a61e-dffd5df864d3,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=7868d4c8-ac41-4e05-b401-776280e8e9f1,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=860c36ed-5241-4c62-a18b-cf6ff9994173,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=0e660ea3-8a5e-4495-9ad7-ca1bd4638f9e,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=a86fe12a-0f62-4e2a-b271-d27f601f8182,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=d85c0bfd-094f-4cad-a2b5-82ac9268475d,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6ada9ff7-c9df-45c1-908e-9fef2fab008a,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=10b3ad2a-6883-4fa7-90fc-6377cbdc1b26,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=98de1d3e-6611-443b-8b4e-f4337f1ded0b,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=f607fd87-80cf-45e2-890b-6cf97ec0e284,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=9cac1f66-2167-47ad-a472-2a13251310e4,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6ff880d6-11e7-4ed1-a20f-aac45da48650,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=446f24ea-cfd5-4c52-8346-96e170bcb912,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=51cba88b-99cf-4e16-bef2-c427b38d0767,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=a3dac986-80e7-4e59-a059-54cb1ab43cb9,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=293f0798-ea5c-4455-9f5d-45f33a30703b,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=5c82b233-75fc-41b3-ac71-c69592e6bf15,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=7ffef925-405b-440a-8d58-35e8cd6e98c3,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=4dfbb973-8a62-4310-a90c-776e00f83222,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=8437C3D8-7689-4200-BF38-79E4AC33DFA0,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=7cfb016c-4f87-4406-8166-bd9df943947f,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=f7ed4553-d82b-49ef-a839-2f38a36bb069,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=8ca38317-13a4-4bd4-806f-ebed6acb5d0c,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=3c784009-1f57-4e2a-9b04-6915c9e71961,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd5678-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd5679-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd567a-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd567b-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd567c-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd567d-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd567e-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd567f-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd5680-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd5681-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd5682-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd5683-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd5684-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd5685-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd5686-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd5687-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd5688-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd5689-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd568a-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd568b-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd568c-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=6bcd568d-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=3051c66f-b332-4a73-9a20-2d6a7d6e6a1c,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=3e4f4182-ac5d-4378-b760-0eab2de593e2,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=c4f17608-e611-11d6-9793-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=13d15cf0-e6c8-11d6-9793-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=8ddf6913-1c7b-4c59-a5af-b9ca3b3d2c4c,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=dda1d01d-4bd7-4c49-a184-46f9241b560e,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=a1789bfb-e0a2-4739-8cc0-e77d892d080a,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=61b34cb0-55ee-4be9-b595-97810b92b017,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=57428d75-bef7-43e1-938b-2e749f5a8d56,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=ebad865a-d649-416f-9922-456b53bbb5b8,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=0b7fb422-3609-4587-8c2e-94b10f67d1bf,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=2951353e-d102-4ea5-906c-54247eeec741,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=71482d49-8870-4cb3-a438-b6fc9ec35d70,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=aed72870-bf16-4788-8ac7-22299c8207f1,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=f58300d1-b71a-4DB6-88a1-a8b9538beaca,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=231fb90b-c92a-40c9-9379-bacfc313a3e3,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=4aaabc3a-c416-4b9c-a6bb-4b453ab1c1f0,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=9738c400-7795-4d6e-b19d-c16cd6486166,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=de10d491-909f-4fb0-9abb-4b7865c0fe80,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=b96ed344-545a-4172-aa0c-68118202f125,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=4c93ad42-178a-4275-8600-16811d28f3aa,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=c88227bc-fcca-4b58-8d8a-cd3d64528a02,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=5e1574f6-55df-493e-a671-aaeffca6a100,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=d262aae8-41f7-48ed-9f35-56bbb677573d,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=82112ba0-7e4c-4a44-89d9-d46c9612bf91,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=c3c927a6-cc1d-47c0-966b-be8f9b63d991,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=54afcfb9-637a-4251-9f47-4d50e7021211,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=f4728883-84dd-483c-9897-274f2ebcf11e,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=83C53DA7-427E-47A4-A07A-A324598B88F7,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=C81FC9CC-0130-4FD1-B272-634D74818133,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=E5F9E791-D96D-4FC9-93C9-D53E1DC439BA,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=e6d5fd00-385d-4e65-b02d-9da3493ed850,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=3a6b3fbf-3168-4312-a10d-dd5b3393952d,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=7F950403-0AB3-47F9-9730-5D7B0269F9BD,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=434bb40d-dbc9-4fe7-81d4-d57229f7b080,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=A0C238BA-9E30-4EE6-80A6-43F731E9A5CD,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Windows2003Update,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=ActiveDirectoryUpdate,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=us.dollarcorp.moneycorp.local,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=PSPs,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=LostAndFound,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Infrastructure,DC=dollarcorp,DC=moneycorp,DC=local

                                                              ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited
                                                              --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   -----------
CreateChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False


CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=S-1-5-17,CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=S-1-5-9,CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=S-1-5-4,CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Program Data,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Microsoft,CN=Program Data,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Managed Service Accounts,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

                                                              ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited
                                                              --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   -----------
CreateChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False


CN=Guests,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Print Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Backup Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Replicator,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Remote Desktop Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Network Configuration Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Performance Monitor Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Performance Log Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Distributed COM Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=IIS_IUSRS,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Cryptographic Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Event Log Readers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Certificate Service DCOM Access,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=RDS Remote Access Servers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=RDS Endpoint Servers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=RDS Management Servers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Hyper-V Administrators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Access Control Assistance Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Remote Management Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Storage Replica Administrators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Server Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Account Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Pre-Windows 2000 Compatible Access,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Windows Authorization Access Group,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Terminal Server License Servers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


CN=Administrators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

                                                                           ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference
                                                                           --------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------
CreateChild, DeleteChild, Self, WriteProperty, ExtendedRight, GenericRead, WriteDacl, WriteOwner            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins


CN=Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference   IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------   ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\Domain Admins       False             None             None


```


## Permissions studentx
Default groups recursive:
```
 Get-ADPrincipalGroupMembershipRecursive student162 | Get-ADGroup | Select Name

Name
----
Domain Users
RDP Users
Users

```

RDP permissions:
```

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=US$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Control170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Control180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None


CN=Support180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- ----------------- ----------- ---------------- ----------------
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow dcorp\RDPUsers          False             None             None

```

Domain Users permissions and users permissions:

```
 Get-DomainObjectAcl | select -expandProperty ObjectDN  | Get-Unique | % {$_;Get-Acl AD:\$_  | select -ExpandProperty Access | ?{$_.IdentityReference -like '*Users*'} | FT}
DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None 05c74c5e-4deb-43b4-bd9f-86664c2a7fd5 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ccc2dc7d-a6ad-4a7a-8846-c04e3cc53501 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None 280f369c-67c7-438e-ae98-1d46f3c6f541 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None b8119fd0-04f6-4762-ab7a-4986c76b3f9a 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD175,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD175,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD176,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD176,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD177,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD177,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD178,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD178,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD179,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD179,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD180,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD180,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STDADMIN,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STDADMIN,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD161,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD161,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD162,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD162,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD163,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD163,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD164,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD164,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD165,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD165,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD166,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD166,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD167,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD167,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD168,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD168,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD169,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD169,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD170,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD170,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD171,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD171,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD172,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD172,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD173,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD173,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-STD174,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-STD174,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-ADMINSRV,OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-ADMINSRV,OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-MSSQL,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-MSSQL,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-SQL1,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-SQL1,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-APPSRV,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-APPSRV,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-CI,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Virtual Machine,CN=DCORP-CI,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=ecorp$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=sql admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=web svc,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=srv admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=app admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=svc admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=test da,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=mgmt admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=krbtgt,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=ci admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Domain Computers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=sql1 admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Domain Controllers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Cert Publishers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Domain Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Domain Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Domain Guests,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Group Policy Creator Owners,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=RAS and IAS Servers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Allowed RODC Password Replication Group,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Denied RODC Password Replication Group,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Read-only Domain Controllers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Cloneable Domain Controllers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Protected Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Key Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DnsUpdateProxy,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=studentadmin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student161,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=mcorp$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student162,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student163,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student164,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student165,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student166,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student167,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student168,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student169,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student170,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student171,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student172,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student173,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student174,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student175,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student176,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student177,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student178,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student179,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=student180,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=US$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Control180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Support180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
           GenericAll            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow dcorp\RDPUsers                         False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=VPN180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Administrator,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Guest,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Computers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=RID Set,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DFSR-LocalSettings,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Domain System Volume,CN=DFSR-LocalSettings,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=SYSVOL Subscription,CN=Domain System Volume,CN=DFSR-LocalSettings,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=eurocorp.local,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Server,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None 91d67418-0135-4acc-8d79-c08e857cfbec 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=moneycorp.local,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=RID Manager$,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None

DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=@,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=A.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=B.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=C.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=D.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=E.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=F.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=G.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=H.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=I.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=J.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=K.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=L.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=M.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=WinsockServices,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=RpcServices,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=FileLinks,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=ObjectMoveTable,CN=FileLinks,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Default Domain Policy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=AppCategories,CN=Default Domain Policy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Meetings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN={0D1CC23D-1F20-4EEE-AF64-D99597AE2A6E},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False ContainerInherit             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False ContainerInherit             None


CN=Machine,CN={0D1CC23D-1F20-4EEE-AF64-D99597AE2A6E},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN=User,CN={0D1CC23D-1F20-4EEE-AF64-D99597AE2A6E},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN={308279C1-FFB6-4D52-948C-660B07AC77FB},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False ContainerInherit             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False ContainerInherit             None


CN=Machine,CN={308279C1-FFB6-4D52-948C-660B07AC77FB},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN=User,CN={308279C1-FFB6-4D52-948C-660B07AC77FB},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN={7478F170-6A0C-490C-B355-9E4618BC785D},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False ContainerInherit             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False ContainerInherit             None


CN=Machine,CN={7478F170-6A0C-490C-B355-9E4618BC785D},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN=User,CN={7478F170-6A0C-490C-B355-9E4618BC785D},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False ContainerInherit             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False ContainerInherit             None


CN=User,CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN=Machine,CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN={6AC1786C-016F-11D2-945F-00C04fB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False ContainerInherit             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False ContainerInherit             None


CN=User,CN={6AC1786C-016F-11D2-945F-00C04fB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN=Machine,CN={6AC1786C-016F-11D2-945F-00C04fB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None
        ExtendedRight             All edacfd8f-ffb3-11d1-b41d-00a0c968f939 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN=RAS and IAS Servers Access Check,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=File Replication Service,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Dfs-Configuration,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Content,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=SYSVOL Share,CN=Content,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Topology,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=DCORP-DC,CN=Topology,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=AdminSDHolder,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=ComPartitions,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=ComPartitionSets,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False ContainerInherit             None


CN=PolicyTemplate,CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN=SOM,CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False ContainerInherit             None


CN=PolicyType,CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN=WMIGPO,CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None
          GenericRead             All 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users        True ContainerInherit             None


CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6E157EDF-4E72-4052-A82A-EC3F91021A22,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=ab402345-d3c3-455d-9ff7-40268a1099b6,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=bab5f54d-06c8-48de-9b87-d78b796564e4,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=f3dd09dd-25e8-4f9c-85df-12d6d2f2f2f5,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=2416c60a-fe15-4d7a-a61e-dffd5df864d3,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=7868d4c8-ac41-4e05-b401-776280e8e9f1,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=860c36ed-5241-4c62-a18b-cf6ff9994173,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=0e660ea3-8a5e-4495-9ad7-ca1bd4638f9e,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=a86fe12a-0f62-4e2a-b271-d27f601f8182,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=d85c0bfd-094f-4cad-a2b5-82ac9268475d,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6ada9ff7-c9df-45c1-908e-9fef2fab008a,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=10b3ad2a-6883-4fa7-90fc-6377cbdc1b26,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=98de1d3e-6611-443b-8b4e-f4337f1ded0b,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=f607fd87-80cf-45e2-890b-6cf97ec0e284,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=9cac1f66-2167-47ad-a472-2a13251310e4,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6ff880d6-11e7-4ed1-a20f-aac45da48650,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=446f24ea-cfd5-4c52-8346-96e170bcb912,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=51cba88b-99cf-4e16-bef2-c427b38d0767,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=a3dac986-80e7-4e59-a059-54cb1ab43cb9,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=293f0798-ea5c-4455-9f5d-45f33a30703b,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=5c82b233-75fc-41b3-ac71-c69592e6bf15,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=7ffef925-405b-440a-8d58-35e8cd6e98c3,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=4dfbb973-8a62-4310-a90c-776e00f83222,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=8437C3D8-7689-4200-BF38-79E4AC33DFA0,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=7cfb016c-4f87-4406-8166-bd9df943947f,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=f7ed4553-d82b-49ef-a839-2f38a36bb069,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=8ca38317-13a4-4bd4-806f-ebed6acb5d0c,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=3c784009-1f57-4e2a-9b04-6915c9e71961,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd5678-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd5679-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd567a-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd567b-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd567c-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd567d-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd567e-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd567f-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd5680-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd5681-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd5682-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd5683-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd5684-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd5685-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd5686-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd5687-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd5688-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd5689-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd568a-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd568b-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd568c-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=6bcd568d-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=3051c66f-b332-4a73-9a20-2d6a7d6e6a1c,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=3e4f4182-ac5d-4378-b760-0eab2de593e2,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=c4f17608-e611-11d6-9793-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=13d15cf0-e6c8-11d6-9793-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=8ddf6913-1c7b-4c59-a5af-b9ca3b3d2c4c,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=dda1d01d-4bd7-4c49-a184-46f9241b560e,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=a1789bfb-e0a2-4739-8cc0-e77d892d080a,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=61b34cb0-55ee-4be9-b595-97810b92b017,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=57428d75-bef7-43e1-938b-2e749f5a8d56,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=ebad865a-d649-416f-9922-456b53bbb5b8,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=0b7fb422-3609-4587-8c2e-94b10f67d1bf,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=2951353e-d102-4ea5-906c-54247eeec741,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=71482d49-8870-4cb3-a438-b6fc9ec35d70,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=aed72870-bf16-4788-8ac7-22299c8207f1,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=f58300d1-b71a-4DB6-88a1-a8b9538beaca,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=231fb90b-c92a-40c9-9379-bacfc313a3e3,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=4aaabc3a-c416-4b9c-a6bb-4b453ab1c1f0,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=9738c400-7795-4d6e-b19d-c16cd6486166,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=de10d491-909f-4fb0-9abb-4b7865c0fe80,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=b96ed344-545a-4172-aa0c-68118202f125,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=4c93ad42-178a-4275-8600-16811d28f3aa,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=c88227bc-fcca-4b58-8d8a-cd3d64528a02,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=5e1574f6-55df-493e-a671-aaeffca6a100,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=d262aae8-41f7-48ed-9f35-56bbb677573d,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=82112ba0-7e4c-4a44-89d9-d46c9612bf91,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=c3c927a6-cc1d-47c0-966b-be8f9b63d991,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=54afcfb9-637a-4251-9f47-4d50e7021211,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=f4728883-84dd-483c-9897-274f2ebcf11e,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=83C53DA7-427E-47A4-A07A-A324598B88F7,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=C81FC9CC-0130-4FD1-B272-634D74818133,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=E5F9E791-D96D-4FC9-93C9-D53E1DC439BA,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=e6d5fd00-385d-4e65-b02d-9da3493ed850,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=3a6b3fbf-3168-4312-a10d-dd5b3393952d,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=7F950403-0AB3-47F9-9730-5D7B0269F9BD,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=434bb40d-dbc9-4fe7-81d4-d57229f7b080,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=A0C238BA-9E30-4EE6-80A6-43F731E9A5CD,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows2003Update,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=ActiveDirectoryUpdate,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=us.dollarcorp.moneycorp.local,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=PSPs,CN=System,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=LostAndFound,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Infrastructure,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=S-1-5-17,CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=S-1-5-9,CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=S-1-5-4,CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          ReadControl            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 59ba2f42-79a2-11d0-9020-00c04fc2d3cf 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e48d0154-bcf8-11d1-8702-00c04fb96050 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None e45795b3-9455-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None 77b5b886-944a-11d1-aebd-0000f80367c1 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Program Data,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Microsoft,CN=Program Data,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Managed Service Accounts,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None 05c74c5e-4deb-43b4-bd9f-86664c2a7fd5 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ccc2dc7d-a6ad-4a7a-8846-c04e3cc53501 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
         ReadProperty            None b8119fd0-04f6-4762-ab7a-4986c76b3f9a 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None 280f369c-67c7-438e-ae98-1d46f3c6f541 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Guests,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Print Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Backup Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Replicator,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Remote Desktop Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Network Configuration Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Performance Monitor Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Performance Log Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Distributed COM Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=IIS_IUSRS,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Cryptographic Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Event Log Readers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Certificate Service DCOM Access,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=RDS Remote Access Servers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=RDS Endpoint Servers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=RDS Management Servers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Hyper-V Administrators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Access Control Assistance Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Remote Management Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Storage Replica Administrators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Server Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Account Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Pre-Windows 2000 Compatible Access,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Windows Authorization Access Group,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Terminal Server License Servers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Administrators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                  ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                  ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000        None             Allow NT AUTHORITY\Authenticated Users       False             None             None


CN=Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights InheritanceType ObjectType                           InheritedObjectType                           ObjectFlags AccessControlType IdentityReference                IsInherited InheritanceFlags PropagationFlags
--------------------- --------------- ----------                           -------------------                           ----------- ----------------- -----------------                ----------- ---------------- ----------------
          GenericRead            None 00000000-0000-0000-0000-000000000000 00000000-0000-0000-0000-000000000000                 None             Allow NT AUTHORITY\Authenticated Users       False             None             None
        ExtendedRight            None ab721a55-1e2f-11d0-9819-00aa0040529b 00000000-0000-0000-0000-000000000000 ObjectAceTypePresent             Allow NT AUTHORITY\Authenticated Users       False             None             None


```

## Find Interesting permissions

```
PS C:\Users\student162> Find-InterestingDomainAcl


ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD175,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5195
IdentityReferenceName   : DCORP-STD175$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD175,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD176,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5196
IdentityReferenceName   : DCORP-STD176$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD176,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD177,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5197
IdentityReferenceName   : DCORP-STD177$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD177,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD178,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5198
IdentityReferenceName   : DCORP-STD178$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD178,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD179,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5199
IdentityReferenceName   : DCORP-STD179$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD179,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD180,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5200
IdentityReferenceName   : DCORP-STD180$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD180,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STDADMIN,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-4202
IdentityReferenceName   : DCORP-STDADMIN$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STDADMIN,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD161,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5181
IdentityReferenceName   : DCORP-STD161$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD161,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD162,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5182
IdentityReferenceName   : DCORP-STD162$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD162,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD163,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5183
IdentityReferenceName   : DCORP-STD163$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD163,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD164,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5184
IdentityReferenceName   : DCORP-STD164$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD164,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD165,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5185
IdentityReferenceName   : DCORP-STD165$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD165,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD166,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5186
IdentityReferenceName   : DCORP-STD166$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD166,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD167,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5187
IdentityReferenceName   : DCORP-STD167$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD167,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD168,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5188
IdentityReferenceName   : DCORP-STD168$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD168,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD169,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5189
IdentityReferenceName   : DCORP-STD169$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD169,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD170,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5190
IdentityReferenceName   : DCORP-STD170$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD170,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD171,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5191
IdentityReferenceName   : DCORP-STD171$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD171,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD172,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5192
IdentityReferenceName   : DCORP-STD172$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD172,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD173,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5193
IdentityReferenceName   : DCORP-STD173$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD173,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-STD174,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-5194
IdentityReferenceName   : DCORP-STD174$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-STD174,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-ADMINSRV,OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1105
IdentityReferenceName   : DCORP-ADMINSRV$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-ADMINSRV,OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

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

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1108
IdentityReferenceName   : DCORP-MGMT$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-MSSQL,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1109
IdentityReferenceName   : DCORP-MSSQL$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-MSSQL,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-SQL1,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1110
IdentityReferenceName   : DCORP-SQL1$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-SQL1,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-APPSRV,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1106
IdentityReferenceName   : DCORP-APPSRV$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-APPSRV,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Windows Virtual Machine,CN=DCORP-CI,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1107
IdentityReferenceName   : DCORP-CI$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-CI,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Control161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Control180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=Support180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1123
IdentityReferenceName   : RDPUsers
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : CN=DFSR-LocalSettings,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : 00000000-0000-0000-0000-000000000000
AceFlags                : None
AceType                 : AccessAllowedObject
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1000
IdentityReferenceName   : DCORP-DC$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=Domain System Volume,CN=DFSR-LocalSettings,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : 00000000-0000-0000-0000-000000000000
AceFlags                : Inherited
AceType                 : AccessAllowedObject
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1000
IdentityReferenceName   : DCORP-DC$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : CN=SYSVOL Subscription,CN=Domain System Volume,CN=DFSR-LocalSettings,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : GenericAll
ObjectAceType           : 00000000-0000-0000-0000-000000000000
AceFlags                : Inherited
AceType                 : AccessAllowedObject
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1000
IdentityReferenceName   : DCORP-DC$
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : computer

ObjectDN                : DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=@,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=A.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=B.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=C.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=D.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=E.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=F.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=G.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=H.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=I.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=J.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=K.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=L.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

ObjectDN                : DC=M.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : CreateChild, DeleteChild, ListChildren, ReadProperty, DeleteTree, ExtendedRight, Delete, GenericWrite, WriteDacl, WriteOwner
ObjectAceType           : None
AceFlags                : ContainerInherit, Inherited
AceType                 : AccessAllowed
InheritanceFlags        : ContainerInherit
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1101
IdentityReferenceName   : DnsAdmins
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : group

```
Extra sweet Example:
```
PS C:\Users\student162> Get-DomainObjectAcl | select -expandProperty ObjectDN  | Get-Unique | % {$_;Get-Acl AD:\$_  | select -ExpandProperty Access | ?{$_.IdentityReference -like '*RDP*'} | select ActiveDirectoryRights, IdentityReference |FT}
DC=dollarcorp,DC=moneycorp,DC=local
OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD175,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD175,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD176,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD176,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD177,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD177,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD178,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD178,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD179,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD179,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD180,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD180,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STDADMIN,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STDADMIN,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD161,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD161,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD162,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD162,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD163,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD163,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD164,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD164,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD165,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD165,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD166,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD166,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD167,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD167,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD168,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD168,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD169,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD169,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD170,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD170,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD171,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD171,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD172,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD172,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD173,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD173,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-STD174,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-STD174,OU=StudentMachines,DC=dollarcorp,DC=moneycorp,DC=local
OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-ADMINSRV,OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-ADMINSRV,OU=Applocked,DC=dollarcorp,DC=moneycorp,DC=local
OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-MSSQL,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-MSSQL,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-SQL1,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-SQL1,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-APPSRV,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-APPSRV,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-CI,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Virtual Machine,CN=DCORP-CI,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=ecorp$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=sql admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=web svc,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=srv admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=app admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=svc admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=test da,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=mgmt admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=krbtgt,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=ci admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Domain Computers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=sql1 admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Domain Controllers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Cert Publishers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=RDP Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Domain Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Domain Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Domain Guests,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Group Policy Creator Owners,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=RAS and IAS Servers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Allowed RODC Password Replication Group,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Denied RODC Password Replication Group,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Read-only Domain Controllers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Cloneable Domain Controllers,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Protected Users,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Key Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=DnsAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=DnsUpdateProxy,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=studentadmin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student161,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=mcorp$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student162,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student163,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student164,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student165,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student166,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student167,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student168,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student169,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student170,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student171,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student172,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student173,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student174,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student175,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student176,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student177,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student178,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student179,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=student180,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Control161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=US$,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Control170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Control180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=Support180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local

ActiveDirectoryRights IdentityReference
--------------------- -----------------
           GenericAll dcorp\RDPUsers


CN=VPN161User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN162User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN163User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN164User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN165User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN166User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN167User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN168User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN169User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN170User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN171User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN172User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN173User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN174User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN175User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN176User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN177User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN178User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN179User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=VPN180User,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Administrator,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Guest,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=Computers,DC=dollarcorp,DC=moneycorp,DC=local
OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
CN=RID Set,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
CN=DFSR-LocalSettings,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
CN=Domain System Volume,CN=DFSR-LocalSettings,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
CN=SYSVOL Subscription,CN=Domain System Volume,CN=DFSR-LocalSettings,CN=DCORP-DC,OU=Domain Controllers,DC=dollarcorp,DC=moneycorp,DC=local
CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=eurocorp.local,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Server,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=moneycorp.local,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=RID Manager$,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=@,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=A.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=B.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=C.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=D.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=E.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=F.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=G.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=H.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=I.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=J.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=K.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=L.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
DC=M.ROOT-SERVERS.NET,DC=RootDNSServers,CN=MicrosoftDNS,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=WinsockServices,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=RpcServices,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=FileLinks,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=ObjectMoveTable,CN=FileLinks,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Default Domain Policy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=AppCategories,CN=Default Domain Policy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Meetings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN={0D1CC23D-1F20-4EEE-AF64-D99597AE2A6E},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Machine,CN={0D1CC23D-1F20-4EEE-AF64-D99597AE2A6E},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=User,CN={0D1CC23D-1F20-4EEE-AF64-D99597AE2A6E},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN={308279C1-FFB6-4D52-948C-660B07AC77FB},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Machine,CN={308279C1-FFB6-4D52-948C-660B07AC77FB},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=User,CN={308279C1-FFB6-4D52-948C-660B07AC77FB},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN={7478F170-6A0C-490C-B355-9E4618BC785D},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Machine,CN={7478F170-6A0C-490C-B355-9E4618BC785D},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=User,CN={7478F170-6A0C-490C-B355-9E4618BC785D},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=User,CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Machine,CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN={6AC1786C-016F-11D2-945F-00C04fB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=User,CN={6AC1786C-016F-11D2-945F-00C04fB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Machine,CN={6AC1786C-016F-11D2-945F-00C04fB984F9},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=RAS and IAS Servers Access Check,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=File Replication Service,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Dfs-Configuration,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Content,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=SYSVOL Share,CN=Content,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Topology,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=DCORP-DC,CN=Topology,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=AdminSDHolder,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=ComPartitions,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=ComPartitionSets,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=PolicyTemplate,CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=SOM,CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=PolicyType,CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=WMIGPO,CN=WMIPolicy,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6E157EDF-4E72-4052-A82A-EC3F91021A22,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=ab402345-d3c3-455d-9ff7-40268a1099b6,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=bab5f54d-06c8-48de-9b87-d78b796564e4,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=f3dd09dd-25e8-4f9c-85df-12d6d2f2f2f5,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=2416c60a-fe15-4d7a-a61e-dffd5df864d3,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=7868d4c8-ac41-4e05-b401-776280e8e9f1,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=860c36ed-5241-4c62-a18b-cf6ff9994173,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=0e660ea3-8a5e-4495-9ad7-ca1bd4638f9e,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=a86fe12a-0f62-4e2a-b271-d27f601f8182,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=d85c0bfd-094f-4cad-a2b5-82ac9268475d,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6ada9ff7-c9df-45c1-908e-9fef2fab008a,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=10b3ad2a-6883-4fa7-90fc-6377cbdc1b26,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=98de1d3e-6611-443b-8b4e-f4337f1ded0b,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=f607fd87-80cf-45e2-890b-6cf97ec0e284,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=9cac1f66-2167-47ad-a472-2a13251310e4,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6ff880d6-11e7-4ed1-a20f-aac45da48650,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=446f24ea-cfd5-4c52-8346-96e170bcb912,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=51cba88b-99cf-4e16-bef2-c427b38d0767,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=a3dac986-80e7-4e59-a059-54cb1ab43cb9,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=293f0798-ea5c-4455-9f5d-45f33a30703b,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=5c82b233-75fc-41b3-ac71-c69592e6bf15,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=7ffef925-405b-440a-8d58-35e8cd6e98c3,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=4dfbb973-8a62-4310-a90c-776e00f83222,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=8437C3D8-7689-4200-BF38-79E4AC33DFA0,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=7cfb016c-4f87-4406-8166-bd9df943947f,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=f7ed4553-d82b-49ef-a839-2f38a36bb069,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=8ca38317-13a4-4bd4-806f-ebed6acb5d0c,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=3c784009-1f57-4e2a-9b04-6915c9e71961,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd5678-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd5679-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd567a-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd567b-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd567c-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd567d-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd567e-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd567f-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd5680-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd5681-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd5682-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd5683-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd5684-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd5685-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd5686-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd5687-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd5688-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd5689-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd568a-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd568b-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd568c-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=6bcd568d-8314-11d6-977b-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=3051c66f-b332-4a73-9a20-2d6a7d6e6a1c,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=3e4f4182-ac5d-4378-b760-0eab2de593e2,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=c4f17608-e611-11d6-9793-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=13d15cf0-e6c8-11d6-9793-00c04f613221,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=8ddf6913-1c7b-4c59-a5af-b9ca3b3d2c4c,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=dda1d01d-4bd7-4c49-a184-46f9241b560e,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=a1789bfb-e0a2-4739-8cc0-e77d892d080a,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=61b34cb0-55ee-4be9-b595-97810b92b017,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=57428d75-bef7-43e1-938b-2e749f5a8d56,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=ebad865a-d649-416f-9922-456b53bbb5b8,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=0b7fb422-3609-4587-8c2e-94b10f67d1bf,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=2951353e-d102-4ea5-906c-54247eeec741,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=71482d49-8870-4cb3-a438-b6fc9ec35d70,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=aed72870-bf16-4788-8ac7-22299c8207f1,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=f58300d1-b71a-4DB6-88a1-a8b9538beaca,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=231fb90b-c92a-40c9-9379-bacfc313a3e3,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=4aaabc3a-c416-4b9c-a6bb-4b453ab1c1f0,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=9738c400-7795-4d6e-b19d-c16cd6486166,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=de10d491-909f-4fb0-9abb-4b7865c0fe80,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=b96ed344-545a-4172-aa0c-68118202f125,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=4c93ad42-178a-4275-8600-16811d28f3aa,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=c88227bc-fcca-4b58-8d8a-cd3d64528a02,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=5e1574f6-55df-493e-a671-aaeffca6a100,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=d262aae8-41f7-48ed-9f35-56bbb677573d,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=82112ba0-7e4c-4a44-89d9-d46c9612bf91,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=c3c927a6-cc1d-47c0-966b-be8f9b63d991,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=54afcfb9-637a-4251-9f47-4d50e7021211,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=f4728883-84dd-483c-9897-274f2ebcf11e,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=83C53DA7-427E-47A4-A07A-A324598B88F7,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=C81FC9CC-0130-4FD1-B272-634D74818133,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=E5F9E791-D96D-4FC9-93C9-D53E1DC439BA,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=e6d5fd00-385d-4e65-b02d-9da3493ed850,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=3a6b3fbf-3168-4312-a10d-dd5b3393952d,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=7F950403-0AB3-47F9-9730-5D7B0269F9BD,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=434bb40d-dbc9-4fe7-81d4-d57229f7b080,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=A0C238BA-9E30-4EE6-80A6-43F731E9A5CD,CN=Operations,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows2003Update,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=ActiveDirectoryUpdate,CN=DomainUpdates,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=us.dollarcorp.moneycorp.local,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=PSPs,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
CN=LostAndFound,DC=dollarcorp,DC=moneycorp,DC=local
CN=Infrastructure,DC=dollarcorp,DC=moneycorp,DC=local
CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local
CN=S-1-5-17,CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local
CN=S-1-5-9,CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local
CN=S-1-5-4,CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local
CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=dollarcorp,DC=moneycorp,DC=local
CN=Program Data,DC=dollarcorp,DC=moneycorp,DC=local
CN=Microsoft,CN=Program Data,DC=dollarcorp,DC=moneycorp,DC=local
CN=Managed Service Accounts,DC=dollarcorp,DC=moneycorp,DC=local
CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Guests,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Print Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Backup Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Replicator,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Remote Desktop Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Network Configuration Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Performance Monitor Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Performance Log Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Distributed COM Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=IIS_IUSRS,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Cryptographic Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Event Log Readers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Certificate Service DCOM Access,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=RDS Remote Access Servers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=RDS Endpoint Servers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=RDS Management Servers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Hyper-V Administrators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Access Control Assistance Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Remote Management Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Storage Replica Administrators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Server Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Account Operators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Pre-Windows 2000 Compatible Access,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Windows Authorization Access Group,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Terminal Server License Servers,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Administrators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
CN=Users,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local
PS C:\Users\student162>
```
