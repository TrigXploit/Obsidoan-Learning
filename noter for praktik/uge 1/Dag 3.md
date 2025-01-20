*Mødte sent da DSB havde problemer med toget så mødte in lidt over 10.*

##### IP læk via PROPFIND
startede med at arbejde videre på ReScan X opgaven hvor jeg lavede evidence for iplæk via- [[PropFind]].

----------------------------
### Pentest møde kl 10:30 med Jonatan
hører hvordan det går med folk og hvad folk arbejder på og om folk mangler hjælp med noget.

en hjemmeside har problemer med at de sender access token i deres URL
samt han er igang med at lave et xxs script hvor han kan få billeder ud af en safeboks.


##### Retest Forsat - medium stærk kryptering 
tilføjede og lærte om

`sslscan IP:PORT`
`nmap -Pn -sV --script ssl-enum-ciphers -p 443 <HOST>`

3DES med 64 bits
der er en vulnerability kaldet [[SWEET32]]


#### Manglende HTTP Strict Transport Security header

```bash
nmap -Pn -p <port> --script http-security-headers <target> ip/url
```


#### Medium stærke krypteringscifre understøttet

```bash
sslscan IP:PORT (virker nogle gange)
nmap -Pn -sV --script ssl-enum-ciphers -p 443 <HOST>
```







Kl 16 begyndte vi på "nerd wednesday" som er en dag hvor ansatte & jeg kan sidde sammen efter arbejde og lave CTF'er eller præsentere nye teknologier eller på andre måder forbedre hinanden.

Jeg sad med Peter og lavede på et room fra HackTheBox og senere et andet rum fra TryHackMe.

