# 1 - Introducció  

**Què és NoSQL?**{.azul}

El terme **NoSQL** (*Not Only SQL*) fa referència a bases de dades que no segueixen el model relacional. En alguns casos, poden ser més eficients perquè permeten una estructura de dades més flexible.  

✅ **Avantatge:** Més adaptabilitat en escenaris on la informació no encaixa bé en taules relacionals.  
❌ **Consideració:** NoSQL no sempre és la millor opció; la seva eficàcia depèn del tipus d'informació que es vulga gestionar.  

---

**Tipus de Bases de Dades NoSQL**{.azul}

Segons la forma en què s'emmagatzemen les dades, podem trobar diferents categories:  

  * **Bases de Dades Orientades a Objectes** , per a poder guardar objectes, com per exemple **DB4O**
  * **Bases de Dades Orientades a Documents** (o simplement Bases de Dades Documentals), per a guardar documents de determinats tipus: XML, JSON, ... Per exemple **eXist** que guarda documents XML, o **MongoDB** que guarda la informació en un format similar a JSON. O **Firebase** , una Base de Dades que utilitza també el format JSON i que en temps real permet sincronitzar amb el núvol les dades locals.
  * **Bases de Dades Clau-Valor** , per a guardar informació de forma molt senzilla, guardant únicament la clau (el nom de la propietat) i el seu valor
  * **Bases de Dades Orientades a Grafs** , per a guardar estructures com els grafs, on hi ha una sèrie de nodes i arestes que comunicarien (relacionarien) els nodes
  * **Bases de Dades Orientades a Columna** , que té una estructura pareguda a les taules del Model Relacional, però orientat a les columnes (o famílies de columnes, de manera que un grup de columnes es guarda en el mateix lloc). Un exemple és **Cassandra**
  * Altres tipus com les **Bases de Dades Multivalor** , les **Bases de Dades Tabulars** , ...


En aquest tema ens centrarem en:

🔹 **Bases de Dades Clau-Valor**, per la seva senzillesa.  
🔹 **MongoDB**, una base de dades documental àmpliament utilitzada.



Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
SenseObraDerivada 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)

