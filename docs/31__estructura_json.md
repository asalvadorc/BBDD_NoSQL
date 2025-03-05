Omet navegació

Bases de Dades

  * [Tema 8: Bases de Dades NoSQL](index.html)
  * [1 - Introducció](1__introducci.html)
  * [2 - BD Clau-Valor: Redis](2__bd_clauvalor_redis.html)
    * [2.1 - Instal·lació de Redis](21__installaci_de_redis.html)
    * [2.2 - Entron gràfic: Redis Desktop Manager](22__entron_grfic_redis_desktop_manager.html)
    * [2.3 - Utilització de Redis](23__utilitzaci_de_redis.html)
      * [2.3.1 - Strings](231__strings.html)
      * [2.3.2 - Keys](232__keys.html)
      * [2.3.3 - Hash](233__hash.html)
      * [2.3.4 - List](234__list.html)
      * [2.3.5 - Set](235__set.html)
      * [2.3.6 - Set ordenat](236__set_ordenat.html)
  * [3 - MongoDB](3__mongodb.html)
    * [3.1 - Estructura JSON](31__estructura_json.html)
    * [3.2 - Instal·lació de MongoDB](32__installaci_de_mongodb.html)
      * [3.2.1 - Connexió al servidor de l'Institut](321__connexi_al_servidor_de_linstitut.html)
    * [3.3 - Utilització de MongoDB](33__utilitzaci_de_mongodb.html)
      * [3.3.1 - Tipus de dades](331__tipus_de_dades.html)
      * [3.3.2 - Operacions bàsiques](332__operacions_bsiques.html)
      * [3.2.3 - Operacions d'actualització avançada](323__operacions_dactualitzaci_avanada.html)
        * [3.3.3.1 - $set](3331__set.html)
        * [3.3.3.2 - $unset](3332__unset.html)
        * [3.3.3.3 - $rename](3333__rename.html)
        * [3.3.3.4 - $inc](3334__inc.html)
        * [3.3.3.5 - Elements d'un array](3335__elements_dun_array.html)
        * [3.3.3.6 - Inserció en Arrays: $push](3336__inserci_en_arrays_push.html)
        * [3.3.3.7 - Eliminació en arrays: $pop i $pull](3337__eliminaci_en_arrays_pop_i_pull.html)
        * [3.3.3.8 - Upsert](3338__upsert.html)
    * [3.4 - Consulta de documents](34__consulta_de_documents.html)
      * [3.4.1 - Paràmetres de les funcions find() i findOne()](341__parmetres_de_les_funcions_find_i_findone.html)
      * [3.4.2 - Operadors de les condicions](342__operadors_de_les_condicions.html)
      * [3.4.3 - Agregació](343__agregaci.html)
  * [4 - BD XML: eXist-db](4__bd_xml_existdb.html)
    * [4.1 - Instal·lació d'eXist-db](41__installaci_dexistdb.html)
    * [4.2 - Llenguatges de consultes XPATH i XQUERY](42__llenguatges_de_consultes_xpath_i_xquery.html)
      * [4.2.1 - XPATH](421__xpath.html)
        * [4.2.1.1 - Vista d'arbre](4211__vista_darbre.html)
        * [4.2.1.2 - Navegació](4212__navegaci.html)
        * [4.2.1.3 - Seqüències](4213__seqncies.html)
        * [4.2.1.4 - Funcions](4214__funcions.html)
      * [4.2.2 - XQUERY](422__xquery.html)
        * [4.2.2.1 - Consultes XQuery](4221__consultes_xquery.html)
        * [4.2.2.2 - Variables](4222__variables.html)
        * [4.2.2.3 - Expressions avaluables](4223__expressions_avaluables.html)
        * [4.2.2.4 - Expressions FLWOR](4224__expressions_flwor.html)
        * [4.2.2.5 - Alternatives](4225__alternatives.html)
        * [4.2.2.6 - Exemple de consulta "elaborada"](4226__exemple_de_consulta_elaborada.html)
        * [4.2.2.7 - Funcions de eXist-db de manipulació de BD](4227__funcions_de_existdb_de_manipulaci_de_bd.html)
  * [5 - FIREBASE (voluntari)](5__firebase_voluntari.html)
    * [5.1 - Creació d'una aplicació](51__creaci_duna_aplicaci.html)
    * [5.2 - Realtime Database (RD)](52__realtime_database_rd.html)
    * [5.3 - Cloud Firestore (CF)](53__cloud_firestore_cf.html)
    * [5.4 - Cloud Storage](54__cloud_storage.html)
  * [Exercicis](exercicis.html)

[« Anterior](3__mongodb.html) | [Següent »](32__installaci_de_mongodb.html)

# 3.1 - Estructura JSON

Amb JSON podrem representar:

  * **Valors** , de tipus **caràcter** (entre cometes dobles), **numèric** (sense cometes) , **booleà** (true o false) o **null**.
  * **Parelles clau valor** , és a dir un nom simbòlic acompanyat d'un valor associat.. Es representen així: **"nom" : valor**
  * **Objectes** , que és una col·lecció de membres, cadascú dels quals pot ser una parella clau valor, o altres objectes (fins i tot arrays): es representen entre claus, i amb els elements separats per comes: **{ "nom1" : "valor1" , "nom2": valor2 , valor 3 , ... }**
  * **Arrays** , que són llistes d'elements. Els elements no tenen per què tenir la mateixa estructura, però nosaltres intentarem que sí que la tinguen per coherència. Cada element pot ser un valor , una parella clau valor, un objecte o un array.

Veja'm algun exemples:

{ "p1" : 2 , "p2" : 4 , "p3" : 6 , "p4" : 8 , "p5" : 10 }

en aquest cas tenim un objecte, l'arrel, que té 5 membres, tots ells parelles
clau-valor.

{  
"num": 1 ,  
"nom": "Andreu" ,  
"departament": 10 ,  
"edat": 32 ,  
"sou": 1000.0  
}

ara un objecte, l'arrel, també amb 5 membres que són parelles clau-valor.
Observeu com la clau sempre la posem entre cometes, i el valor quan és un
string també, però quan és numèric, no.

{ "empleat" :  
{ "num": 1 ,  
"nom": "Andreu" ,  
"departament": 10 ,  
"edat": 32 ,  
"sou": 1000.0  
}  
}

en aquest cas tenim un objecte, l'arrel que consta d'un únic objecte,
**empleat** , el qual consta de 5 membres clau-valor.

Mirem ara un exemple amb un array:

{ "notes" :  
[ 5 , 7 , 8 , 7 ]  
}

on tenim l'element arrel que consta d'un únic membre, notes, que és un array.

També seria correcte d'aquesta manera, per veure que l'element arrel no té
perquè ser un objecte, sinó també un array

[ 5 , 7 , 8 , 7 ]

I ara un més complet amb la mateixa estructura que el fitxer XML que havíem
vist en la pregunta 4. Tindrem un objecte arrel, amb només un objecte,
**empresa** , que té un únic element **empleat** que és un array amb 4
elements, cadascun dels empleats:

{ "empresa":  
{ "empleat":  
[ {  
"num": "1",  
"nom": "Andreu",  
"departament": "10",  
"edat": "32",  
"sou": "1000.0"  
},  
{  
"num": "2",  
"nom": "Bernat",  
"departament": "20",  
"edat": "28",  
"sou": "1200.0"  
},  
{  
"num": "3",  
"nom": "Clàudia",  
"departament": "10",  
"edat": "26",  
"sou": "1100.0"  
},  
{  
"num": "4",  
"nom": "Damià",  
"departament": "10",  
"edat": "40",  
"sou": "1500.0"  
}  
]  
}  
}

Anem a veure un parell de casos més reals. Aquesta és la contestació que fa el
WebService de **Bicicas** en sol·licitar l'estat actual de bicicletes en els
diferents punts (en el moment de fer els apunts es consulta en l'adreça
<http://gestiona.bicicas.es/apps/apps.php>):

[  
{"ocupacion":  
[  
{"id":"01","punto":"UJI -
FCHS","puestos":27,"ocupados":12,"latitud":"39.99533","longitud":"-0.06999",
"porcentajeAltaOcupacion":"80","porcentajeBajaOcupacion":"20"},  
{"id":"02","punto":"ESTACIÓN DE FERROCARRIL Y
AUTOBUSES","puestos":24,"ocupados":7,"latitud":"39.98765","longitud":"-0.05281",
"porcentajeAltaOcupacion":"80","porcentajeBajaOcupacion":"20"},  
{"id":"03","punto":"PLAZA DE
PESCADERÍA","puestos":28,"ocupados":4,"latitud":"39.98580","longitud":"-0.03798",
"porcentajeAltaOcupacion":"80","porcentajeBajaOcupacion":"20"},  
...  
]  
}  
]

Com podeu comprovar, l'arrel no és un objecte, sinó un **Array**. En l'array
només ens interessa el primer element que és un objecte amb un únic membre,
**ocupacion**(en l'exemple no hi ha més elements, però en poden haver més en
un moment determinat, quan volen fer avisos). I **ocupacion és un array** ,
amb **un objecte per cada estació de bicicas** , amb les parelles clau valor
**id** , **punto** , **puestos** (les bicicletes que caben), **ocupados**
(quantes bicicletes hi ha col·locades en aquest moment), **latitud** i
**longitud** (les coordenades), ...

**Nota**

En realitat ens apareixerà tota la informació molt més apegada, perquè
realment està en una única línia.

Per a poder observar millor l'estructura podem utilitzar un **visor** de json.
Normalment el navegador Firefox els visualitza bé, encara que també depén de
la versió. Si tenim instal·lada una versió que admet la visualització de JSON,
ho intentarà interpretar, encara que segurament la millor manera de veure el
format JSON és,tiar les opcions **Dades sense processar -- > Format
d'impressió**, que és la que veiem a la dreta:

![](T3_5_1_0_1.png) | ![](T3_5_1_0_2.png)  
---|---  
  
Si la versió nostra de Firefox no visualitza el format JSON, podem buscar un
visor dels molts que hi ha per internet. En la figura n'hem utilitzat un, i es
pot observar com facilita molt la lectura.

![](T3_5_1_1.png)

Un altre exemple. Un WebService de GeoNames (una Base de Dade geogràfica
gratuïta i accessible a través d'Internet) ens proporciona informació dels
llocs que troba dins d'un rectangle delimitat per un latitud al nord i al sud,
i una longitud a l'esti a l'oest (en l'exemple: nord 40.01, sud 39.9, est 0.1
i oest -0.1). Per exemple,
[http://api.geonames.org/citiesJSON?north=40.01&south=39.99&east=0.01&west=-0.01&lang=ES&username=demo](https://maps.googleapis.com/maps/api/geocode/json?latlng=40,0)
torna el següent:

    
    
    {
      "geonames": [
        {
          "lng": -0.04935,
          "geonameId": 2519752,
          "countrycode": "ES",
          "name": "Castelló de la Plana",
          "fclName": "city, village,...",
          "toponymName": "Castelló de la Plana",
          "fcodeName": "seat of a second-order administrative division",
          "wikipedia": "en.wikipedia.org/wiki/Castell%C3%B3n_de_la_Plana",
          "lat": 39.98567,
          "fcl": "P",
          "population": 180005,
          "fcode": "PPLA2"
        },
        {
          "lng": -0.06313,
          "geonameId": 2521909,
          "countrycode": "ES",
          "name": "Almazora",
          "fclName": "city, village,...",
          "toponymName": "Almassora",
          "fcodeName": "populated place",
          "wikipedia": "en.wikipedia.org/wiki/Almassora",
          "lat": 39.94729,
          "fcl": "P",
          "population": 24963,
          "fcode": "PPL"
        },
        ...
        ]
    }

A partir de l'arrel (que ara sí que és un objecte), tenim un membre:
**geonames******, que és un array (un element per cada "lloc" trobat), on cada
element té informació diversa, com el nom del lloc, les coordenades, la
població, ...

**_Nota_**

De fa uns mesos que Google limita el servei anterior, i ha de ser amb un
usuari validat. No valdrà la pena, per al poc profit que li trauríem. Mostrem
en què consisteix el servei únicament a nivell il·lustratiu

[« Anterior](3__mongodb.html) | [Següent »](32__installaci_de_mongodb.html)

Llicenciat sota la  [Llicència Creative Commons Reconeixement
SenseObraDerivada 4.0](http://creativecommons.org/licenses/by-nd/4.0/)

