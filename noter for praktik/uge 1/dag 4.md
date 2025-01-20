startede dagen med at kigge på Tryhackme rummet fra i går


fra 10.30 lavede jeg på Rescan x rapporten.

##### SSL/TLS FREAK sårbarhed
virksomheden var blevet flagget for at understøtte EXPORT RSA på max 512 bits.
Det kan gøre den sårbar for [[FREAK]] angreb

##### Selv signeret certifikat 
kunden havde en IP hvor de selv have sineret certifikatet 

##### Svage krypteringscifre understøttet
Evidence fra `sslscan IP:PORT`

##### TLS version 1.0 & 1.1 understøttet 
Evidence fra `sslscan IP:PORT`

Virksomheden understøttede både [[TLS 1.0 & 1.1]] som har flere kendte sårbarheder.
==**ask Sofus hvordan man kan udnytte**==

#####  Udløbet certifikat 


##### Ukrypteret Basic Autentifikation 
Brug wireshark og se at trafikken er ukrypteret på port 5000

##### Web Server Generic Cookie Injection 
==**ask Sofus**==

##### Web server detaljerede fejlmeddelelser


##### Web server intern IP læk 
```http request
GET /autodiscover/autodiscover.xml HTTP/1.0

Accept-Charset: iso-8859-1,utf-8;q=0.9,*;q=0.1

Accept-Language: en

User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.0)

Pragma: no-cache

Accept: image/gif, image/x-xbitmap, image/jpeg, image/pjpeg, image/png, */*

Connection: keep-alive
```

##### Web.config konfigurationsfil tilgængelig 
```http request
GET /web.config HTTP/1.1

Host: example.dk

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate, br

Connection: keep-alive

Upgrade-Insecure-Requests: 1

Priority: u=0, i
```

##### Åbne biblioteker 


tilføjede al evidence til ReScan x rapporten 