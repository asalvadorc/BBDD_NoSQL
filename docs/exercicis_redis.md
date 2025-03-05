# :pencil2: Exercicis

## Exercici 1

Sobre la Base de Dades **REDIS** del Servidor de l'Institut (adreça
**89.36.214.106**) fer les següents operacions, tant per a guardar una sèrie
de dades, com per a recuperar-les. Sempre posarem a les claus el prefix
**9999x_** , on com sempre heu de substituir 9999 per les 4 últimes xifres del
vostre DNI, i la x per la letra del NIF. Copia-les en un únic fitxer de text,
de forma numerada. És aquest fitxer el que hauràs de pujar.

  1. Crea la clau **9999x_Nom** amb el teu nom
  2. Crea la clau **9999x_Cognoms** amb els teus cognoms. Una de les dues almenys, nom o cognoms, ha de constar de més d'una paraula.
  3. Mostra totes les claus teues, i únicament les teues.
  4. Dóna un temps de vida a la clau **9999x_Nom** de **200 segons**. Comprova el temps de vida que li queda. Posteriorment fes-la **permanent**.
  5. Crea la clau **9999x_Adreca** , de tipus Hash, amb els subcamps **carrer** , **numero** i **cp**. No importa que les dades siguen falses. Pots fer-lo en una sentència o en més d'una.
  6. Afegeix a l'anterior el subcamp **poblacio**
  7. Mostra tota la informació de la teua adreça (només la informació, no les subclaus)****
  8. Crea la clau **9999x_Moduls_ASIX** o **9999x_Moduls_DAM** o **9999x_Moduls_DAW** , depenent del teu cicle. Ha de ser de tipus Set, amb tots els mòduls del teu cicle, que es detallen a continuació. Pots fer-lo en una o en més d'una sentència. 
     * **ASIX** : ISO, PAX, FH, GBD, LM, FOL, ASO, SXI, IAW, ASGBD, SAD, EIE, PROJ i FCT
     * **DAW** : SI, BD, PR, LM, ED, FOL, DWEC, DWES, DAW, DIW, EIE, PROJ i FCT
     * **DAM** : SI, BD, PR, LM, ED, FOL, AD, PMDM, DI, PSP, SGE, EIE, PROJ i FCT.
  9. Crea la clau **9999x_Moduls_meus** , de tipus Set, amb tots els mòduls als quals estàs matriculat. Pots fer-lo en una o en més d'una sentència.
  10. Guarda en la clau **9999x_Moduls_altres** els mòduls en els quals no estàs matriculat actualment. Ha de ser per mig d'operacions de conjunts. Pots comprovar que el resultat és correcte amb **smembers**
  11. Crea una llista amb el nom **9999x_Notes_BD** amb la nota de 4 exercicis de BD. Les notes seran: 7, 9, 6, 10. Han de quedar en aquest ordre (no en ordre invers)
  12. Modifica la tercera nota, que passa de ser 6 a 8.
  13. Crea un **Set Ordenat** (**zset**) anomenat **9999x_Carrera** amb els següents valors. Pots fer-lo en una o en més d'una sentència. I ves amb compte perquè els temps han de ser numèrics  

    
        Sandra	12'52
        Isabel	12'25
        Marta 	12'10
        Maria 	12'07
        Rosa 	11'95
        Bea		11'97
        Balma  	11'90
        Anna  	12'74

  14. Trau les participants de la carrera ordenades pel temps
  15. Penalitza el temps de Bea amb 2 dècimes (0'2), i torna a traure les participants ordenades (Bea ha d'haver perdut 2 posicions, passant de tercera a cinquena posició)

Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
SenseObraDerivada 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)
