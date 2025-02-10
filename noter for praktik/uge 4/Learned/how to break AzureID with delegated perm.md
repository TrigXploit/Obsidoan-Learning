Connecting via 

## creating a fake app



### making a user authenticate
a user has to 


### make sure the workflow is correct
from Entra ID
- select app registrations
- select your app
	- and go into authentication
		- under platform configurations,

#### commands
Get Access Token (not working)
```bash
curl -X POST https://login.microsoftonline.com/5dc7e152-c906-4d95-bd1a-bcf75eb3e939/oauth2/v2.0/token \ 
-H "Content-Type: application/x-www-form-urlencoded" \ 
-d "client_id=675c27b6-e1ff-4a2c-924e-701186f97951" \ 
-d "client_secret=eV68Q~WEECCIzSJKzATfkd2rly.Kxds1SFzNDbyg" \ 
-d "scope=https://graph.microsoft.com/.default" \ 
-d "grant_type=client_credentials"
```

update user (example)
```bash
curl -X PATCH https://graph.microsoft.com/v1.0/users/<testUserId> \
  -H "Authorization: Bearer <access_token>" \
  -H "Content-Type: application/json" \
  -d '{"jobTitle": "flag"}'
```


Verify the Update
```bash
curl -X PATCH https://graph.microsoft.com/v1.0/users/<testUserId> \
  -H "Authorization: Bearer <access_token>"
```




`https://login.microsoftonline.com/5dc7e152-c906-4d95-bd1a-bcf75eb3e939/reprocess?ctx=rQQIARAA02I20jOwUjEzN002Mk8y0001TEvTNUk0Sta1NDJJ1TU3MDS0MEuzNLc0NcxgLBLiEuDcr1-1JU7Dc4YMo9WExT1OqxhdMkpKCoqt9PVzE3MykzPzS4t1y1OTdBMLCvQSq0qLUoGc4syS1GK9vNQSfb3E0pIM_Zz89Mw8_cTEFP3kxJycpMTk7B2MjBcYGV8wMt5i4vd3BKoxAhH5RZlVqa-Y2JPz84pT80o-MbEWlqYWVa5iVjGAAGNdEAkhkmEsGNjErGKakmyeamhqpJtsaWCma5JiaaqblGKYqJuUnGZumppknGppbHmKmS-0OLVILyg1MSW8COjSG8yMF1gYX7HwGDBbcXBwCTBIMCgw_GBhXMQK9H_zHLfjphHT3Tpt1b62NocynGLVL87PSc83zkzNTMvwzgot0Hd1dvdzDzL0KvMy8C1wNA43csvOS_dxrwrzzLe1sDKcwMb4gY2xg51hFydVgu4AL8MPvh-9z9o_vL_4zuMVv45-saFbQVaAvraxV0Zwip-lSUFQYJqZeWFkiXtETraFq0GRZ7FvinaVe2Kk7QYBhgcCDAA1&sessionid=001f76c9-d47c-1a6c-a177-910c4e07ec2d`