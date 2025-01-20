blev vist hvordan man laver en 'Rescan x' hvilket er deres version af en sikkerheds rapport

#####  ICMP Timestamp Informationslæk 
Jeg fik lov til at teste og tilføje et eksembel af en server som lækkede IMCP timestamps. (ICMP fortæller hvad klokken er ***præcist*** så det kan bruges til kordinering af angreb)


##### IP læk via Exchange server 
brugte Burpsuite til at finde IP læk via en Exchange Server. Hvor jeg for første gang støtte på [[autodiscover.xml]]
```
GET /autodiscover/autodiscover.xml HTTP/1.0
```
Men det tog lidt tid da jeg ikke vidste at man skulle tilføje nogle ekstra newlines til pakken.
