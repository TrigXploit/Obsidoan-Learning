 brugte dagen på at skrive en guide på hvordan man skaffer en accesstoken samt hvordan man tilføjer scopes / permissions til en Service probider.

Brugte resten af dagen på at kigge på siden https://aadinternals.com/aadinternals/
Det er et "Microsoft 365 hacking and admin toolkit"

set up the aadInternals and started playing around.
it looks like is a giant library of function calls but should make it a lot easier to break Entra ID

i used
```bash
$token = New-AADIntServicePrincipalLogin -TenantID "<TENANT_ID>" -ClientID "<CLIENT_ID>" -ClientSecret "<CLIENT_SECRET>"
```
to login as the SP
and 