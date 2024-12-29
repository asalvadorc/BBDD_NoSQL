# 4.1 - Introducció: Firebase

**Firebase** és un servei de backend, és a dir del costat del servidor, que
ofereix la possibilitat de guardar les dades d'aplicacions web i de
dispositius mòbils, amb la particularitat que en temps real es poden
actualitzar les dades modificades des d'un dels clients connectats a tots els
altres que estiguen connectats.

Ofereix una sèrie de serveis, entre els quals podem destacar:

  * **Firebase Auth - Autenticació d'usuaris** : permet gestionar la validació d'usuaris còmodament. Inclou l'autenticació per mig de Facebook, GitHub, Twitter i Google, i també la validació utilitzant compte de correu, guardant-se la contrasenya en Firebase, amb la qual cosa ens allibera d'haver de guardar-les al nostre dispositiu.
  * **Realtime Database - Base de Dades en Temps Real** : permet guardar unes dades en el núvol, que estaran sincronitzades en totes les aplicacions (clients) connectats. No és realment una Base de Dades, tan sols un document JSON.
  * **Cloud Firestore** : és una evolució de l'anterior que ja sí que permet guardar col·leccions de documents JSON, i per tant ja es pot considerar una Base de Dades.
  * **Firebase Storage** : permet guardar arxius per a les nostres aplicacions, com poden ser audios, vídeos, imatges, ...

En aquest tema ens fixarem sobretot en el segon i tercer servei, els de Bases
de Dades. Hem de recordar, que la **Realtime Database** no és una Base de
Dades completa. Tan sols és un document JSON el que podrem guardar, això sí,
tan llarg com vulguem. Es tracta per tant d'una Base de Dades NoSQL, o millor
dit una mini Base de Dades NoSQL. Això sí, que permet sincronitzar les dades
en temps real en tots els dispositius connectats.

En canvi el **Cloud Firestore** sí que és una Base de Dades completa.



Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
SenseObraDerivada 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)

