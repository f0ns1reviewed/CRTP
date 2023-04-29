CRTP: Hands-On 16

```
• Enumerate users in the domain for whom Constrained Delegation is enabled.
  – For such a user, request a TGT from the DC and obtain a TGS for the service to which delegation is configured.
  – Pass the ticket and access the service as DA.
• Enumerate computer accounts in the domain for which Constrained Delegation is enabled.
  – For such a user, request a TGT from the DC.
  – Use the TGS for executing the DCSync attack.
```

