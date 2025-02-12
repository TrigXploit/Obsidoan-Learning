having issues with the redirect endpoint in azure app, so i installed at setup a public endpoint via. ngrok.com.

i found the solution was with the script i made yesterday.
it was calling `$request.Request.QueryString`. but i should have been calling `$request.QueryString` instead.


Spent most of the Day finishing the script I started working on [[noter for praktik/uge 5/Dag 2#test 2 Redirecting to external url, grab the auth_code, redirect back to app|Dag 2]]




