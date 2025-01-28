## Pentest af ældresagen  
Jeg lavede Test på Ældresagen, arrangemant system
Vi blev givet brugere på 5 forskellige niveauer
jeg startede med at teste på en niveaue 1 burger (laveste) 

Hvor jeg fandt 2 måder at exploite deres system på ved brug af burp.

### level 1 user
#### Problem 1 
det var muligt at få andre brugers information, og ændre i den, uanset område.
en lv 1 bruger i amager2300 kunne se en brugers info, i Østerbro:
Adresse (vej hus-nr + by) 
samt kunne ændre en brugers email, tlf eller mobil.

#### Problem 2
det var muligt at skifte fra 1 kommune til en anden og se arrangemanger som foregik i den kommune.
det var dog ikke muligt at se detaljer eller lave CRUD operationer i de Arrangemanger.

### level 2 user
same as lv 1
