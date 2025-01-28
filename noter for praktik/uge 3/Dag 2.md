begyndt at prøve SQLi og de fleste sidder håndterer det pænt og giver en generisk fejlbesked.
men deres login reagerer underligt på `'or 1=1--`  men reagerer ikke på `'--`

>viste sig at de brugte No-SQL 
>prøvede at injecte noget No-SQLi men login formen ser ud til at sanatize det.

blev givet en opgave med at kigge på en jquery 1.9.0 som er forældet hvilket resulterer i 
- CVE 2015-9251 

#### problem 2 forlænget
Det er muligt at opsnappe en get request
```http
GET /api/Person/GetPerson?id=269697&navId=30514213
```
som giver en masse data på en specifik user 

der udover var det muligt at kalde
```http
POST /api/Person/UpdatePerson
```
men man kan ikke lave nogle CRUD handlinger




**påbegyndt eJPT certificering** [[Index|eJPT Index]]

