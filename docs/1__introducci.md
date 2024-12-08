# 1 - Introducció

En el tema anterior vam tenir el primer contacte amb les Bases de Dades NoSQL. En
aquella ocasió es tractava de Bases de Dades Orientades a objectes, que ens
permetien guardar els objectes de forma còmoda, i sense desfasament objecte-
relacional, ja que el que guardàvem era directament objectes. Per contra,
se'ns complicava la manera de fer consultes, ja que no disposàvem d'un
llenguatge tan potent com el SQL.

El terme **NoSQL** (Not Only SQL) és més extens. Són en definitiva Bases de Dades
que no estan basades en el Model Relacional, i que en determinades ocasions
poden ser més eficients que les Bases de Dades Relacionals precisament per
fugir de la rigidesa que proporciona aquest model, amb dades tan ben
estructurades. El terme clau és "en determinades ocasions", és a dir, per a
guardar un determinat tipus d'informació. Així ens trobarem distints tipus de
Bases de Dades NoSQL, depenent del tipus d'informació que vulguen guardar:

  * **Bases de Dades Orientades a Objectes** , per a poder guardar objectes, com per exemple **DB4O**
  * **Bases de Dades Orientades a Documents** (o simplement Bases de Dades Documentals), per a guardar documents de determinats tipus: XML, JSON, ... Per exemple **eXist** que guarda documents XML, o **MongoDB** que guarda la informació en un format similar a JSON. O **Firebase** , una Base de Dades que utilitza també el format JSON i que en temps real permet sincronitzar amb el núvol les dades locals.
  * **Bases de Dades Clau-Valor** , per a guardar informació de forma molt senzilla, guardant únicament la clau (el nom de la propietat) i el seu valor
  * **Bases de Dades Orientades a Grafs** , per a guardar estructures com els grafs, on hi ha una sèrie de nodes i arestes que comunicarien (relacionarien) els nodes
  * **Bases de Dades Orientades a Columna** , que té una estructura pareguda a les taules del Model Relacional, però orientat a les columnes (o famílies de columnes, de manera que un grup de columnes es guarda en el mateix lloc). Un exemple és **Cassandra**
  * Altres tipus com les **Bases de Dades Multivalor** , les **Bases de Dades Tabulars** , ...

En aquest tema només veurem les Bases de Dades Clau-Valor, per la seua
senzillesa, i una altra Base de Dades orientada a documents, **MongoDB** , per
la seua utilització actual. Com que ja hem vist una Orientada a Objectes.



Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
SenseObraDerivada 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)

