# 3 - MongoDB

Segurament **MongoDB** és el més famós dels Sistemes Gestors de Bases de Dades
**NoSQL**.
 
El nom de **MongoDB** prové de la paraula anglesa _hu **mongo** us_, que
significa enorme, que és el propòsit d'aquesta Base de Dades: guardar grans
quantitats d'informació. És de codi obert i està programada en C++. El va
crear l'empresa **10gen** (actualment **MongoDB Inc.**)

És un SGBD **Documental** , és a dir, que servirà per a guardar documents. La
manera interna de guardar-los és en format **BSON** (Binary JSON) que en
essència és una variant del JSON per a poder guardar físicament les dades
d'una manera més eficient.

En un servidor Mongo poden haver més d'una Base de Dades, encara que nosaltres
només en gastarem una: **test**.

  * En cada Base de Dades la informació es guardarà en **col·leccions**.
  * Cada col·lecció constarà d'uns quants **documents**.
  * I cada document seran una sèrie de dades guardades en forma de **clau-valor** , dels tipus suportats per MongoDB, i amb el format JSON (en realitat BSON)

Per tant, en Mongo no hi ha taules. Mirem uns exemples de documents JSON per a
guardar la informació de llibres i autors. Depenen de com s'haja d'accedir a
la informació ens podem plantejar guardar els llibres amb els seus autors, o
guardar els autors, amb els seus llibres. Fins i tot ens podríem guardar els
dos, per a poder accedir de totes les maneres, encara que és a costa de doblar
la informació.

De la primera manera, guardant els llibres amb el seu autor, podríem tenir
documents amb aquesta estructura, que es podrien guardar en una col·lecció
anomenada **Llibres** :
```
  {  
    _id:101,  
    titol:"El secret de Khadrell",  
    autor: {  
    nom:"Pep",  
    cognoms:"Castellano Puchol",  
    any_naixement:1960  
    },  
    isbn:"84-95620-72-3"  
  },  
  {  
    _id:102,  
    titol:"L'Ombra del Vent",  
    autor: {  
    nom:"Carlos",  
    cognoms:"Ruiz Zafon",  
    pais:"Espanya"  
    },  
    pagines:490,  
    editorial:"Planeta"  
  }
```
Observeu com els objectes no tenen per què tenir la mateixa estructura. La
manera d'accedir al nom d'un autor seria aquesta: _objecte.autor.nom_

Una manera alternativa de guardar la informació, com havíem comentat abans
seria organitzar per autors, amb els seus llibres. D'aquesta manera podríem
anar omplint la col·lecció **Autors** amb un o més documents d'aquest estil:
```json
  {  
      _id: 201,  
      nom:"Pep",  
      cognoms:"Castellano Puchol",  
      any_naixement:1960,  
      llibres: [  
      {  
        titol:"El secret de Khadrell",  
        isbn:"84-95620-72-3"  
      },  
      {  
        titol:"Habitació 502",  
        editorial:"Tabarca"  
      }  
    ]  
  },  
  {  
      _id:202,  
      nom:"Carlos",  
      cognoms:"Ruiz Zafon",  
      pais:"Espanya",  
      llibres: [  
        {  
          titol:"L'Ombra del Vent",  
          pagines:490,  
          editorial:"Planeta"  
        }  
    ]  
  }
```
Observeu com per a un autor, ara tenim un array ( els claudàtors: **[ ]**) amb
els seus llibres.

Quina de les dues maneres és millor per a guardar la informació? Doncs depén
de l'accés que s'haja de fer a les dades. La millor serà segurament aquella
que depenent dels accessos que s'hagen de fer, torne la informació de forma
més ràpida.

## 3.1 - Instal·lació de MongoDB

Podrem instal·lar MongoDB en qualsevol plataforma. I fins i tot sense tenir
permisos d'administrador, com veurem en el cas d'Ubuntu.

També està la possibilitat de crear un servidor en el núvol, fins i tot
gratuït. És l'opció que ens suggereix Mongo per defecte, però nosaltres no la
utilitzarem.

### Instal·lació en Linux

Per a poder fer la instal·lació més bàsica, podrem fer-lo sense permisos
d'administrador. Si els tenim tot és més còmode, però si no en tenim també ho
podem fer, com veurem i remarcarem a continuació.

**Instal·lació del servidor**{.azul}

De la pàgina de **MongoDB (<https://www.mongodb.com/try/download/community>)** anem al menú Products - > Comunity Edition ->Comunity Server
i ens baixem la versió apropiada per al nostre Sistema Operatiu. Observeu com en el cas de Linux hi ha moltes versions, per a moltes distribucions. I millor triar
en el paquet el **tgz** , ja que amb descomprimir el fitxer serà suficient. En
el cas d'**Ubuntu 22.04 de 64 bits** , aquest fitxer és:
**<https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu2204-8.0.4.tgz>**. Però recordeu que us heu d'assegurar de la versió.

Senzillament descomprimirem aquest fitxer on vulguem, i ja estarà feta la
instal·lació bàsica.


Per defecte el directori de la Base de Dades és **/data/db**

L'únic problema que podríem tenir si no som administradors és que no tinguem
permís per crear aquest directori. Aleshores crearem un altre directori i en
el moment d'arrancar el servidor, li especificarem aquest lloc.

La manera d'arrancar el servidor serà:
```
<dirctori arrel MongoDB>/bin/mongod
```
Opcionalment li podem dir on està la Base de Dades (si no ho especifiquem
assumirà que està en **/data/db**):
```
<directori arrel MongoDB>/bin/mongod --dbpath <directori de la BD>
```
Resumint, i estant situats al directori on hem descomprimit MongoDB:

* Si som administradors: 
  >* Creem el directori de dades:
```
mkdir /data  
mkdir /data/db
```
  > * Arranquem el servidor:
  
```
./bin/mongod
```
* Si no som administradors: 
  >* Creem el directori de dades:
```
mkdir data  
mkdir data/db
```
  > * Arranquem el servidor:
```
./bin/mongod --dbpath ./data/db
```
La següent imatge il·lustra aquesta segona opció. Està per a una versió
anterior de MongoDB, però és totalment equivalent:

![](T8_3_1_1.png)

!!!note "Nota"
  Una vegada en marxa el servidor, no hem de tancar aquesta terminal, ja que
  pararíem el servidor.


**Instal·lació del client**{.azul}

De la pàgina de **MongoDB (<https://www.mongodb.com/try/download/shell>)** anem al menú Products - > Tools ->MongoDB Shell
i ens baixem la versió apropiada per al nostre Sistema Operatiu. Observeu com en el cas de Linux hi ha moltes versions, per a moltes distribucions. I millor triar
en el paquet el **tgz** , ja que amb descomprimir el fitxer serà suficient. En
el cas d'**Ubuntu 22.04 de 64 bits** , triarem l'opió genèrica **Linux 64** ja que és la opció que té el paquet **tgz**, i aquest fitxer és:
**<https://downloads.mongodb.com/compass/mongosh-2.3.6-linux-x64.tgz>**. Però recordeu que us heu d'assegurar de la versió.


Per a connectar un client, obrim una segona terminal i
executem el client **mongosh** :
```
./bin/mongosh
```
![](T8_3_1_2.png)

Per a provar el seu funcionament, anem a fer un parell de comandos: un per a
guardar un document i un altre per a recuperar-lo.

Per a qualsevol operació s'ha de posar **db** seguit del nom de la col·lecció,
i després l'operació que volem fer. Amb el següent:
```
> db.exemple.Insert( {msg:"Hola, què tal?"} )
```
Ens contestarà:
```
WriteResult({ "nInserted" : 1 })
```
Indicant que ha inserit un document en la col·lecció **exemple** (si no estava
creada, la crearà).

I amb el següent comando recuperem la informació:
```
> db.exemple.findOne()
```
Que ens tornarà:
```
{ "_id" : ObjectId("56cc130590d651d45ef3d3be"), "msg" : "Hola, què tal?" }
```
Tot ho fa en la mateixa terminal, i a cadascú de nosaltres ens donarà un
número diferent en **ObjectId**. En la següent imatge es veuen les dues
operacions:

![](T8_3_1_3.png)

En realitat estem connectats a una Base de Dades anomenada **test**. Podem
crear i utilitzar més d'una Base de Dades, però en aquest curs tindrém més que
suficient amb aquesta Base de Dades. Per a comprovar-ho podem executar la
següent sentència, que ens torna el nom de la Base de Dades:
```
> db.getName()  
test
```
### Instal·lació en Windows

No ofereix cap dificultat. Ens baixem la versió apropiada de MongoDB per a
Windows, depenent de si és de 32 o 64 bits la nostra versió, que resultarà ser
un .msi directament executable. En el moment de fer aquestos apunts, el de la
versió de 64 bits era el fitxer:

<https://www.mongodb.com/dr/fastdl.mongodb.org/win32/mongodb-win32-x86_64-2008plus-ssl-3.6.2-signed.msi/download>
<!--
[mongodb-win32-x86_64-2008plus-
ssl-3.6.2-signed.msi](https://www.mongodb.com/dr/fastdl.mongodb.org/win32/mongodb-
win32-x86_64-2008plus-ssl-3.6.2-signed.msi/download)
-->
Una vegada baixat, executem el fitxer. Haurem d'acceptar la llicència,
instal·lar la versió completa, i acceptar quan Windows ens avise que un
programa vol instal·lar software. El de sempre.

Com en el cas de Linux, abans d'executar el servidor haurem de tenir el
directori creat. Per defecte el directori serà **\data\db**

Aqueste serien les ordres per a crear el directori i després arrancar el
servidor. Està per a una versió anterior, però seria totalment equivalent:
```
mkdir \data\db  
C:\Program Files\MongoDB\Server\3.2\bin\mongod.exe
```
En aquest a imatge s'observa que en intentar posar en marxa el servidor, el
Firewall de Windows ho detecta, i sol·licita permís per posar-lo en marxa.
Acceptem i prou:

![](T8_3_1_11.png)

Per a connectar-nos com a clients, ho haurem de fer des d'una altra terminal,
ja que si tanquem aquesta pararem el servidor. El programa és **mongo.exe** :
```
C:\Program Files\MongoDB\Server\3.2\bin\mongo.exe
```
![](T8_3_1_12.png)

Per a provar el seu funcionament, anem a fer un parell de comandos: un per a
guardar un document i un altre per a reculerar-lo.

Per a qualsevol operació s'ha de posar **db** seguit del nom de la col·lecció,
i després l'operació que volem fer. Amb el sgüent:
```
> db.exemple.InsertOne( {msg:"Hola, què tal?"} )
```
Ens contestarà:
```
WriteResult({ "nInserted" : 1 })
```
Indicant que ha inserit un document en la col·lecció **exemple** (si no estava
creada, la crearà).

I amb el següent comando recuperem la informació:
```
> db.exemple.findOne()
```
Que ens tornarà:
```
{ "_id" : ObjectId("56cc1acd73b559230de8f71b"), "msg" : "Hola, què tal?" }
```
Tot ho fa en la mateixa terminal, i a cadascú de nosaltres ens donarà un
número diferent en **ObjectId**. En la següent imatge es veuen les dues
operacions:

![](T8_3_1_13.png)

En realitat estem connectats a una Base de Dades anomenada **test**. Podem
crear i utilitzar més d'una Base de Dades, però en aquest curs tindrés més que
suficient amb aquesta Base de Dades. Per a comprovar-ho podem executar la
següent sentència, que ens torna el nom de la Base de Dades:
```
> db.getName()  
test
```
### 3.1.1 - Connexió al servidor de l'Institut

Quan hem utilitzat el client, hem executat el programa **mongo** sense posar-
li res més. Per defecte s'ha connectat al servidor que tenim en la mateixa
màquina.

Però en realitat li podem especificar l'adreça on està el servidor al qual
volem connectar. En concret, l'adreça del servidor de dades de l'Institut en
el qual també tenim instal·lat MongoDB:

```    
    mongo  89.36.214.106
```
Podem connectar, però no podrem fer cap operació, perquè està habilitada
l'autenticació per a previndre atacs, cosa que no tenim en el servidor que hem
instal·lat cadascú en la seua màquina.

Aleshores, si intentem fer qualsevol operació, ens donarà error perquè no
estem autenticats:

![](T8_3_1_1_1.png)

La manera d'autenticar serà utilitzant el comando **db.auth("_usuari"_ ,
"_contrasenya"_)**. I en connectarem a un usuari que té permís per a utilitzar
la BD test:

  * Usuari: **ad**
  * Contrasenya: **ieselcaminas**

A partir d'eixe moment ja podrem utilitzar-lo sense problemes:

![](T8_3_1_1_2.png)

## 3.2 - Utilització de MongoDB

Començarem la utilització de MongoDB des de la consola que havíem arrancat al
final de la instal·lació.

Recordeu que tindrem dues teminals:

  * Una amb el servidor en marxa (i que no hem de tancar): **mongod**
  * Una altra amb el client que es connecta al servidor: **mongo**

En aquesta última consola del client podem utilitzar sentències del llenguatge
**Javascipt** , però el que més ens interessarà, evidentment, són les
sentències d'accés a dades. Del llenguatge Javascript pràcticament l'únic que
utilitzarem són variables i algunes funcions.

**Utilització de variables**{.azul}

Com comentàvem el que més utilitzarem del llenguatge **Javascript** és la
utilització de variables, que ens pot ser molt útil en algunes ocasions.
Podrem utilitzar-les durant la sessió, però evidentment no perduraran d'una
sessió a l'altra.

Per a definir una variable podem posar opcionalment davant la paraula
reservada **var** , però no és necessari. Posarem el nom de la variable, el
signe igual, i a continuació el valor de la variable, que pot ser una
constant, o una expressió utilitzant constants, operadors, altres variables,
funcions de Javascript, ...

Especialment interessant són les variables que poden contenir un document
JSON.

Per exemple:
```
  > a = 30  
  30  
  > b = a/4  
  7.5  
  > Math.sqrt(b)  
  2.7386127875258306  
  > doc = {camp1: "Hola", camp2: 45, camp3: new Date()}  
  {  
    "camp1" : "Hola",  
    "camp2" : 45,  
    "camp3" : ISODate("2022-01-16T18:07:51.118Z")  
  }  
  >
```
Una variable de tipus JSON es podrà modificar molt fàcilment, tota ella, o
algun dels elements. Per a arribar als elements posarem
**_nom_variable.nom_camp_** :
```
> doc.camp4 = 3.141592  
3.141592
```
```
> doc.camp5 = [ 2 , 4 , 6 , 8]  
[ 2, 4, 6, 8 ]
```
I si ara intentem traure el contingut de la variable:
```
> doc  
{  
  "camp1" : "Hola",  
  "camp2" : 45,  
  "camp3" : ISODate("2022-01-16T18:07:51.118Z"),  
  "camp4" : 3.141592,  
  "camp5" : [  
              2,  
              4,  
              6,  
              8  
    ]  
}  
>
```
També hem de fer constar que en un document, que serà de tipus JSON
(pràcticament), serà un conjunt de parelles clau-valor, amb algunes
restriccions:

  * El document (que moltes vegades l'associarem a objecte de JSON) va entre claus ( **{ }** )
  * Els elements d'un objecte van separats per comes, i són parelles clau-valor.
  * La clau no pot ser nula, ni repetir-se en el mateix objecte (sí en diferents objectes, clar)
  * Els valors són dels tipus que veurem en la pregunta 3.2.1
  * Un document guardat ha de contenir obligatòriament un camp anomenat **_id** , i que contindrà un valor únic en la col·lecció i servirà per a identificar-lo. Si en guardar un document no li hem posat camp **_id** , el generarà automàticament MongoDB.

### 3.2.1 - Tipus de dades

Els valors dels elements, és a dir de les parelles clau valor, poden ser d'uns
quants tipus. Fem un ràpid repàs.

En els exemples que van a continuació definim senzillament parelles clau-valor
dels diferents tipus, o en tot cas ens ho guardem en variables, però no
guardarem encara en la Base de Dades (ho farem en la següent pregunta).

Quan guardem en una variable es mostrarà el prompt, la definició de la
variable i després el resultat d'haver guardat la variable. Utilitzarem
requadres blancs. Els requadres grocs són únicament de la definició d'una
clau-valor d'un determinat tipus

**NULL**

Més que un tipus de dades és un valor, millor dit, l'absència de valor
```
{ "x" : null }
```
**BOOLEAN**

El tipus booleà, que pot agafar els valors true o false.
```
{ "x" : true }

{ "y" : false }
```
**NUMBER**

Per defecte, el tiups de dades numèrics serà el de coma flotant (**float**),
simple precisió. Si volem un altre tiups (enter, doble precisió, ...) ho
haurem d'indicar expressament. Així els dos següents valors són float:
```
{ "x" : 3.14 }

{ "y" : 3 }
```
Si volem que siga estrictament enter, per exemple, haurem d'utilitzar una
funció de conversió:
```
{ "x" : NumberDouble("3.14") }

{ "y" : NumberInt("3") }
```
**STRING**

Es pot guardar qualsevol cadena amb caràcters de la codificació UTF-8
```
{ x : "Hola, què tal?"}
```
**DATE**

Es guarda data i hora, i internament es guarden en milisegons des de l'any
inicial. No es guarda el ****_Time zone_ , és a dir,__ la desviació respecte a
l'hora internacional.
```
{ x : ISODate("2022-01-16T11:15:27.471Z") }
```
Normalment utilitzarem funcions de tractament de la data-hora. L'anterior era
per a convertir el string en data-hora. La següent és per a obtenir la data-
hora actual:
```
{ x : new Date() }
```
És a dir, que si no posem paràmetre, ens dóna la data-hora actual. Però li
podem posar com a paràmetre la data-hora que volem que genere. En aquest
exemple, només posem data, per tant l'hora serà les 00:00:
```
> z = new Date("2022-01-16")  
ISODate("2022-01-16T00:00:00Z")
```
En aquest sí que posem una determinada hora, i observeu com hem deposar la T
(Time) entre el dia i l'hora:
```
> z = new Date("2022-02-16T18:00")  
ISODate("2022-01-16T18:00:00Z")
```
És molt important que posem sempre **New Date()** per a generar una data-hora.
Si posem únicament **Date()** , el que estem generant és un string (segurament
amb la data i hora actual, però un string):
```
> z = Date("2022-01-16")  
Sun Jan 16 2022 22:20:09 GMT+0100 (CET)
```
**ARRAY**

És un conjunt d'elements, cadascun de qualsevol tipus, encara que el més
habitual és que siguen del mateix tipus. Van entre claudàtors (**[ ]**) i els
elements separats per comes.
```
{ x : [ 2 , 4 , 6 , 8 ] }
```
Com comentàvem, cada element de l'array pot ser de qualsevol tipus:
```
{ y : [ 2 , 3.14 , "Hola" , new Date() ] }
```
En MongoDB podrem treballar molt bé amb arrays, i tindrem operacions per a
poder buscar dins de l'array, modificar un element, crear índex, ...

**DOCUMENTS (OBJECTES)**

Els documents poden contenir com a elements uns altres documents (**objectes**
en la terminologia JSON, però **documents** en la terminologia de MongoDB).

Van entre claus ( **{ }** ), i els elements que contindran van separats per
comes i seran parelles clau-valor de qualsevol tipus (fins i tot altres
documents).
```
{ x : { a : 1 , b : 2 } }
```
Posar documents dins d'uns altres documents (el que s'anomena _embedded_
_document_) ens permet guardar la informació d'una manera més real, no tan
plana. Així per exemple, les dades d'una persona les podríem definir de la
següent manera. Les posarem en una variable, per veure després com podem
accedir als diferents elements, encara que el més normal serà guardar-lo en la
Base de Dades (amb **insert()**). Si copiem el que va a
continuació al terminal de Mongo, ens apareixerà amb un format estrany. És
perquè la sentència d'assignació a la variable ocupa més d'una línia, i
apareixeran 3 punts al principi per a indicar que continua la sentència. Però
funcionarà perfectament :

```json
doc = {  
  nom:"Joan Martí",  
  adreça: {  
  carrer:"Major",  
  número:1,  
  població:"Castelló"  
  } ,  
  telèfons : [964223344,678345123]  
}
```
Observeu com aquesta estructura que ha quedat tan clara, segurament en una
Base de Dades Relacional ens hauria tocat guardar en 3 taules: la de persones,
la d'adreces i la de telèfons.

Per a accedir als elements d'un document posàvem el punt. Doncs el mateix per
als elements d'un document dins d'un document. I també podem accedir als
elements d'un array, posant l'índex entre claudàtors.
```
  > doc.nom  
  Joan Martí  
    
  > doc.adreça  
  { "carrer" : "Major", "número" : 1, "població" : "Castelló" }  
    
  > doc.adreça.carrer  
  Major  
    
  > doc.telèfons  
  [ 964223344, 678345123 ]  
    
  > doc.telèfons[0]  
  964223344
```
**OBJECT ID**

És un tipus que defineix MongoDB per a poder obtenir valors únics. És el valor
per defecte de l'element **_id** , necessari en tot document (atenció: en un
document, no en un element de tipus document que hem dit equivalent a
l'objecte de JSON). És un número long, és a dir que utilitza 24 bytes.

Farem proves de la seua utilització en la seüent pregunta, en el moment
d'inserir diferents documents.

### 3.2.2 - Operacions bàsiques

En aquest punt anem a veure les operacions més bàsiques, per a poder treballar
sobre exemples pràctics, i així disposar ja d'unes dades inicials per a
practicar.

#### Inserció elemental: insert {.azul}

La funció **insert** afegirà documents a una col·lecció. En el paràmetre posem
el document directament, o una variable que continga el document. Si la
col·lecció no existia, la crearà i després afegirà el document. En la següent
sentència estem treballant sobre la col·lecció **exemple** , que segurament ja
existirà de quan vam fer la pregunta 3.1 d'instal·lació de MongoDB, que per a
provar vam inserir un document. Però si no existia, la crearà sense problemes.
```
> db.exemple.insert({ msg2 : "Com va la cosa?"})  
WriteResult({ "nInserted" : 1 })
```
Acabem d'inserir un nou document, i així ens ho avisa ( **{ "nInserted" : 1
}** , s'ha inserit un document). Automàticament haurà creat un element **_id**
de tipus **ObjectId** , ja que li fa falta per a identificar el document entre
tots els altres de la col·lecció.

I en aquest exemple ens guardem el document en la variable **doc** , i després
l'inserim
```
> doc = { msg3 : "Per ací no ens podem queixar ..."}  
{ "msg3" : "Per ací no ens podem queixar ..." }  
> db.exemple.insert(doc)  
WriteResult({ "nInserted" : 1 })
```
També ens indica que ha inserit un document. I haurà creat també el camp
**_id** com veurem en el següent punt.

#### Lectura: find {.azul}

Tenim dues funcions per a recuperar informació: _**find**_ i _**findOne**_.

  * **find()** : recuperarà tots els documents de la col·lecció, encara que podrem posar criteris per a que ens torne tots els documents que acomplesquen aquestos criteris (ho veurem més avant).
  * **findOne()** : només tornarà un document, en principi el primer. Pot ser sobre tots els documents (i per tant seria el primer document), o posar una condició, i tornaria el primer que acomplirà la condició.

Exemple de **find()** :
```
> db.exemple.find()  
{ "_id" : ObjectId("56ce310bc61e04ba81def50b"), "msg" : "Hola, què tal?" }  
{ "_id" : ObjectId("56ce31f6c61e04ba81def50c"), "msg2" : "Com va la cosa?" }  
{ "_id" : ObjectId("56ce3237c61e04ba81def50d"), "msg3" : "Per ací no ens podem queixar ..." }  
>
```
Exemple de **findOne()** :
```
> db.exemple.findOne()  
{ "_id" : ObjectId("56ce310bc61e04ba81def50b"), "msg" : "Hola, què tal?" }  
>
```
En tots els casos podem comprovar que és cert el que veníem afirmant, que ha
creat automàticament l'element **_id** per a cada document guardat.
Evidentment, cadascú de nosaltres tindrà una valors diferents.

Inserció especificant el id

Ara que ja sabem consultar els document de la col·lecció amb **find()** anem a
continuar les insercions de documents, per veure les possibilitats que tenim.

En els document que hem inserit fins el moment, no hem especificat el camp
**_id** , i Mongo l'ha generat automàticament de tipus **ObjectId**.

Però nosaltres podrem posar aquest camp **_id** amb el valor que vulguem. Això
sí, haurem d'estar segurs que aquest valor no l'agafa cap altre document de la
col·lecció, o ens donarà un error.

Així per exemple anem a inserir la informació d'uns alumnes. Els posarem en
una col·lecció nova anomenada **alumnes** , i els intentarem posar un **_id**
personal. Per exemple posarem els números 51, 52, 53, ...
```
> db.alumnes.insert ({_id: 51 , nom: "Rebeca" , cognoms: "Martí Peral"})  
WriteResult({ "nInserted" : 1 })
```
Ha anat bé, i si mirem els documents que tenim en la col·lecció, comprovarem
que ens ha respectat el **_id** :
```
> db.alumnes.find()  
{ "_id" : 51, "nom" : "Rebeca", "cognoms" : "Martí Peral" }  
>
```
Però si intentem inserir un altre document amb el mateix **_id** (51), ens
donarà error:
```
> db.alumnes.insert ({_id: 51 , nom: "Raquel" , cognoms: "Gomis Arnau"})  
WriteResult({  
"nInserted" : 0,  
"writeError" : {  
    "code" : 11000,  
    "errmsg" : "E11000 duplicate key error collection: test.alumnes index: _id_
    dup key: { : 51.0 }"  
  }  
})  
>
```
Ens avisa que estem duplicant la _clau_ _principal_ , és a dir
l'identificador.

#### Inserció múltiple {.azul}

Quan els documents que volem inserir són senzills, podem inserir més d'un a la
vegada, posant dis del **insert()** un **array** amb tots els elements. En el
següent exemple creem uns quants nombres primers en la col·lecció del mateix
nom:
```
> db.nombresprimers.insert( [ {_id:2} , {_id:3} , {_id:5} , {_id:7} , {_id:11}
> , {_id:13} , {_id:17} , {_id:19} ] )  
BulkWriteResult({  
    "writeErrors" : [ ],  
    "writeConcernErrors" : [ ],  
    "nInserted" : 8,  
    "nUpserted" : 0,  
    "nMatched" : 0,  
    "nModified" : 0,  
    "nRemoved" : 0,  
    "upserted" : [ ]  
})  
>
```
Ens avisa que ha fet 8 insercions, i ací els tenim:
```
> db.nombresprimers.find()  
{ "_id" : 2 }  
{ "_id" : 3 }  
{ "_id" : 5 }  
{ "_id" : 7 }  
{ "_id" : 11 }  
{ "_id" : 13 }  
{ "_id" : 17 }  
{ "_id" : 19 }  
>
```
#### Esborrat: remove {.azul}

Per a esborrar un document d 'una col·lecció utilitzarem la funció **remove**
, passant-li com a paràmetre la condició del document o documents a esborrar.
```
> db.nombresprimers.remove( {"_id" : 19} )  
WriteResult({ "nRemoved" : 1 })  
>
```

Ens avisa que ha esborrat un document.

La condició no cal que siga sobre el camp **_id**. Pot ser sobre qualsevol
camp, i esborrarà tots els que coincideixen.
```
> db.exemple.remove( {"msg3" : "Per ací no ens podem queixar ..."} )  
WriteResult({ "nRemoved" : 1 })  
>
```
També tenim la possibilitat d'esborrar tota una col·lecció amb la funció
**drop()**. Pareu atenció perquè és molt senzilla d'eliminar, i per tant,
potencialment molt perillosa.
```
> db.nombresprimers.drop()  
true  
>
```
#### Actualització - update {.azul}

La funció _**update**_ servirà per a actualitzar un document ja guardat.
Tindrà dos paràmetres:

  * El primer paràmetre serà la condició per a trobar el document que s'ha d'actualitzar.
  * El segon paràmetre serà el nou document que substituirà l'anterior

Per exemple, si mirem les dades actuals:
```
> db.exemple.find()  
{ "_id" : ObjectId("56ce310bc61e04ba81def50b"), "msg" : "Hola, què tal?" }  
{ "_id" : ObjectId("56ce31f6c61e04ba81def50c"), "msg2" : "Com va la cosa?" }
```
Podem comprovar el contingut del segon document, el que te **msg2**. Anem a
modificar-lo: en el primer paràmetre posem condició de recerca (només hi haurà
un) i en el segon posem el nou document que substituirà l'anterior
```
> db.exemple.update( {msg2:"Com va la cosa?"} , {msg2:"Què? Com va la cosa?"})  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
Observeu que la contestació del **update****()** és que ha fet **match** (hi
ha hagut coincidència) amb un document, i que ha modificat un. Si no en troba
cap, no donarà error, senzillament dirà que ha fet match amb 0 documents, i
que ha modificat 0 documents. Mirem com efectivament ha canviat el segon
document
```
> db.exemple.find()  
{ "_id" : ObjectId("56ce310bc61e04ba81def50b"), "msg" : "Hola, què tal?" }  
{ "_id" : ObjectId("56ce31f6c61e04ba81def50c"), "msg2" : "Què? Com va la cosa?" }
```
Ens vindran molt bé les variables per a les actualitzacions, ja que en moltes
ocasions serà modificar lleugerament el document, canviant o afegint algun
element. Ho podrem fer còmodament amb la variable: primer guardem el document
a modificar en una variable; després modifiquem la variable; i per últim fem
l'operació d'actualització. Evidentment si tenim alguna variable amb el
contingut del document ens podríem estalviar el primer pas.
```
> doc1 = db.exemple.findOne()  
{ "_id" : ObjectId("56ce310bc61e04ba81def50b"), "msg" : "Hola, què tal?" }

> doc1.titol = "Missatge 1"  
Missatge 1

> db.exemple.update( {msg:"Hola, què tal?"} , doc1)  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.exemple.findOne()  
{  
  "_id" : ObjectId("56ce310bc61e04ba81def50b"),  
  "msg" : "Hola, què tal?",  
  "titol" : "Missatge 1"  
}  
>
```


### 3.2.3 - Operacions d'actualització avançada

Al final de la pregunta anterior hem vist l'actualització de documents ja
existents a la Base de Dades. Aquesta actualització la féiem modificant tot el
document, encara que tenim la variant de guardar el document en una variable,
modificar aquesta variable i després fer l'actualització amb aquesta variable.
Però observeu que continua sent una modificació de tot el document, una
substitució del document antic per un document nou.

En aquesta pregunta veurem la utilització d'uns modificadors (_modifiers_) de
l'operació **update()** , que ens permetran modificar documents de forma
potent: creant i eliminant claus (elements) d'un document, o canviant-los, i
fins i tot afegir o eliminar elements d'un array.


#### $set {.azul}

El modificador **$set** assigna un valor a un camp del document seleccionat de
la Base de Dades. Si el camp ja existia, modificarà el valor, i si no existia
el crearà.

La sintaxi del modificador **$set** és la següent:
```
{ $set : { clau : valor} }
```
Però recordeu que és un modificador, i l'hem d'utilitzar dins d'una operació
d'actualització. Anirà en el segon paràmetre del **update()** , i per tant amb
aquestos modificadors ja no posem tot el document en el segon paràmetre, sinó
únicament l'operador de modificació.

Mirem-ho millor en un exemple:
```
> db.alumnes.insert( {nom:"Abel", cognoms:"Bernat Carrera"} )  
WriteResult({ "nInserted" : 1 })  
>  
> db.alumnes.findOne()  
{  
  "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
  "nom" : "Abel",  
  "cognoms" : "Bernat Carrera"  
}  
>
```
Suposem ara que li volem afegir l'edat. Abans ho faríem guardant el document
en una variable, i afegint el camp, per a guardar després. Ara ho tenim més
fàcil:
```
> db.alumnes.update( {nom:"Abel"} , { $set: {edat:21} } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
Ha trobat un, i l'ha modificat. Evidentment, si hi haguera més d'un alumne a
mb el nom Abel, els modificaria tots.
```
> db.alumnes.findOne()  
{  
  "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
  "nom" : "Abel",  
  "cognoms" : "Bernat Carrera",  
  "edat" : 21  
}
```
Es pot especificar més d'un camp amb els valor corresponents. Si no existien
es crearan, i si ja existien es modificaran:
```
> db.alumnes.update( {nom:"Abel"} , { $set: {nota: 8.5 , edat:22} } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.alumnes.findOne()  
{  
  "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
  "nom" : "Abel",  
  "cognoms" : "Bernat Carrera",  
  "edat" : 22,  
  "nota" : 8.5  
}
```
I fins i tot es pot canviar el tipus d'un camp determinat, i utilitzar arrays,
i objectes, ...
```
> db.alumnes.update( {nom:"Abel"} , { $set: {nota: [8.5,7.5,9] ,
> adreça:{carrer:"Major",numero:7,cp:"12001"} } } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  

> db.alumnes.findOne()  
{  
  "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
  "nom" : "Abel",  
  "cognoms" : "Bernat Carrera",  
  "edat" : 22,  
  "nota" : [  
      8.5,  
      7.5,  
      9  
  ],  
  "adreça" : {  
      "carrer" : "Major",  
      "numero" : 7,  
      "cp" : "12001"  
  }  
}  
```  

Podem fins i tot modificar ara només el valor d'un camp d'un objecte del
document. Per exemple, anem a modificar el codi postal de l'anterior alumne.
La manera d'arribar al codi postal serà **adreça.cp** , però haurem d'anar amb
compte que vaja entre cometes per a que el trobe:
```
> db.alumnes.update( {nom:"Abel"} , { $set: {adreça.cp:"12502"} } )  
uncaught exception: SyntaxError: missing : after property id :  
@(shell):1:49  
  
> db.alumnes.update( {nom:"Abel"} , { $set: {"adreça.cp":"12502"} } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.alumnes.findOne()  
{  
  "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
  "nom" : "Abel",  
  "cognoms" : "Bernat Carrera",  
  "edat" : 22,  
  "nota" : [  
        8.5,  
        7.5,  
        9  
  ],  
  "adreça" : {  
        "carrer" : "Major",  
        "numero" : 7,  
        "cp" : "12502"  
  }  
}
```
#### $unset {.azul}

El modificador **$unset** servirà per a **eliminar** elements (camps) d'un o
uns documents. Si el camp existia, l'eliminarà, i si no existia, no donarà
error (avisarà que s'han modificat 0 documents).

La sintaxi és:
```
{ $unset : {camp : 1 } }
```
Haurem de posar un valor al camp que anem a esborrar per a mantenir la sintaxi
correcta, i posem 1 que equival a true. També podríem posar -1, que equival a
false, però aleshores no l'esborraria, i per tant no faríem res. Sempre
posarem 1.

Mirem el següent exemple. Afegim un camp, que serà el número d'ordre, i
després el llevarem.
```
> db.alumnes.update( {nom:"Abel"} , { $set: {num_ordre:10} } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.alumnes.findOne()  
{  
  "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
  "nom" : "Abel",  
  "cognoms" : "Bernat Carrera",  
  "edat" : 22,  
  "nota" : [  
        8.5,  
        7.5,  
        9  
  ],  
  "adreça" : {  
        "carrer" : "Major",  
        "numero" : 7,  
        "cp" : "12502"  
  },  
  "num_ordre" : 10  
}  
  
> db.alumnes.update( {nom:"Abel"} , { $unset: {num_ordre:1} } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.alumnes.update( {nom:"Abel"} , { $unset: {puntuacio:1} } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 0 })
```
Hem afegit primer el camp **num_ordre** , i hem mostrat el document per
comprovar que existeix. Després esborrem el camp **num_ordre** (i ens confirma
que ha modificat un document). Després intentem esborrar un camp que no
existeix, **puntuacio**. No dóna error, però ens avisa que ha modificat 0
documents. Podem comprovar al final com el document ha quedat com esperàvem.
```
> db.alumnes.findOne()  
{  
    "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
    "nom" : "Abel",  
    "cognoms" : "Bernat Carrera",  
    "edat" : 22,  
    "nota" : [  
        8.5,  
        7.5,  
        9  
    ],  
    "adreça" : {  
        "carrer" : "Major",  
        "numero" : 7,  
        "cp" : "12502"  
    }  
}
```
#### $rename {.azul}

El modificador **$rename** canviarà el nom d'un camp. Si no existia, no donarà
error i senzillament no el modificarà. Hem de cuidar de posar el nou nom del
camp entre cometes, per a que no done error.

La sintaxi és:
```
{ $rename : { camp1 : "nou_nom1" , camp2 : "nou_nom2" , ... } }
```
Per exemple, canviem el nom del camp **nota** a **notes** :
```
> db.alumnes.update( {nom:"Abel"} , { $rename: {nota:"notes"} } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.alumnes.findOne()  
{  
    "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
    "nom" : "Abel",  
    "cognoms" : "Bernat Carrera",  
    "edat" : 22,  
    "adreça" : {  
          "carrer" : "Major",  
          "numero" : 7,  
          "cp" : "12502"  
    },  
    "notes" : [  
          8.5,  
          7.5,  
          9  
    ]  
}
```
Observeu que l'ha canviat de lloc, cosa que ens fa pensar que en canviar de
nom un camp, el que fa és tornar a crear-lo amb el nou nom, i esborrar el camp
antic.

En aquest exemple tornem a canviar el nom a **nota** , i intentem canviar el
nom a un camp inexistent, **camp1**. No donarà error.
```
> db.alumnes.update( {nom:"Abel"} , { $rename: {camp1: "camp2" , notes:"nota"} } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.alumnes.findOne()  
{  
  "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
  "nom" : "Abel",  
  "cognoms" : "Bernat Carrera",  
  "edat" : 22,  
  "adreça" : {  
      "carrer" : "Major",  
      "numero" : 7,  
      "cp" : "12502"  
  },  
  "nota" : [  
      8.5,  
      7.5,  
      9  
  ]  
}
```
#### $inc {.azul}

Com cabria esperar, el modificador **$inc** servirà per a incrementar un camp
numèric. Si el camp existia, l'incrementarà en la quantitat indicada. Si no
existia, crearà el camp amb un valor inicial de 0, i incrementarà el valor amb
la quantitat indicada. La quantitat pot ser positiva, negativa o fins i tot
amb part fraccionària. Sempre funcionarà bé, excepte quan el camp a
incrementar no siga numèric, que donarà error.

La sintaxi és aquesta:
```
{ $inc : {camp : quantitat } }
```
En els següents exemples, incrementem un camp nou (per tant el crearà amb el
valor especificat), i després l'incrementem en quantitats positives, negatives
i fraccionàries, concretament l'inicialitzem amb un **2** , i despés
l'incrementem en **5** , en **-4** i en **2.25** , per tant el resultat final
serà **5.25** :
```
> db.alumnes.update( {nom:"Abel"} , { $inc: {puntuacio:2} } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.alumnes.findOne()  
{  
    "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
    "nom" : "Abel",  
    "cognoms" : "Bernat Carrera",  
    "edat" : 22,  
    "nota" : [  
        8.5,  
        7.5,  
        9  
    ],  
    "adreça" : {  
        "carrer" : "Major",  
        "numero" : 7,  
        "cp" : "12502"  
    },  
    "puntuacio" : 2  
}  
> db.alumnes.update( {nom:"Abel"} , { $inc: {puntuacio:5} } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.alumnes.update( {nom:"Abel"} , { $inc: {puntuacio:-4} } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.alumnes.update( {nom:"Abel"} , { $inc: {puntuacio:2.25} } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.alumnes.findOne()  
{  
    "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
    "nom" : "Abel",  
    "cognoms" : "Bernat Carrera",  
    "edat" : 22,  
    "nota" : [  
        8.5,  
        7.5,  
        9  
    ],  
    "adreça" : {  
        "carrer" : "Major",  
        "numero" : 7,  
        "cp" : "12502"  
    },  
    "puntuacio" : 5.25  
}
```

#### Elements d'un array {.azul}

Per a accedir directament a un element d'un array d'un determinat document es
pot utilitzar la següent sintaxi:
```
"array.index"
```
Hem de tenir present que el primer element de l'array és el de subíndex 0. I
no us oblideu de tancar-ho tot entre comentes per a que ho puga trobar.

Si no existeix l'element amb el subíndex indicat, donarà error.

Per exemple, anem a pujar un punt la primera nota de l'alumne que estem
utilitzant en tots els exemples :
```
>db.alumnes.update( {nom:"Abel"} , { $inc : { "nota.0" : 1 } } )
```

```
> db.alumnes.findOne()  
{  
    "_id" : ObjectId("56df11d778549bdfbf2125e3"),  
    "nom" : "Abel",  
    "cognoms" : "Bernat Carrera",  
    "edat" : 22,  
    "adreça" : {  
        "carrer" : "Major",  
        "numero" : 7,  
        "cp" : "12502"  
    },  
    "nota" : [  
        9.5,  
        7.5,  
        9  
    ]  
}
```
#### Inserció en Arrays: $push {.azul}

La manera més senzilla d'introduir un element en un array és utilitzar
**$push** sense més. Si existia l'array, introduirà el o els nous elements al
final. Si no existia l'array, el crearà amb aquest o aquestos elements.

La sintaxi és:
```
{ $push : { clau : element } }
```
Per exemple anem a afegir una nota a l'alumne de sempre, i posem-la diferent
per veure que s'introdueix al final:
```
> db.alumnes.update( {nom:"Abel"} , { $push : { nota : 7 } } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
```
> db.alumnes.findOne()  
{  
    "_id" : ObjectId("56df11d778549bdfbf2125e3"),  
    "nom" : "Abel",  
    "cognoms" : "Bernat Carrera",  
    "edat" : 22,  
    "adreça" : {  
        "carrer" : "Major",  
        "numero" : 7,  
        "cp" : "12502"  
    },  
    "nota" : [  
        9.5,  
        7.5,  
        9,  
        7  
    ]  
}
```
També hi ha manera d'introduir un element en una determinada posició que no
siga al final, però es complica prou la cosa, ja que hem d'utilitzar per una
banda el modificador **$position** per a dir on s'ha d'inserir, i per una
altra banda el modificador **$each** per a poder especificar el o els valors
que es volen inserir. Es posa a continuació únicament de forma il·lustrativa.

Per a inserir en una determinada posició hem d'utilitzar obligatòriament 2
modificadors més:

  * **$position** indicarà a partir de quina posició es farà l'acció (normalment d'inserir en l'array, és a dir, **$push**)
  * **$each** ens permet especificar una sèrie de valors com un array, i vol dir que es farà l'operació per a cada valor de l'array

Els dos modificadors seguiran la sintaxi de sempre, de clau valor, per tant el
conjunt de la sintaxi és:
```
{ $ push :  
  { clau_del_array :  
    { $position : _posició_ ,  
    $each : [ _valors_ ]  
    }  
  }  
}
```
Ací tenim un exemple on introduïm una nota en la primera posició:
```
> db.alumnes.update( {nom:"Abel"} , { $push : { nota : { $position : 0 , $each
> : [5] } } } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

> db.alumnes.findOne()  
{  
    "_id" : ObjectId("56df11d778549bdfbf2125e3"),  
    "nom" : "Abel",  
    "cognoms" : "Bernat Carrera",  
    "edat" : 22,  
    "adreça" : {  
          "carrer" : "Major",  
          "numero" : 7,  
          "cp" : "12502"  
    },  
    "nota" : [  
        5,  
        9.5,  
        7.5,  
        9,  
        7  
    ]     
}
```
#### Eliminació en arrays: $pop i $pull {.azul}

Hi ha més d'una manera d'eliminar elements d'un array.

 **$pop**{.azul}

Si volem eliminar el primer element o l'últim, el modificador adequat és
**$pop**. La sintaxi és
```
{ $pop : { clau : posicio } }
```
On en posició podrem posar:

  * -1 , i esborrarà el primer element
  * 1 , i esborrarà l'últim

En els següents exemples s'esborren primer l'últim element i després el
primer.
```
> db.alumnes.findOne()  
{  
    "_id" : ObjectId("56df11d778549bdfbf2125e3"),  
    "nom" : "Abel",  
    "cognoms" : "Bernat Carrera",  
    "edat" : 22,  
    "adreça" : {  
          "carrer" : "Major",  
          "numero" : 7,  
          "cp" : "12502"  
    },  
    "nota" : [  
          5,  
          9.5,  
          7.5,  
          9,  
          7  
    ]  
}
```

```
> db.alumnes.update( {nom:"Abel"} , { $pop : { nota : 1 } } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.alumnes.findOne()  
{  
      "_id" : ObjectId("56df11d778549bdfbf2125e3"),  
      "nom" : "Abel",  
      "cognoms" : "Bernat Carrera",  
      "edat" : 22,  
      "adreça" : {  
            "carrer" : "Major",  
            "numero" : 7,  
            "cp" : "12502"  
      },  
      "nota" : [  
              5,  
              9.5,  
              7.5,  
              9  
      ]  
}
```
```
> db.alumnes.update( {nom:"Abel"} , { $pop : { nota : -1 } } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })  
> db.alumnes.findOne()  
{  
      "_id" : ObjectId("56df11d778549bdfbf2125e3"),  
      "nom" : "Abel",  
      "cognoms" : "Bernat Carrera",  
      "edat" : 22,  
      "adreça" : {  
            "carrer" : "Major",  
            "numero" : 7,  
            "cp" : "12502"  
      },  
      "nota" : [  
            9.5,  
            7.5,  
            9  
      ]  
}
```
 ### $pull {.azul} 

Amb aquest modificador esborrarem els elements de l'array que coincidesquen
amb una condició, estiguen en la posició que estiguem. Observeu com es pot
eliminar més d'un element.

Per a poder comprovar-lo bé, primer inserim un altre element al final de
l'array, amb el valor **7.5** (si heu seguit els mateixos exemples que en
aquestos apunts, aquest valor ja es troba en la segona posició).
```
> db.alumnes.update( {nom:"Abel"} , { $push : { nota : 7.5 } } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
```  
> db.alumnes.findOne()  
{  
    "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
    "nom" : "Abel",  
    "cognoms" : "Bernat Carrera",  
    "edat" : 22,  
    "adreça" : {  
        "carrer" : "Major",  
        "numero" : 7,  
        "cp" : "12502"  
    },  
    "nota" : [  
        9.5,  
        7.5,  
        9,  
        7.5  
    ]  
}
```
Ara anem a esborrar amb **$pull** l'element de valor **7.5**
```
> db.alumnes.update( {nom:"Abel"} , { $pull : { nota : 7.5 } } )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
```
> db.alumnes.findOne()  
{  
      "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
      "nom" : "Abel",  
      "cognoms" : "Bernat Carrera",  
      "edat" : 22,  
      "adreça" : {  
        "carrer" : "Major",  
        "numero" : 7,  
        "cp" : "12502"  
      },  
      "nota" : [  
          9.5,  
          9  
      ]  
}
```
#### Upsert {.azul}

Aquesta paraula ja l'havíem comentada en un punt anterior.

En el **update()** normal, si la condició de búsqueda no donava cap resultat
(parlant ràpid, si no feis _matching_ amb cap document), doncs no actualitzava
cap document i punt.

El **Upsert** és una variant de l'update, que quan no coincidesca cap document
amb la condició, crearà un document nou que serà el resultat de combinar el
criteri que s'ha utilitzat en la condició amb les operacions d'actualització
fetes en el segon paràmetre

Per a que un **Update** actue d'aquesta manera, li hem de posar un tercer
paràmetre amb el valor **true** :
```
update ( {...} , {...} , true )
```
Recordeu que el primer paràmetre era la condició, i el segon l'actualització.

Mirem-ho en l'exemple dels alumnes. Si anem a actualitzar els cognoms, i es
troba el document, s'actualitzarà:
```
> db.alumnes.update( { nom:"Abel" } , { $set : { cognoms : "Bernat Cantera" }} , true )  
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
Efectivament, ens diu que ha modificat un document.

Però si no es troba el document (per exemple perquè li hem posat el nom
**Berta**):
```
> db.alumnes.update( { nom:"Berta" } , { $set : { cognoms : "Bernat Cantero" }} , true )  
WriteResult({  
"nMatched" : 0,  
"nUpserted" : 1,  
"nModified" : 0,  
"_id" : ObjectId("56dfdbd136d8b095cb6bd57a")  
})
```
Ja ens avisa que no ha fet cap _matching_ , i ha fet un **Upsert**. Ho podem
comprovar mirant tots els document de la col·lecció:
```
> db.alumnes.find()  
{ "_id" : ObjectId("56debe3017bf4ed437dc77c8"), "nom" : "Abel", "cognoms" : "Bernat Cantera", 
"edat" : 22, "adreça" : { "carrer" : "Major", "numero" : 7, "cp" : "12502" }, "nota" : [ 9.5, 9 ] }  
{ "_id" : ObjectId("56dfdbd136d8b095cb6bd57a"), "nom" : "Berta", "cognoms" : "Bernat Cantero" }  
>
```
El nou document tindrà els camps:

  * **_id** , amb el que ens havia avisat que generaria
  * Els camps de la condició, que en el nostre exemple és **{ nom:"Berta" }**
  * Els camps de l'actualització, que en el nostre exemple eren els cognoms

## 3.3 - Consulta de documents

En la pregunta anterior hem vist com introduir, eliminar i modificar
documents. Les consultes de documents han segut molt senzilles, per a
comprovar únicament els resultats.

En aquesta pregunta veurem en profunditat la consulta de documents.

  * Funcions **find()** i **findOne()** , que són les que hem utilitzat fina ara. Veurem en profunditat la seua sintaxi i potència.
  * Limitarem i ordenarem també els resultats
  * Fins i tot podrem elaborar més els resultats, agrupant els resultats, utilitzant funcions d'agregació (o millor dir operadors d'agregació) i donant-los un aspecte diferent

### 3.3.1 - Paràmetres de les funcions find() i findOne()

Les funcions **find()** i **findOne()** són absolutament equivalents, amb
l'única diferència que la primera torna tots els documents trobats, mentre que
la segona només torna el primer document trobat.

Per una millor comprensió, utilitzarem únicament **find()** , per veure tots
els resultats obtinguts.

La funció **find()** s'ha comparat tradicionalment amb la sentència SELECT de
SQL. Sempre tornarà un conjunt de documents, que poden variar des de no tornar
cap document, a tornar-los tots els de la col·lecció.

La funció **find()** pot tenir uns quants paràmetres.

  * El primer indica una condició o criteri, i tornarà aquells documents de la col·lecció que acomplesquen la condició o criteri. Aquesta condició ve donada en forma de document (o objecte) JSON, i és com l'havíem vist en la funció **update()** :
```
db.col_leccio1.find( { clau1 : valor1 } )
```
Tornarà tots els documents de la col·lecció **col_leccio1** que tinguen el
camp **clau1** i que en ell tinguen el valor **valor1**.

Aquest criteri pot ser el complicat que faça falta, formant-lo en JSON. Pot
tenir més d'un membre. En definitiva, tornarà aquells documents que facen
_matching_ amb el document del criteri, és a dir, funcionaria com un **and**
en cas que tinga més d'un membre en la condició
```
db.col_leccio1.find( { clau1 : valor1 , clau2 : valor2 } )
```
que tornaria aquells documents de la **col_lecció1** que tenen el camp
**clau1** amb el valor **valor1** i que tenen el camp **clau2** amb el valor
**valor2**

Si no volem posar cap criteri, per a que els torne tots, no posem res com a
paràmetre, o encara millor, li passem un document (objecte) buit, de manera
que tots els documents de la col·lecció faran _matching_ amb ell.
```
db.col_leccio1.find( { } )
```
Tindrem açò present, sobretot quan ens toque utilitzar el segon paràmetre de
**find**. Si no volem cap criteri, posarem el document buit com l'exemple
anterior.

  * El segon paràmetre ens servirà per a delimitar els camps dels documents que es tornaran. També tindrà el format JSON d'un objecte al qual li posarem com a claus els diferents camps que volem que apareguen o no, i com a valor 1 per a que sí que apareguen i 0 per a que no apareguen.

Si posem algun camp a que sí que aparega (és a dir, amb el valor 1), els únics
que apareixeran seran aquestos, a més del**_id** que per defecte sempre
apareix.
```
> db.alumnes.find({},{nom:1})  
{ "_id" : ObjectId("56debe3017bf4ed437dc77c8"), "nom" : "Abel" }  
{ "_id" : ObjectId("56dfdbd136d8b095cb6bd57a"), "nom" : "Berta" }
```
Per tant si no volem que aparega **_id** posarem:
```
> db.alumnes.find({},{_id:0})  
{ "nom" : "Abel", "cognoms" : "Bernat Cantera", "edat" : 22, "adreça" : {
"carrer" : "Major", "numero" : 7, "cp" : "12502" }, "nota" : [ 9.5, 9 ] }  
{ "nom" : "Berta", "cognoms" : "Bernat Cantero" }
```
I si volem traure únicament el nom:
```
> db.alumnes.find({},{nom:1,_id:0})  
{ "nom" : "Abel" }  
{ "nom" : "Berta" }
```
Per últim, com que a partir d'ara utilitzarem documents més complicats, si
volem que ens apareguen els camps que retornem d'una forma un poc més elegant
o bonica (_pretty_), posarem aquesta funció al final: **find().pretty()**
```
> db.alumnes.find().pretty()  
{  
      "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
      "nom" : "Abel",  
      "cognoms" : "Bernat Cantera",  
      "edat" : 22,  
      "adreça" : {  
          "carrer" : "Major",  
          "numero" : 7,  
          "cp" : "12502"  
      },  
      "nota" : [  
          9.5,  
          9  
      ]  
}  
{  
"_id" : ObjectId("56dfdbd136d8b095cb6bd57a"),  
"nom" : "Berta",  
"cognoms" : "Bernat Cantero"  
}
```
### 3.3.2 - Operadors de les condicions

Abans de començar aquesta pregunta, anem a agafar unes dades de prova, que
estan en el fitxer **libros_ejemplo.json**

Només heu de copiar el contingut del fitxer en la terminal del client de
Mongo.

Posem ací el contingut per a que pugueu pegar-li una miradeta sense necessitat
d'obrir-lo. Anirà bé per als exemples posteriors.

    
    
    db.libro.insert({  
        "_id":"9788408117117",  
          "titulo":"Circo Máximo",  
        "autor":"Santiago Posteguillo",  
        "editorial":"Planeta",  
        "enstock":true,  
        "paginas":1100,  
        "precio":21.75,  
        "fecha":new ISODate("2013-08-29T00:00:00Z"),          
        "resumen":"Circo Máximo, de Santiago Posteguillo, que ha escrito otras obras de narrativa histórica como Las Legiones Malditas o La traición de Roma, es la segunda parte de la trilogía de Trajano, que comenzó con Los asesinos del emperador, un relato impactante, descomunal, descrito con un trepidante pulso narrativo destinado a trasla dar al lector a la Roma imperial de los césares. Santiago posteguillo se ha convertido en el autor español de referencia de la novela histórica sobre Roma y el mundo antiguo. Bienvenidos al mundo de Marco Ulpio Trajano. Circo Máximo es la historia de Trajano y su gobierno, guerras y traiciones, lealtades insobornables e historias de amor imposibles. Hay una vestal, un juicio, inocentes acusados, un abogado especial, mensajes cifrados, códigos secretos, batallas campales, fortalezas inexpugnables, asedios sin fin, dos aurigas rivales, el Anfiteatro, los gladiadores y tres carreras de cuadrigas. Hay también un caballo especial, diferente a todos, leyes antiguas olvidadas, sacrificios humanos, amargura y terror, pero también destellos de nobleza y esperanza, como la llama de Vesta, que mientras arde preserva a Roma. Sólo que hay noches en las que la llama del Templo de Vesta tiembla. La rueda de la Fortuna comienza entonces a girar. En esos momentos, todo puede pasar y hasta la vida del propio Trajano, aunque él no lo sepa, corre peligro. Y, esto es lo mejor de todo, ocurrió: hubo un complot para asesinar a Marco Ulpio Trajano."  
    })  
      
    db.libro.insert({  
         "_id":"9788401342158",  
          "titulo":"El juego de Ripper",  
          "autor":"Isabel Allende",  
          "editorial":"Plaza & Janes",  
          "enstock":true,  
          "paginas":480,  
          "precio":21.75,  
        "fecha":new ISODate("2014-03-01T00:00:00Z"),          
          "resumen":"Tal como predijo la astróloga más reputada de San Francisco, una oleada de crímenes comienza a sacudir la ciudad. En la investigación sobre los asesinatos, el inspector Bob Martín recibirá la ayuda inesperada de un grupo de internautas especializados en juegos de rol, Ripper. 'Mi madre todavía está viva, pero la matará el Viernes Santo a medianoche', le advirtió Amanda Martín al inspector jefe y éste no lo puso en duda, porque la chica había dado pruebas de saber más que él y todos sus colegas del Departamento de Homicidios. La mujer estaba cautiva en algún punto de los dieciocho mil kilómetros cuadrados de la bahía de San Francisco, tenían pocas horas para encontrarla con vida y él no sabía por dónde empezar a buscarla",  
     })  
       
    db.libro.insert({  
        "_id":"9788496208919",  
       "titulo":"Juego de tronos: Canción de hielo y fuego 1",  
       "autor":"George R.R. Martin",  
       "editorial":"Gigamesh",  
       "enstock":true,  
       "paginas":793,  
       "precio":9.5,  
       "fecha":new ISODate("2011-11-24T00:00:00Z"),       
       "resumen":"Tras el largo verano, el invierno se acerca a los Siete Reinos. Lord Eddars Stark, señor de Invernalia, deja sus dominios para unirse a la corte del rey Robert Baratheon el Usurpador, hombre díscolo y otrora guerrero audaz cuyas mayores aficiones son comer, beber y engendrar bastardos. Eddard Stark desempeñará el cargo de M ano del Rey e intentará desentrañar una maraña de intrigas que pondrá en peligro su vida... y la de los suyos. En un mundo cuyas estaciones duran décadas y en el que retazos de una magia inmemorial y olvidada surgen en los rincones más sombrios y maravillosos, la traición y la lealtad, la compasión y la sed de venganza, el amor y el poder hacen del juego de tronos una poderosa trampa que atrapa en sus fauces a los personajes... y al lector. 'El regreso triunfal de Martin a la fantasía de más alta calidad... con personajes desarrollados con maestría, prosa hábil y pura obstinación.'"  
    })  
      
    db.libro.insert({  
      "_id":"9788499088075",  
      "titulo":"La ladrona de libros",  
      "autor":"Markus Zusak",  
      "editorial":"Debolsillo",  
      "enstock":false,  
      "paginas":544,  
      "precio":9.45,  
      "fecha":new ISODate("2009-01-09T00:00:00Z"),        
      "resumen":"En plena II Guerra Mundial, la pequeña Liesel hallará su salvación en la lectura. Una novela preciosa, tremendamente humana y emocionante, que describe las peripecias de una niña alemana de nueve años desde que es dada en adopción por su madre hasta el final de la guerra. Su nueva familia, gente sencilla y nada afecta al na zismo, le enseña a leer y a través de los libros Rudy logra distraerse durante los bombardeos y combatir la tristeza. Pero es el libro que ella misma está escribiendo el que finalmente le salvará la vida.",  
    })  
      
    db.libro.insert({  
      "_id":"9788415140054",  
      "titulo":"La princesa de hielo",  
      "autor":"Camilla Lackberg",  
      "editorial":"Embolsillo",  
      "enstock":true,  
      "precio":11,  
      "fecha":new ISODate("2012-10-30T00:00:00Z"),      
      "resumen":"Misterio y secretos familiares en una emocionante novela de suspense Erica vuelve a su pueblo natal tras el fallecimiento de sus padres, pero se va a encontrar con un nuevo drama. Aparentemente su amiga de la infancia, Alex, se ha suicidado. Pronto se descubre que no solamente fue asesinada sino que estaba embarazada. El primer sospechoso es Anders, un artista fracasado con quien Alex mantenía una relación especial. Pero poco después de ser liberado por falta de pruebas, Anders aparece muerto en su domicilio. Con la ayuda del comisario Patrik, Erica investigará el pasado de su amiga Alex."  
    })  
      
    db.libro.insert({  
      "_id":"9788408113331",  
      "titulo":"Las carreras de Escorpio",  
      "autor":"Maggie Stiefvater",  
      "editorial":"Planeta",  
      "enstock":false,  
      "paginas":290,  
      "precio":17.23,  
      "fecha":new ISODate("2013-06-04T00:00:00Z"),    
      "resumen":"En la pequeña isla de Thisby, cada noviembre los caballos de agua de la mitología celta emergen del mar. Y cada noviembre, los hombres los capturan para participar en una emocionante carrera mortal. En las carreras de Escorpio, algunos compiten para ganar. Otros para sobrevivir. Los jinetes intentan dominar a sus caballos de agua el tiempo suficiente para acabar la carrera. Algunos lo consiguen. El resto, muere en el intento. Sean Kendrick es el favorito, y necesita ganar la carrera para ganar, también, su libertad. Pero Puck Connolly está dispuesta a ser su más dura adversaria. Ella nunca quiso participar en las carreras. Pero no tiene elección: o compite y gana o… lo pierde todo.",  
    })  
      
    db.libro.insert({  
      "_id":"9788468738895",  
      "titulo":"Las reglas del juego",  
      "autor":"Anna Casanovas",  
      "enstock":true,  
      "paginas":null,  
      "precio":15.90,  
      "fecha":new ISODate("2014-02-06T00:00:00Z"),  
      "resumen":"Susana Lobato tiene la vida perfectamente planeada y está a punto de conseguir todo lo que quiere: va a tener su propio programa de noticias económicas y en dos meses va a casarse con un hombre maravilloso. Pero una noche Tim anula la boda y la abandona para perseguir un sueño que no la incluye a ella.Kev MacMurray acaba de cumplir treinta y cinco años y siente que ha llegado el momento de dar un cambio a su vida. No sabe por qué, pero últimamente se está asfixiando y está convencido de que no puede seguir donde está. Lo único que lo retiene es la boda de Tim, su mejor amigo.Pero Tim anula la boda y una noche Kev coincide con Susana y respira por primera vez en mucho tiempo.¿Por qué no le había sucedido antes? Se suponía que él y Susana no se soportaban ¿Desde cuándo siente que si no besa a la prometida de su mejor amigo no podrá seguir respirando?Susana nunca había reaccionado así con nadie. ¿Puede correr el riesgo de averiguar qué pasará si se entrega a Kev?Y qué pasará si vuelve Tim, ¿podrán dar un paso atrás?.",  
    })
    


Podeu comprovar que hi ha 7 documents en la nova col·lecció **libro** :
```
> db.libro.count()  
7
```
I també podem consultar els títols de forma còmoda:
```
> db.libro.find( {} , {titulo:1} )  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo" }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper" }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1" }  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros" }  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo" }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio" }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego" }
```
I un altre exemple, on consultem els llibres que estan en stock (hi ha un camp
booleà que ho diu: **enstock**), mostrant títol, editorial i preu
```
> db.libro.find( {enstock: true} , {titulo:1 , editorial:1 , precio:1} )  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "editorial" : "Planeta", "precio" : 21.75 }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "editorial" : "Plaza & Janes", "precio" : 21.75 }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1", "editorial" : "Gigamesh", "precio" : 9.5 }  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo", "editorial" : "Embolsillo", "precio" : 11 }
```
I un últim exemple, on consultem els llibres que estan en stock i tenen un
preu de 21.75 €, mostrant tot excepte el _id i el resum
```
> db.libro.find( {enstock: true , precio: 21.75} , {titulo:1 , editorial:1 , > precio:1} )  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "editorial" : "Planeta", "precio" : 21.75 }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "editorial" : "Plaza & Janes", "precio" : 21.75 }
```
Anem a mirar ara operadors que ens serviran per fer millor les consultes.

#### Operadors de comparació

Fins ara en totes les condicions hem utilitzat la igualtat, si un determinat
camp era igual a un determinat valor. Però hi ha infinitat de consultes en les
quals voldrem altres operacions de comparació: major, major o igual, menor,
...

Aquestos són els operadors de comparació:

  * **$lt** (_less than_) **menor**
  * **$lte**(_less than or equal_)**menor o igual**
  * **$gt**(_gretaer than_) major
  * **$gte** (_gretaer than or equal_) **major o igual**
  * **$ne** (_not equal_) **distint**
  * **$eq** (_equal_) **igual** (però aquest quasi que no caldria, perquè en no posar res es refereix a la igualtat com fins ara)

La sintaxi per a la seua utilització és, com sempre, acoplar-se a la sintaxi
JSON:
```
clau : { $operador : valor [, ... ] }
```
Així per exemple, per a buscar els llibres de més de 10 €:
```
> db.libro.find( { precio : { $gt : 10 } } , { titulo:1 , precio:1 } )  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "precio" : 21.75 }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "precio" : 21.75 }  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo", "precio" : 11 }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "precio" : 17.23 }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego", "precio" : 15.9
}
```
I per a buscar els llibres entre 10 i 20 €:
```
> db.libro.find( { precio : { $gt : 10 , $lt:20 } } , { titulo:1 , precio:1 }
> )  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo", "precio" : 11 }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "precio" : 17.23 }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego", "precio" : 15.9
}
```
És especialment útil per a les dates, ja que difícilment trobarem una data (i
hora) exacta, i voldrem quasi sempre els documents anteriors a una data, o
posteriors, o entre dues dates. Haurem d'anar amb compte pel tractament
especial de les dates: hem de comparar coses del mateix tipus, i per tant la
data amb la qual volem comparar l'haurem de tenir en forma de data:
```
> var d = new ISODate("2013-01-01T00:00:00Z")  
> db.libro.find( {fecha:{$gte:d} } , {fecha:1} )  
{ "_id" : "9788408117117", "fecha" : ISODate("2013-08-29T00:00:00Z") }  
{ "_id" : "9788401342158", "fecha" : ISODate("2014-03-01T00:00:00Z") }  
{ "_id" : "9788408113331", "fecha" : ISODate("2013-06-04T00:00:00Z") }  
{ "_id" : "9788468738895", "fecha" : ISODate("2014-02-06T00:00:00Z") }
```
 **$in**{.azul}

Servirà per a comprovar si el valor d'un camp està entre els d'una llista,
proporcionada com un array. La sitaxi és:
```
clau : { $in : [valor1 , valor2 , ... , valorN] }
```
I ací tenim un exemple, els llibre de les editorials Planeta i Debolsillo:
```
> db.libro.find( { editorial: {$in : ["Planeta" , "Debolsillo"]} } , {titulo:1
> , editorial:1} )  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "editorial" : "Planeta"}  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros", "editorial" : "Debolsillo" }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "editorial" : "Planeta" }  
  
```
 **$nin**{.azul}

És el contrari, traura els que no estan en la llista.
```
> db.libro.find( { editorial: {$nin : ["Planeta" , "Debolsillo"]} } , > {titulo:1 , editorial:1} )  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "editorial" : "Plaza & Janes" }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1", "editorial" : "Gigamesh" }  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo", "editorial" : "Embolsillo" }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego" }
```
Observeu com també trau els llibres que no tenen editorial, com és el cas de
l'últim llibre, Las reglas del juego

 **$or**{.azul}

L'operador anterior, **$in** , ja feia una espècie de OR, però sempre sobre el
mateix camp. Si l'operació OR la volem fer sobre camps distints, haurem
d'utilitzar l'operador **$or**. La seua sintaxi ha de jugar amb la
possibilitat de posar molts elements, i per tant convé l'array:
```
$or : [ {clau1:valor1} , {clau2:valor2} , ... , {clauN:valorN} ]
```
Serà cert si s'acompleix alguna de les condicions. Per exemple, traure els
llibres que no estan en stock o que no tenen editorial:
```
> db.libro.find( { $or : [ {enstock:false} , {editorial:null} ] } , {titulo:1 > , enstock:1 , editorial:1} )  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros", "editorial" : "Debolsillo", "enstock" : false }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "editorial" : "Planeta", "enstock" : false }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego", "enstock" : true}
```
 **$not**{.azul}

Serveix per a negar una altra condició.
```
$not : { condició }
```
Per exemple els llibres que no són de l'editorial Planeta (observeu que seria
més senzill utilitzar l'operador **$ne** , però és per a mostrar el seu
funcionament:
```
> db.libro.find( { editorial: {$not : {$eq:"Planeta"} } } , {titulo:1 , editorial:1} )  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "editorial" : "Plaza & Janes" }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1", "editorial" : "Gigamesh" }  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros", "editorial" : "Debolsillo" }  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo", "editorial" : "Embolsillo" }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego" }  
``` 

 **$exists**{.azul}

Servirà per a saber els documents que tenen un determinat camp
```
clau : { $exists : _boolean_ }
```
Depenet del valor _boolean_ , el funcionament serà:

  * **true** : torna els documents en els quals existeix el camp, encara que el seu valor siga nul
  * **false** : torna els documents que no tenen el camp.

Anem a traure els llibres que tenen el camp **paginas** :
```
> db.libro.find( { paginas: {$exists:true} } , {titulo:1 , paginas:1} )  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "paginas" : 1100 }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "paginas" : 480 }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1", "paginas" : 793 }  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros", "paginas" : 544 }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "paginas" : 290 }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego", "paginas" : null }
```
Observeu com ens apareix també l'ultim llibre, que té el camp **paginas** amb
el valor **nul**. En canvi si haguérem fet la consulta preguntant pels que són
diferents de nul, no apareixeria aquest últim llibre:
```
> db.libro.find( { paginas: {$ne:null} } , {titulo:1 , paginas:1} )  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "paginas" : 1100 }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "paginas" : 480 }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1", "paginas" : 793 }  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros", "paginas" : 544 }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "paginas" : 290 }
```
I si posem **false** al valor en el **$exists** , únicament ens apareixerà el
llibre que no té el camp:
```
> db.libro.find( { paginas: {$exists:false} } , {titulo:1 , paginas:1} )  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo" }
```
I per la mateixa raó que abans, si traiem els que tenen **paginas** a null,
ens eixirà tant qui no té el camp, com qui el té però amb valor nul:
```
> db.libro.find( { paginas: null } , {titulo:1 , paginas:1} )  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo" }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego", "paginas" : null}
```
Per tant, per a segons quines coses, ens interessa l'operador **$exists** , en
compte de jugar amb el nul.

#### Expressions regulars

Mongo accepta les expressions regulars de forma nativa, cosa que dóna molta
potència per a poder buscar informació diversa.

Les expressions regulars en Mongo tenen la mateixa sintaxi que en Perl, i que
és molt molt pareguda a la major part de llenguatges de programació.

Mirem alguns exemples. Els llibres dins dels quals està la paraula **juego** :
```
> db.libro.find( { titulo: /juego/ } , {titulo:1} )  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper" }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego" }
```
Ara que tenen la paraula **juego** sense importar majúscules o minúscules:
```
> db.libro.find( { titulo: /juego/i } , {titulo:1} )  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper" }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1" }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego" }
```
I ara que tenen la paraula **juego** només al principi.
```
> db.libro.find( { titulo: /^juego/i } , {titulo:1} )  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1" }
```
I ara els llibres que en el resum (**resumen**) tenen la paraula **amiga** o
**amigo** , és a dir **amig** seguit d'una **a** o una **o** :
```
> db.libro.find( { resumen: /amig[ao]/i } , {titulo:1} )  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo" }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego" }
```
 **Arrays**{.azul}

Les consultes dins d'arrays de Mongo són molt senzilles.

La més senzilla és quan busquem un valor d'un tipus senzill, i en aquest cas
el que farà Mongo és buscar en tot l'array per si està aquest valor. És a dir,
exactament igual que el que hem fet fins ara.
```
db.col_leccio1.find ( { clau_array : valor } )
```
Mirem-ho en un exemple. Anem a crear dos documents que tinguen un array
cadascun, per exemple de colors. El creem en una col·lecció nova, anomenada
**colorins** , en dos documents amb el mateix camp de tipus array, **color** ,
però amb dades diferents********:
```
> db.colorins.insert({color: ["roig","blau","groc"]})  
WriteResult({ "nInserted" : 1 })
```

```
> db.colorins.insert({color: ["negre","blanc","roig"]})  
WriteResult({ "nInserted" : 1 })
```
```
> db.colorins.find();  
{ "_id" : ObjectId("56e1438ff6663c8169030e09"), "color" : [ "roig", "blau", "groc" ] }  
{ "_id" : ObjectId("56e14398f6663c8169030e0a"), "color" : [ "negre", "blanc", "roig" ] }
```
  
Com es veu en la sintaxi, triar els documents que tenen un camp (en aquest cas
d'array) que continga un valor, és igual de senzill que quan es tracta d'un
camp de tipus string, per exemple:

```
> db.colorins.find({color:"roig"})  
{ "_id" : ObjectId("56e1438ff6663c8169030e09"), "color" : [ "roig", "blau", "groc" ] }  
{ "_id" : ObjectId("56e14398f6663c8169030e0a"), "color" : [ "negre", "blanc", "roig" ] }
```

També podem utilitzar qualsevol dels operadors vistos fins el moment, com per
exemple l'operador **$in** , que mirarà els documents que tenen algun dels
colors que s'especifica a continuació:
```
> db.colorins.find({color: {$in : ["groc","lila"]} })  
{ "_id" : ObjectId("56e1438ff6663c8169030e09"), "color" : [ "roig", "blau", "groc" ] }
```
O per exemple també utilitzar **expressions regulars** :
```
> db.colorins.find({color: /bl/ })  
{ "_id" : ObjectId("56e1438ff6663c8169030e09"), "color" : [ "roig", "blau", "groc" ] }  
{ "_id" : ObjectId("56e14398f6663c8169030e0a"), "color" : [ "negre", "blanc", "roig" ] }
```
 **$ all**{.azul}

L'operador **$all** el podem utilitzar quan vulguem seleccionar els documents
que en l'array tiguen **tots** els elements especificats.

Per exemple, anem a buscar els document que tenen el color roig i blau.

```
> db.colorins.find({color : { $all : ["roig","blau"]} })  
{ "_id" : ObjectId("56e1438ff6663c8169030e09"), "color" : [ "roig", "blau", "groc" ] }
```
 **Subíndex**{.azul}

Si volem mirar exactament una determinada posició de l'array, podem
especificar la posició immediatament després de la clau, **separada per un
punt**. Recordeu que la primera posició és la **0**. Hem de posar entre
cometes la clau i la posició, sinó no sabrà trobar-la.

Per exemple, busquem els documents que tenen el roig en la primera posició.

```
> db.colorins.find({"color.0" : "roig"} )  
{ "_id" : ObjectId("56e1438ff6663c8169030e09"), "color" : [ "roig", "blau", "groc" ] }
```

!!! note "Nota"
    Accedir a una determinada posició és fàcil, però no és tan fàcil accedir a una
    posició calculada, per exemple a l'última posició. Ja fa falta coneixements un
    poc més avançats de JavaScript, per a posar dins del **find()** una funció en
    JavaScript, i actuar dins d'aquesta.
---

Únicament de manera il·lustrativa, posem ací la manera de traure els
documents, l'últim color dels quals és el roig. En ella ens creem una variable
amb l'últim element de l'array (amb **pop()**), i el comparem amb el color
roig, tornant true en cas de que sí que siguen iguals:

```
> db.colorins.find(function() { var a =this.color.pop(); return (a =="roig")})  
{ "_id" : ObjectId("56e14398f6663c8169030e0a"), "color" : [ "negre", "blanc", "roig" ] }
```
També hi ha una forma alternativa de fer-ho, que és utilitzant l'operador
**$where**, que ens permet crear condicions amb sintaxi JavaScript:

```
> db.colorins.find({$where:"this.color[this.color.length - 1]=='roig'"})  
{ "_id" : ObjectId("56e14398f6663c8169030e0a"), "color" : [ "negre", "blanc", "roig" ] }
```
 **$size**{.azul}

L'operador **$size** ens servirà per a fer condicions sobre el número
d'elements d'un array.

Incorporem 2 documents nous, amb 2 i 4 elements respectivament, per a poder
comprovar-lo:

```
> db.colorins.insert({color: ["negre","blanc"]})  
WriteResult({ "nInserted" : 1 })
> db.colorins.insert({color: ["taronja","gris","lila","verd"]})  
WriteResult({ "nInserted" : 1 })
```
```
> db.colorins.find()  
{ "_id" : ObjectId("56e1438ff6663c8169030e09"), "color" : [ "roig", "blau", "groc" ] }  
{ "_id" : ObjectId("56e14398f6663c8169030e0a"), "color" : [ "negre", "blanc", "roig" ] }  
{ "_id" : ObjectId("56e16972aa3c92aaed389da6"), "color" : [ "negre", "blanc" ] }  
{ "_id" : ObjectId("56e16990aa3c92aaed389da7"), "color" : [ "taronja", "gris", "lila", "verd" ] }
```
Ara anem a seleccionar els documents que tenen 4 colors
```
> db.colorins.find({color:{$size:4}})  
{ "_id" : ObjectId("56e16990aa3c92aaed389da7"), "color" : [ "taronja", "gris", "lila", "verd" ] }
```
!!! note "Nota"
    L'operador **$size** només admet un valor numèric, i no es poden concatenar
    expressions amb altres operadors, com per exemple intentar la condició que la
    grandària de l'array siga menor o igual a un determinat valor. Es pot tornar a
    esquivar la qüestió amb l'operador **$where** , i posar la condició en
    JavaScript. Així la consulta dels documents que tenen 3 o menys colors la
    podríem traure d'aquesta manera:

```
> db.colorins.find({$where:"this.color.length<=3"})  
{ "_id" : ObjectId("56e1438ff6663c8169030e09"), "color" : [ "roig", "blau", "groc" ] }  
{ "_id" : ObjectId("56e14398f6663c8169030e0a"), "color" : [ "negre", "blanc", "roig" ] }  
{ "_id" : ObjectId("56e16972aa3c92aaed389da6"), "color" : [ "negre", "blanc" ]}
```

 **$slice**{.azul}

L'operador **$slice** no és un operador que es puga posar en les condicions
(criteris), sinó que servirà per a extraure determinats elements de l'array,
pel número d'ordre d'aquestos elements en l'array. Només el podrem posar, per
tant, en el segon paràmetre del **find()**.

La sintaxi és:
```
clau : {$slice : x }
```
Els valors que pot agafar **x** són:

  * Números positius: serà el número d'elements del principi (per l'esquerra)
  * Números negatius: serà el número d'elements del final (per la dreta)
  * Un array de 2 elements (**[x,y]**): traurà a partir de la posició **x** (0 és el primer), tants elements com indique **y**

Per exemple, anem a traure els dos primers colors de cada document:
```
> db.colorins.find({} , {color:{$slice:2} })  
{ "_id" : ObjectId("56e1438ff6663c8169030e09"), "color" : [ "roig", "blau" ] }  
{ "_id" : ObjectId("56e14398f6663c8169030e0a"), "color" : [ "negre", "blanc" ] }  
{ "_id" : ObjectId("56e16972aa3c92aaed389da6"), "color" : [ "negre", "blanc" ] }  
{ "_id" : ObjectId("56e16990aa3c92aaed389da7"), "color" : [ "taronja", "gris" ] }
```
O traure l'últim color:
```
> db.colorins.find({} , {color:{$slice:-1 }})  
{ "_id" : ObjectId("56e1438ff6663c8169030e09"), "color" : [ "groc" ] }  
{ "_id" : ObjectId("56e14398f6663c8169030e0a"), "color" : [ "roig" ] }  
{ "_id" : ObjectId("56e16972aa3c92aaed389da6"), "color" : [ "blanc" ] }  
{ "_id" : ObjectId("56e16990aa3c92aaed389da7"), "color" : [ "verd" ] }
```
O traure el tercer element, tinguen els que tinguen. Recordeu que el segon
element, és el de la posició 2, i en volem traure 1.
```
> db.colorins.find({} , {color:{$slice:[2,1] }})  
{ "_id" : ObjectId("56e1438ff6663c8169030e09"), "color" : [ "groc" ] }  
{ "_id" : ObjectId("56e14398f6663c8169030e0a"), "color" : [ "roig" ] }  
{ "_id" : ObjectId("56e16972aa3c92aaed389da6"), "color" : [ ] }  
{ "_id" : ObjectId("56e16990aa3c92aaed389da7"), "color" : [ "lila" ] }
```
**Recerques en objectes**

Per a fer recerques en camps que a la seua vegada són objectes (o documents
dins de documents, en la terminologia de Mongo), només hem de posar la ruta de
les claus separant per mig de punts, i cuidar de posar-la entre cometes.

Així, per exemple, anem a fer una consulta sobre la col·lecció d'alumnes, que
eren uns documents en els quals hi havia algun camp de tipus objecte.
```
> db.alumnes.find().pretty()  
{  
    "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
    "nom" : "Abel",  
    "cognoms" : "Bernat Cantera",  
    "edat" : 22,  
    "adreça" : {  
    "carrer" : "Major",  
    "numero" : 7,  
    "cp" : "12502"  
    },  
    "nota" : [  
        9.5,  
        9  
    ]  
}  
{  
"_id" : ObjectId("56dfdbd136d8b095cb6bd57a"),  
"nom" : "Berta",  
"cognoms" : "Bernat Cantero"  
}
```
Es podrien traure els documents (els alumnes) que viuen en el codi postal
12502. Ens ha d'eixir l'únic alumne del qual tenim l'adreça, que justament té
aquest codi postal. Recordeu que en la clau (realment clau.subclau), ha d'anar
entre cometes. Hem posat al final **pretty()** per a una millor lectura, però
evidentment no és necessari.
```
> db.alumnes.find({"adreça.cp": "12502"}).pretty()  
{  
    "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
    "nom" : "Abel",  
    "cognoms" : "Bernat Cantera",  
    "edat" : 22,  
    "adreça" : {  
        "carrer" : "Major",  
        "numero" : 7,  
        "cp" : "12502"  
    },  
    "nota" : [  
        9.5,  
        9  
    ]  
}
```
I funcionaria igual amb qualsevol número de subnivells, és a dir, documents
que tenen objectes, els quals tenen objectes, ... I també amb altres tipus
d'operadors, o expressions regulars, ...

Per exemple, tots els alumnes de Castelló (el codi postal ha de començar per
12 i contenir 3 xifres més, és a dir, caràcter del 0 al 9, i 3 vegades.
```
> db.alumnes.find({"adreça.cp": /^12[0-9]{3}/}).pretty()  
{  
    "_id" : ObjectId("56debe3017bf4ed437dc77c8"),  
    "nom" : "Abel",  
    "cognoms" : "Bernat Cantera",  
    "edat" : 22,  
    "adreça" : {  
        "carrer" : "Major",  
        "numero" : 7,  
        "cp" : "12502"  
    },  
    "nota" : [  
        9.5,  
        9  
    ]  
}
```
 **Limit, Skip i Sort**{.azul}

Una vegada tenim feta una consulta, podem limitar el nombre de documents que
ens ha de tornar, o ordenar-los.

Per a això hi ha uns mètodes que apliqiuem al final del **find()** , és a dir,
a continuació del **find()** , separats per un punt.

Ho aplicarem als llibres, que és on tenim més documents. I no mostrem tots els
camps, per a una millor lectura:
```
> db.libro.find({} , {titulo:1 , precio:1 , editorial:1})  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "editorial" : "Planeta", "precio" : 21.75 }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "editorial" : "Plaza & Janes", "precio" : 21.75 }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1", "editorial" : "Gigamesh", "precio" : 9.5 }  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros", "editorial" : "Debolsillo", "precio" : 9.45 }  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo", "editorial" : "Embolsillo", "precio" : 11 }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "editorial" : "Planeta", "precio" : 17.23 }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego", "precio" : 15.9 }
```
 **limit(_n_)**{.azul}

Limita el número de documents tornats a _**n**_ documents.
```
> db.libro.find({} , {titulo:1 , precio:1 , editorial:1}).limit(3)  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "editorial" : "Planeta", "precio" : 21.75 }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "editorial" : "Plaza & Janes", "precio" : 21.75 }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1", "editorial" : "Gigamesh", "precio" : 9.5 }
```
Si el número de documents que fa la consulta és menor que _**n**_ , doncs se'n
tornaran menys. Així per exemple, de l'editorial Planeta només hi ha dos
llibres. Encara que posem limit(3), se'n tornaran 2.
```
> db.libro.find({editorial:"Planeta"} , {titulo:1 , precio:1 , > editorial:1}).limit(3)  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "editorial" : "Planeta", "precio" : 21.75 }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "editorial": "Planeta", "precio" : 17.23 }
```
 **skip(_n_)**{.azul}

Se saltaran els primers _**n**_ documents. Si hi haguera menys documents dels
que se salten, doncs no se'n mostraria cap.
```
> db.libro.find({} , {titulo:1 , precio:1 , editorial:1}).skip(2)  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1", "editorial" : "Gigamesh", "precio" : 9.5 }  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros", "editorial" : "Debolsillo", "precio" : 9.45 }  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo", "editorial" : "Embolsillo", "precio" : 11 }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "editorial" : "Planeta", "precio" : 17.23 }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego", "precio" : 15.9 }
```
 **sort()**{.azul}

Serveix per a ordenar. Com a paràmetre se li passarà un objecte JSON amb les
claus per a ordenar, i els valors seran:

  * 1: ordre ascendent
  * -1: ordre descendent

Si posem més d'una clau, s'ordenarà pel primer, en cas d'empat pel segon, ...

En aquest exemple ordenem pel preu
```
> db.libro.find({} , {titulo:1 , precio:1 , editorial:1}).sort({precio:1})  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros", "editorial" : "Debolsillo", "precio" : 9.45 }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1", "editorial" : "Gigamesh", "precio" : 9.5 }  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo", "editorial" : "Embolsillo", "precio" : 11 }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego", "precio" : 15.9 }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "editorial" : "Planeta", "precio" : 17.23 }  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "editorial" : "Planeta", "precio" : 21.75 }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "editorial" : "Plaza & Janes", "precio" : 21.75 }
```
I com déiem, es pot posar més d'un camp d'ordenació. Per exemple, per
editorial en ordre ascendent, i per preu en ordre descendent
```
> db.libro.find({} , {titulo:1 , precio:1 , editorial:1}).sort({editorial:1 ,
> precio:-1})  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego", "precio" : 15.9 }  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros", "editorial" : "Debolsillo", "precio" : 9.45 }  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo", "editorial" : "Embolsillo", "precio" : 11 }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1", "editorial" : "Gigamesh", "precio" : 9.5 }  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "editorial" : "Planeta", "precio" : 21.75 }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "editorial" : "Planeta", "precio" : 17.23 }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "editorial" : "Plaza & Janes", "precio" : 21.75 }
```
Observeu com el primer és el que no té editorial (equivalent a null). I com
que hi ha dos de l'editorial Planeta, apareix primer el més car, i després el
més barat.

I evidentment, es poden combinar els mètodes limit, skip i sort.

En aquest exemple traurem el segon i tercer llibre més car. Per a això ordenem
per preu de forma descendent, saltem un i limitem a 2. No importa l'ordre com
col·locar skip, limit i sort.
```
> db.libro.find({} , {titulo:1 , precio:1 , > editorial:1}).sort({precio:-1}).skip(1).limit(2)  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "editorial" : "Plaza & Janes", "precio" : 21.75 }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "editorial" : "Planeta", "precio" : 17.23 }
```

### 3.3.3 - Agregació

L'agregació ens permetrà fer consultes molt avançades. És un procés un poc
complicat però molt potent. Ens donarà una potència quasi com la del SQL quan
comencem a utilitzar el GROUP BY i HAVING.

La tècnica que s'utilitza és la del _**pipeline**_ , és a dir fer una sèrie de
comandos, cadascun agafa les dades que proporciona l'anterior i a la seua
vegada proporciona les dades al següent comando. D'aquesta manera es tractarà
un conjunt de documents i es faran "operacions" sobre ells seqüencialment en
blocs: filtrat, projecció, agrupacions, ordenació, limitació i _skipping_
(saltar alguns).

La sintaxi serà:
```
db.col_leccio1.aggregate ( _operador $matc_ h ,  _operador $projec_ t ,
_operador $group_ , _operador $sort_ , _operador $limit_ , _operador $skip_ )
```
L'ordre dels operadors pot canviar, però hem de tenir en compte que els
comandos s'executen en el ordre en què els posem (d'esquerra a dreta). Així,
per exemple, pot ser molt convenient posar el primer operador el $match, que
és el de seleccionar documents, així les altres operacions es faran sobre
menys documents i aniran més ràpides.

Cada paràmetre del aggregate, és a dir, cada operador tindrà format JSON, i
per tant sempre serà de l'estil:
```
{ $operador : { clau:valor , ... } }
```
#### $match {.azul}

Servirà per a filtrar els documents. Aleshores, l'agregació només afectarà als
documents seleccionats. Es poden utilitzar tots els operadors que hem anat
estudiant.

El següent exemple selecciona els documents de l'editorial Planeta. Ho fa per
mig de **aggregate** , però com no fem res més, senzillament selecciona els
documents.
```
> db.libro.aggregate({$match:{editorial:"Planeta"}})
```
En el següent exemple, a més de seleccionar els de l'editorial Planeta després
apliquem una projecció sobre els camps títol i editorial, per a poder
visualitzar millor el resultat.
```
>
> db.libro.aggregate({$match:{editorial:"Planeta"}},{$project:{titulo:1,editorial:1}})  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "editorial" : "Planeta"}  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "editorial": "Planeta" }
```
#### $project {.azul}

Ens permet projectar sobre determinats camps del document, però és molt més
complet que en la projecció "normal" que havíem fet fins ara, ja que permet
també renomenar camps, fer càlculs, etc.

 **Projeccció**

La manera més senzilla, evidentment és projectar sobre alguns camps dels
existents, i el funcionament és idèntic al de l'altra vegada (valors 1 per a
que apareguen, 0 per a que no apareguen; per defecte**_id** sempre apareix):
```
> db.libro.aggregate({$project:{titulo:1,editorial:1}})  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "editorial" : "Planeta" }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "editorial" : "Plaza & Janes" }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1", "editorial" : "Gigamesh" }  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros", "editorial" : "Debolsillo" }  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo", "editorial" : "Embolsillo" }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "editorial" : "Planeta" }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego" }
```
 **Renomenar**

**$project** també ens permet renomenar camps existents (després veurem que també
càlculs). La manera serà posar d'aquest manera:

```json
{ $project : { "nom_nou" : "$camp_vell" }}
```

El secret està en el dòlar que va davant del camp vell, ja que d'aquesta
manera ens referim al valor d'aquest camp. Així per exemple renomenem el camp
**enstock** a **disponible** , a banda de traure el títol:

```
> db.libro.aggregate({$project:{titulo:1 , disponible:"$enstock"}})  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "disponible" : true }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "disponible" : true }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1" disponible" : true }  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros", "disponible" : false }  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo", "disponible" : true }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "disponible" : false }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego", "disponible" : true }
```

  **Camps calculats**

Amb aquest nom genèric ens referirem a tots els càlculs, expressions i més
coses que podrem posar per a trasnsformar el que ja tenim. Com veiem, açò és
molt més potent que la projecció normal.

  * **Expressions matemàtiques** : Podrem aplicar fórmules per a sumar (**$add**), restar (**$subtract**), multiplicar (**$multiply**), dividir (**$divide**) i més coses (potència, arrel quadrada, valor absolut, mòdul, ...). Cada operació té el seu operador que serà una paraula precedida pel dòlar, i amb la sintaxi de JSON, on posarem els operands en un array. 

Per exemple, traurem títol del llibre, preu i preu en pessetes (multiplicant
per 166.386)
```
> db.libro.aggregate({$project:{titulo:1 , precio:1 ,
> preu_pessetes:{$multiply:["$precio" , 166.386]}}})  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "precio" : 21.75, "preu_pessetes" : 3618.8955 }  
{ "_id" : "9788401342158", "titulo" : "El juego de Ripper", "precio" : 21.75, "preu_pessetes" : 3618.8955 }  
{ "_id" : "9788496208919", "titulo" : "Juego de tronos: Canción de hielo y fuego 1", "precio" : 9.5, "preu_pessetes" : 1580.667 }  
{ "_id" : "9788499088075", "titulo" : "La ladrona de libros", "precio" : 9.45, "preu_pessetes" : 1572.3476999999998 }  
{ "_id" : "9788415140054", "titulo" : "La princesa de hielo", "precio" : 11, "preu_pessetes" : 1830.2459999999999 }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "precio" : 17.23, "preu_pessetes" : 2866.83078 }  
{ "_id" : "9788468738895", "titulo" : "Las reglas del juego", "precio" : 15.9, "preu_pessetes" : 2645.5374 }
```
  * **Expressions de dates** : Ja veurem i ja podem anar intuint que moltes agregacions estaran basades en el temps, per a poder fer consultes de documents de la setmana passada, o el mes passat, ... Per a poder fer aquestes agregacions, hi ha un conjunt d'expressions que permeten extraure fàcilment d'una data el seu dia, mes, any, ... en forma de número

Són les expressions: **$year, $month, $week, $dayOfMonth, $DayOfWeek,
$dayOfYear, $hour, $minute** i **$second**.

En el següent exemple traurem tots els documents, projectant per la data, any
i mes:
```
> db.libro.aggregate({$project : {fecha:1 , año:{$year:"$fecha"} , > mes:{$month:"$fecha"}}})  
{ "_id" : "9788408117117", "fecha" : ISODate("2013-08-29T00:00:00Z"), "año" : 2013, "mes" : 8 }  
{ "_id" : "9788401342158", "fecha" : ISODate("2014-03-01T00:00:00Z"), "año" : 2014, "mes" : 3 }  
{ "_id" : "9788496208919", "fecha" : ISODate("2011-11-24T00:00:00Z"), "año" : 2011, "mes" : 11 }  
{ "_id" : "9788499088075", "fecha" : ISODate("2009-01-09T00:00:00Z"), "año" : 2009, "mes" : 1 }  
{ "_id" : "9788415140054", "fecha" : ISODate("2012-10-30T00:00:00Z"), "año" : 2012, "mes" : 10 }  
{ "_id" : "9788408113331", "fecha" : ISODate("2013-06-04T00:00:00Z"), "año" : 2013, "mes" : 6 }  
{ "_id" : "9788468738895", "fecha" : ISODate("2014-02-06T00:00:00Z"), "año" : 2014, "mes" : 2 }
```
  * **Expressions de strings** : Ens permeten manipular els strings per a extraure subcadenes, concatenar, passar a majúscules o minúscules. Aquestes són algunes de les funcions:

    * **$substr : [exp , inici , llargària]** : extrau una subcadena del string del primer paràmetre, des de la posició que indica el segon paràmetre (o primer caràcter) i tants caràcters com el tercer paràmetre
    * **$concat : [ exp1 , exp2 , ...]** : concatena les expressions que hi ha en l'array
    * **$toLower : exp** i **$toUpper : exp** : converteixen l'expressió a majúscules i minúscules respectivament

Per exemple, anem a traure el títol dels llibres amb l'autor entre parèntesis:
```
> db.libro.aggregate({$project: { "Llibre:" : {$concat : ["$titulo" , " (" , > "$autor" , ")"]}}})  
{ "_id" : "9788408117117", "Llibre:" : "Circo Máximo (Santiago Posteguillo)" }  
{ "_id" : "9788401342158", "Llibre:" : "El juego de Ripper (Isabel Allende)" }  
{ "_id" : "9788496208919", "Llibre:" : "Juego de tronos: Canción de hielo y fuego 1 (George R.R. Martin)" }  
{ "_id" : "9788499088075", "Llibre:" : "La ladrona de libros (Markus Zusak)" }  
{ "_id" : "9788415140054", "Llibre:" : "La princesa de hielo (Camilla Lackberg)" }  
{ "_id" : "9788408113331", "Llibre:" : "Las carreras de Escorpio (Maggie Stiefvater)" }  
{ "_id" : "9788468738895", "Llibre:" : "Las reglas del juego (Anna Casanovas)"}
```
I ara el mateix, però amb el títol en majúscules:
```
> db.libro.aggregate({$project: { "Llibre:" : {$concat :
> [{$toUpper:"$titulo"}, " (" , "$autor" , ")"]}}})  
{ "_id" : "9788408117117", "Llibre:" : "CIRCO MáXIMO (Santiago Posteguillo)" }  
{ "_id" : "9788401342158", "Llibre:" : "EL JUEGO DE RIPPER (Isabel Allende)" }  
{ "_id" : "9788496208919", "Llibre:" : "JUEGO DE TRONOS: CANCIóN DE HIELO Y FUEGO 1 (George R.R. Martin)" }  
{ "_id" : "9788499088075", "Llibre:" : "LA LADRONA DE LIBROS (Markus Zusak)" }  
{ "_id" : "9788415140054", "Llibre:" : "LA PRINCESA DE HIELO (Camilla Lackberg)" }  
{ "_id" : "9788408113331", "Llibre:" : "LAS CARRERAS DE ESCORPIO (Maggie Stiefvater)" }  
{ "_id" : "9788468738895", "Llibre:" : "LAS REGLAS DEL JUEGO (Anna Casanovas)" }
```
#### $group {.azul}

Realitza grups sobre els documents seleccionats prèviament, per a valors
iguals del camp o expressions que determinem. Posteriorment, amb els grups,
podrem realitzar operacions, com sumar o traure la mitjana d'alguna quantitat
dels documents del grup, o el màxim o mínim, ...

Per a poder agrupar, haurem de definir com a **_id** del grup el camp o camps
pels valors dels quals volem agrupar. Per exemple, si volem agrupar els
llibres per l'editorial, haurem de definir el **_id** del grup el camp
editorial

```
$group : { "_id" : _camp o camps_ }
```
Si agrupem per un únic camp, senzillament el posem amb un dòlar davant i entre
cometes. Si és més d'un camp, els posem com un objecte. Per exemple, agrupem
per editorial:
```
> db.libro.aggregate( { $group : { "_id" : "$editorial" } } )  
{ "_id" : "Debolsillo" }  
{ "_id" : null }  
{ "_id" : "Gigamesh" }  
{ "_id" : "Embolsillo" }  
{ "_id" : "Plaza & Janes" }  
{ "_id" : "Planeta" }
```
Podem observar com hi ha algun llibre que no té editorial.

Ara agrupem per any de publicació (l'extraurem del camp **fecha**):
```
> db.libro.aggregate( { $group : { "_id" : { "any" : { $year : "$fecha" } } }} )  
{ "_id" : { "any" : 2012 } }  
{ "_id" : { "any" : 2009 } }  
{ "_id" : { "any" : 2011 } }  
{ "_id" : { "any" : 2014 } }  
{ "_id" : { "any" : 2013 } }
```
I ara agrupem per editorial i any de publicació (els dos llibres de Planeta
són del 2013)
```
> db.libro.aggregate( { $group : { "_id" : { "Editorial" : "$editorial" ,
> "any" : { $year : "$fecha" } } } } )  
{ "_id" : { "Editorial" : "Embolsillo", "any" : 2012 } }  
{ "_id" : { "any" : 2014 } }  
{ "_id" : { "Editorial" : "Debolsillo", "any" : 2009 } }  
{ "_id" : { "Editorial" : "Gigamesh", "any" : 2011 } }  
{ "_id" : { "Editorial" : "Plaza & Janes", "any" : 2014 } }  
{ "_id" : { "Editorial" : "Planeta", "any" : 2013 } }
```
 **Operadors d'agrupació**

Ens permetran fer alguna operació sobre els documents del grup. Es posen com a
segon paràmetre del grup (després de la definició del **_id**).

  * **$sum : valor** : sumarà el valor de tots els documents del grup. El valor pot ser un camp numèric, o alguna altra cosa més complicada.
  * **$avg : valor** : calcularà la mitjana dels valors per als documents del grup
  * **$max : valor** : màxim
  * **$min : valor** : mínim
  * **$first : exp** : agafarà el primer valor de l'expressió del grup, ignorant les altres del grup
  * **$last : exp** : agafarà l'últim

La documentació diu que també existeix l'operador **$count** , però a partir
d'una determinada versió. Es pot substituir la seua utilització per l'operador
**$sum** , sumant la quantitat 1.

Per exemple, la suma dels preus dels llibres de cada editorial:
```
> db.libro.aggregate( { $group : { "_id" : "$editorial" , "suma_preus" : {
> $sum : "$precio"} } } )  
{ "_id" : "Debolsillo", "suma_preus" : 9.45 }  
{ "_id" : null, "suma_preus" : 15.9 }  
{ "_id" : "Gigamesh", "suma_preus" : 9.5 }  
{ "_id" : "Embolsillo", "suma_preus" : 11 }  
{ "_id" : "Plaza & Janes", "suma_preus" : 21.75 }  
{ "_id" : "Planeta", "suma_preus" : 38.980000000000004 }
```
O la mitjana dels preus de cada any:
```
> db.libro.aggregate( { $group : { "_id" : { "any" : { $year : "$fecha" } } ,
> "mitjana preus" : { $avg : "$precio" } } } )  
{ "_id" : { "any" : 2012 }, "mitjana preus" : 11 }  
{ "_id" : { "any" : 2009 }, "mitjana preus" : 9.45 }  
{ "_id" : { "any" : 2011 }, "mitjana preus" : 9.5 }  
{ "_id" : { "any" : 2014 }, "mitjana preus" : 18.825 }  
{ "_id" : { "any" : 2013 }, "mitjana preus" : 19.490000000000002 }
```
I ara intentem comptar la quantitat de llibres de cada editorial:
```
> db.libro.aggregate( { $group : { "_id" : "$editorial" , "quants" : { $sum :
> 1} } } )  
{ "_id" : "Debolsillo", "quants" : 1 }  
{ "_id" : null, "quants" : 1 }  
{ "_id" : "Gigamesh", "quants" : 1 }  
{ "_id" : "Embolsillo", "quants" : 1 }  
{ "_id" : "Plaza & Janes", "quants" : 1 }  
{ "_id" : "Planeta", "quants" : 2 }
```
#### $sort {.azul}

Serveix per a ordenar i segueix la mateixa sintàxi que en les consultes normal
(1: ordre ascendent; -1: ordre descendent). Podrem ordenar pels camps normals
o per camps clalculats.

Per exemple ordenem per la suma de preus de cada editorial:
```
> db.libro.aggregate( { $group : { "_id" : "$editorial" , "suma_preus" : {
> $sum : "$precio"} } } , { $sort : { suma_preus : 1 } })  
{ "_id" : "Debolsillo", "suma_preus" : 9.45 }  
{ "_id" : "Gigamesh", "suma_preus" : 9.5 }  
{ "_id" : "Embolsillo", "suma_preus" : 11 }  
{ "_id" : null, "suma_preus" : 15.9 }  
{ "_id" : "Plaza & Janes", "suma_preus" : 21.75 }  
{ "_id" : "Planeta", "suma_preus" : 38.980000000000004 }
```
I ara ordenem de forma descendent per la mitjana de preus de cada any:
```
> db.libro.aggregate( { $group : { "_id" : {"any":{$year:"$fecha"}} , "mitjana
> preus":{$avg:"$precio"} } } , {$sort:{"mitjana preus":-1}})  
{ "_id" : { "any" : 2013 }, "mitjana preus" : 19.490000000000002 }  
{ "_id" : { "any" : 2014 }, "mitjana preus" : 18.825 }  
{ "_id" : { "any" : 2012 }, "mitjana preus" : 11 }  
{ "_id" : { "any" : 2011 }, "mitjana preus" : 9.5 }  
{ "_id" : { "any" : 2009 }, "mitjana preus" : 9.45 }
```
#### $limit {.azul}

Limita el resultat del aggregate al número indicat.

Per exemple, els tres anys de mitjana de preus més cara. És com l'últim
exemple, afegint el límit:
```
> db.libro.aggregate({$group:{"_id":{"any":{$year:"$fecha"}},"mitjana
> preus":{$avg:"$precio"}}} , {$sort:{"mitjana preus":-1}} , {$limit:3})  
{ "_id" : { "any" : 2013 }, "mitjana preus" : 19.490000000000002 }  
{ "_id" : { "any" : 2014 }, "mitjana preus" : 18.825 }  
{ "_id" : { "any" : 2012 }, "mitjana preus" : 11 }
```
#### $skip {.azul}

Salta el número indicat

En l'exemple anterior, ara saltem els 3 primers:
```
> db.libro.aggregate({$group:{"_id":{"any":{$year:"$fecha"}},"mitjana
> preus":{$avg:"$precio"}}} , {$sort:{"mitjana preus":-1}} , {$skip:3})  
{ "_id" : { "any" : 2011 }, "mitjana preus" : 9.5 }  
{ "_id" : { "any" : 2009 }, "mitjana preus" : 9.45 }
```

## 3.4 - Connexió des de Kotlin i Exemples

Per a poder connectar des de Java o Kotlin ens sera suficient amb un driver,
que haurem d'incorporar al projecte. En la següent pàgina podem trobar-lo, en
diferents versions:

<https://repo1.maven.org/maven2/org/mongodb/mongo-java-driver/>

En el moment de fer aquestos apunts, l'últim driver disponible és el següent:

<https://repo1.maven.org/maven2/org/mongodb/mongo-java-driver/3.9.1/mongo-
java-driver-3.9.1.jar>

Per a separar les proves i exercicis de la part de **Redis** , creem un nou
projecte anomenat **Tema7_2** , en un paquet anomenat **Exemples**.

 **<u>Connexió**</u>

La connexió és tan senzilla com el següent:

```   
       val con = MongoClient("localhost", 27017)
       val bd = con.getDatabase("test")
```    

És a dir, obtenim un objecte **MongoClient** passant-li al constructor
l'adreça del servidor i el port de connexió (que per defecte és 27017).

Posteriorment hem de connectar amb la Base de Dades. Ja havíem comentat en la
instal·lació de Mongo que nosaltres només utilitzaríem una Base de Dades ja
creada anomenada **test**. Obtenim un objecte **MongoDatabase** que farà
referència a la Base de Dades, i és l'objecte que utilitzarem a partir d'ara.
Evidentment ho podríem haver fet en una única línia.

Si el servidor no el tenim en la mateixa màquina, només haurem de substituir
**localhost** per l'adreça on estiga el servidor.

Per a tancar la connexió:
```   
       con.close()
```
 **<u>Inserció de documents</u>**

Des de Kotlin podrem inserir documents amb la mateixa facilitat que des de la
consola. Només haurem de crear un objecte **Document** de **BSON** (recordeu
que és el format intern de Mongo, absolutament similar a **JSON**). La manera
d'anar posant parelles calu valor en aquest document és per mig del mètode
**put**. Fem un exemple molt senzill on senzillament guardem un document amb
una parella clau-valor d'un missatge. Guardeu-lo amb el nom **Prova1.kt** :

    
    
    import com.mongodb.MongoClient
    import org.bson.Document
    
    fun main(args: Array<String>) {
        val con = MongoClient("localhost", 27017)
        val bd = con.getDatabase("test")
        val doc = Document()
        doc.put("msg4", "Missatge inserit des de Kotlin")
        bd.getCollection("exemple").insertOne(doc)
        con.close()
    }

Segurament traurà avisos en la consola, però només són avisos. Podem comprovar
en la terminal com s'ha inserit el document:

![](T8_3_4_1.png)

 **<u>Consultes</u>**

Tenim el mètode **find()** per a fer consultes, i li podem posar un document
com a paràmetre per a seleccionar determinats documents o traure determinada
informació. Guardeu el següent exemple amb el nom **Prova2.kt** :

    
    
    import com.mongodb.MongoClient
    
    fun main(){
        val con = MongoClient("localhost" , 27017)
        val bd = con.getDatabase("test")
       val llibres = bd.getCollection("libro").find()
    
        for (llibre in llibres)
            println(llibre.get("titulo"))
    
       con.close();
    }

I com comentàvem podem posar com a paràmetres en el find() per a seleccionar
determinats documents, ordenar, etc. Només hem de cuidar que ho hem de posar
en **JSON**(millor dit **BSON**), i per tant haurem de crear un document per a
això. Copieu el següent exemple amb el nom **Prova3.kt** :

    
    
    import com.mongodb.MongoClient
    import org.bson.Document
    
    fun main(){
        val con = MongoClient("localhost", 27017)
        val bd = con.getDatabase("test")
    
        val ordenar = Document()
        ordenar.put("precio", -1)
    
        val llibres = bd.getCollection("libro").find().sort(ordenar)
    
        for (llibre in llibres) System.out.println(
            "Títol: " + llibre.get("titulo").toString() + ". Preu: " + llibre.get("precio")
        )
    
        con.close()
    }

El resultat serà aquest:

![](T8_3_4_2.png)

Hem pogut observar en l'execució dels exemples anteriors que ens trau una gran
quantitat d'avisos. No són errors, sinó senzillament informació de com van les
connexions, que mongo torna, i apareixen en l'eixida estàndar.

Si no volem que apareguen podem posar al principi del programa una sentència
per a dir que únicament apareguen els errors, i no els avisos. Els errors
estarien en la categoria de **SEVERE**

    
```
        LogManager.getLogManager().getLogger("").setLevel(Level.SEVERE)
```  

Hem d'importar les classes de **java.util**. Quedaria el mateix exemple
**Prova3.kt** d'aquesta manera:

    
    
    import com.mongodb.MongoClient
    import org.bson.Document
    import java.util.logging.Level
    import java.util.logging.LogManager
    
    fun main(){
        LogManager.getLogManager().getLogger("").setLevel(Level.SEVERE)
        val con = MongoClient("localhost", 27017)
        val bd = con.getDatabase("test")
    
        val ordenar = Document()
        ordenar.put("precio", -1)
    
        val llibres = bd.getCollection("libro").find().sort(ordenar)
    
        for (llibre in llibres) System.out.println(
            "Títol: " + llibre.get("titulo").toString() + ". Preu: " + llibre.get("precio")
        )
    
        con.close()
    }

I el resultat ara serà:

![](T8_3_4_3.png)

 **<u>Agregació</u>**

Per a poder utilitzar la funció d'agregació, que té tanta potència, ens ho hem
de muntar d'aquesta manera:

  * Sobre la col·lecció utilitzar el mètode **aggregate()**
  * Com a paràmetres, afegirem un **MutableList**. Cada element d'ell serà una opció d'aggregate
  * Per a cada opció ($match, $project...), utilitzarem el mètode d'**Aggregates**(una classe del driver de Mongo) amb el mateix nom: 
    * Per a **$match** : **Aggregates.match()**
    * Per a **$project: Aggregates.project()**
    * ...
  * Dins de cadascun d'aquestos mètodes posarem un document BSon amb les especificacions que necessitem

En aquest exemple farem la selecció (match) dels llibres de l'editorial
Planeta, agafant (project) únicament el títol i l'editorial.

La sentència en la consola la faríem així:
```
> db.libro.aggregate({$match:{editorial:"Planeta"}},{$project:{titulo:1,
> editorial:1}})  
{ "_id" : "9788408117117", "titulo" : "Circo Máximo", "editorial" : "Planeta" }  
{ "_id" : "9788408113331", "titulo" : "Las carreras de Escorpio", "editorial" : "Planeta" }
```
El programa ens quedaria així. Guardeu-lo en en fitxer Kotlin anomenat
**Prova_Aggregate_1.kt** :

    
    
    import com.mongodb.MongoClient
    import com.mongodb.client.model.Aggregates
    import org.bson.Document
    import java.util.logging.Level
    import java.util.logging.LogManager
    
    fun main(){
        LogManager.getLogManager().getLogger("").setLevel(Level.SEVERE)
        val con = MongoClient("localhost", 27017)
        val bd = con.getDatabase("test")
    
        val seleccionar = Document()
        seleccionar.put("editorial", "Planeta")
    
        val projeccio = Document()
        projeccio.put("titulo",1)
        projeccio.put("editorial",1)
    
        val llibres = bd.getCollection("libro").aggregate(mutableListOf(Aggregates.match(seleccionar),Aggregates.project(projeccio)))
    
        for (llibre in llibres)
            println("Títol: " + llibre.get("titulo").toString() + ".   Editorial: " + llibre.get("editorial").toString()
        )
    
        con.close()
    }

Mirem un altre exemple. Traure el llibre més barat de cada any.

La consulta en Mongo que faríem en la consola de Mongo seria:
```
> db.libro.aggregate(  
{$group : {"_id" : { "any" : { $year : "$fecha" } } , "minim":{$min: "$precio"}}}, {$sort:{"_id":1}})  
{ "_id" : { "any" : 2009 }, "minim" : 9.45 }  
{ "_id" : { "any" : 2011 }, "minim" : 9.5 }  
{ "_id" : { "any" : 2012 }, "minim" : 11 }  
{ "_id" : { "any" : 2013 }, "minim" : 17.23 }  
{ "_id" : { "any" : 2014 }, "minim" : 15.9 }
```
Ara col·locarem aquesta sentència en un programa Kotlin. Guardeu el següent
codi com el fitxer Kotlin **Prova_Aggregate_2.kt** :

    
    
    import com.mongodb.MongoClient
    import com.mongodb.client.model.Aggregates
    import org.bson.Document
    import java.util.logging.Level
    import java.util.logging.LogManager
    
    fun main(){
        LogManager.getLogManager().getLogger("").setLevel(Level.SEVERE)
        val con = MongoClient("localhost", 27017)
        val bd = con.getDatabase("test")
    
        val grup = Document()
        grup.put("_id", Document("any",Document("\$year","\$fecha")))
        grup.put("minim",Document("\$min","\$precio"))
    
        val ordenar = Document()
        ordenar.put("_id",1)
    
        val llibres = bd.getCollection("libro").aggregate(mutableListOf(Aggregates.group(grup),Aggregates.sort(ordenar)))
    
        for (llibre in llibres){
            val d = llibre.get("_id") as Document
            println("Any: " + (d.get("_id") as Document).getInteger("any")+ ".   Mínim: " + d.getDouble("minim"))
        }
    
        con.close()
    }

Hem tingut la complicació que el resultat és més complicat, té un document
dins d'una altre document (com es veu en el resultat de la sentència executada
en la consola). Aquest serà el resultat:

![](T8_3_4_1.1.png)

## 3.5 - Connexió des de Kotlin al servidor de l'Institut

La connexió canvia si la fem al servidor de l'Institut, no per ser una
direcció externa, sinó perquè cal la autenticació. Hem habilitat
l'autenticació per a intentar evitar atacs.

Si no tinguèrem autenticació, la connexió seria com abans:

    
```   
        val con = MongoClient("89.36.214.106")
        val bd = con.getDatabase("test")
```    

Però per tenir l'autenticació activada la connexió haurà de ser diferent:

    
```    
        val con = MongoClient(MongoClientURI("mongodb://ad:ieselcaminas@89.36.214.106/?authSource=test"))
        val bd = con.getDatabase("test")
```    

on hem especificat l'adreça donant-li un usuari (**ad**), una contrasenya
(**ieselcaminas**), el servidor (**89.36.214.106**) i fins i tot la Base de
Dades on connectem, que ja va comentar que només teníem la BD **test**

Per a tancar la connexió, com sempre:

    
```    
        con.close()
```
  
  

Mirem un exemple on podem comprovar que l'utilitzem exactament igual que amb
la connexió a localhost, però ara ens està contestant el servidor de
l'Institut. Guardeu-lo amb el nom **ProvaServidor.kt** :

    
    
    import com.mongodb.MongoClient
    import com.mongodb.MongoClientURI
    import org.bson.Document
    import java.util.logging.Level
    import java.util.logging.LogManager
    
    fun main(){
        LogManager.getLogManager().getLogger("").setLevel(Level.SEVERE)
    
        val con = MongoClient(MongoClientURI("mongodb://ad:ieselcaminas@89.36.214.106/?authSource=test"))
        val bd = con.getDatabase("test")
    
        val ordenar = Document()
        ordenar.put("precio", -1)
    
        val llibres = bd.getCollection("libro").find().sort(ordenar)
    
        for (llibre in llibres)
            System.out.println(llibre.get("titulo").toString() + " --> " + llibre.get("precio"))
    
        con.close()
    }

I ací tenim el resultat:

![](T8_3_5_2.png)



Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
SenseObraDerivada 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)

