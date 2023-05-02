# CRTP: Hands-On 4

```
  Enumerate all domains in the moneycorp.local forest.
    - Map the trusts of the dollarcorp.moneycorp.local domain.
    - Map External trusts in moneycorp.local forest.
    - Identify external trusts of dollarcorp domain. Can you enumerate trusts for a trusting forest?
```

Index:
  
  1. [Trust Domains dollarcorp.moneycorp.local](#trust-domains-dollarcorp.moneycorp.local)
  2. [External Trust moneycorp.local](#external-trust-moneycorp.local)
  3. [External Trust of Trust](#external-trust-of-trust)


## Trust Domains dollarcorp.moneycorp.local
```
Get-ADTrust -Filter * | select Direction, Name, Source, Target, TGTDelegation | FT

    Direction Name                          Source                              Target                        TGTDelegation
    --------- ----                          ------                              ------                        -------------
BiDirectional moneycorp.local               DC=dollarcorp,DC=moneycorp,DC=local moneycorp.local                       False
BiDirectional us.dollarcorp.moneycorp.local DC=dollarcorp,DC=moneycorp,DC=local us.dollarcorp.moneycorp.local         False
BiDirectional eurocorp.local                DC=dollarcorp,DC=moneycorp,DC=local eurocorp.local                        False

```
```
 Get-DomainTrust | FT

SourceName                 TargetName                    TrustType                TrustAttributes TrustDirection WhenCreated           WhenChanged
----------                 ----------                    ---------                --------------- -------------- -----------           -----------
dollarcorp.moneycorp.local moneycorp.local               WINDOWS_ACTIVE_DIRECTORY WITHIN_FOREST   Bidirectional  11/12/2022 5:59:01 AM 4/30/2023 4:02:45 AM
dollarcorp.moneycorp.local us.dollarcorp.moneycorp.local WINDOWS_ACTIVE_DIRECTORY WITHIN_FOREST   Bidirectional  11/12/2022 6:22:51 AM 4/30/2023 4:03:03 AM
dollarcorp.moneycorp.local eurocorp.local                WINDOWS_ACTIVE_DIRECTORY FILTER_SIDS     Bidirectional  11/12/2022 8:15:23 AM 4/30/2023 4:02:17 AM

```
## External Trust moneycorp.local

```
 Get-ADTrust -Filter * -Server moneycorp.local | select Direction, Name, Source, Target, TGTDelegation | FT

    Direction Name                       Source                Target                     TGTDelegation
    --------- ----                       ------                ------                     -------------
BiDirectional dollarcorp.moneycorp.local DC=moneycorp,DC=local dollarcorp.moneycorp.local         False


```

## External Trust of Trust

```
PS C:\Users\student162> Get-ADTrust -Filter * -Server us.dollarcorp.moneycorp.local | select Direction, Name, Source, Target, TGTDelegation | FT

    Direction Name                       Source                                    Target                     TGTDelegation
    --------- ----                       ------                                    ------                     -------------
BiDirectional dollarcorp.moneycorp.local DC=us,DC=dollarcorp,DC=moneycorp,DC=local dollarcorp.moneycorp.local         False


PS C:\Users\student162> Get-ADTrust -Filter * -Server eurocorp.local | select Direction, Name, Source, Target, TGTDelegation | FT

    Direction Name                       Source               Target                     TGTDelegation
    --------- ----                       ------               ------                     -------------
BiDirectional eu.eurocorp.local          DC=eurocorp,DC=local eu.eurocorp.local                  False
BiDirectional dollarcorp.moneycorp.local DC=eurocorp,DC=local dollarcorp.moneycorp.local         False


```
