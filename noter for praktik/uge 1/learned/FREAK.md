En Detaljeret forklaring a Freak kan findes [her](https://thehackernews.com/2015/03/freak-openssl-vulnerability.html)

### checking for FREAK   
brug kommandoen:
```
nmap -Pn -sV --script ssl-enum-ciphers -p 443 <HOST>
```
hvis et at resultaterne har `TLS_RSA_EXPORT` er den vulnerable for FREAK angreb

Eksempel på resultat
``` bash
TLS_RSA_EXPORT1024_WITH_RC4_56_SHA (rsa 64) - E

TLS_RSA_EXPORT_WITH_RC4_40_MD5 (rsa 64) - E

TLS_RSA_EXPORT_WITH_DES40_CBC_SHA (rsa 64) - E

TLS_RSA_EXPORT_WITH_RC2_CBC_40_MD5 (rsa 64) - E

TLS_RSA_EXPORT1024_WITH_RC4_56_MD5 (rsa 64) - E

TLS_RSA_EXPORT1024_WITH_RC2_CBC_56_MD5 (rsa 64) - E
```

### Using FREAK (theory)



### Hvordan virker FREAK