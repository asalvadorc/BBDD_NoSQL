# :pencil2: Exercicis

## Exercici 3

Agafa l'estat actual de Bicicas de la següent adreça:

<http://gestiona.bicicas.es/apps/apps.php>

En compte de copiar aquestes dades actuals en un fitxer i després analitzar el
fitxer, analitzarem directament fent la petició a la pàgina del servidor
d'aquesta manera:

    
    
        val url = URL("http://gestiona.bicicas.es/apps/apps.php")
        val rd = url.openConnection().getInputStream().reader()
    

L'objecte **rd** és un Reader que directament el podem passar com a paràmetre
del **JSONTokener** , vist en el Tema 3.

Estudia el format JSON, per a poder agafar bé la informació de cada estació,
tal i com vam veure en el Tema 3.

En un paquet nou anomenat **ExercicisMongo** dins del projecte **Tema7**:

  * Fes un programa anomenat **T7Ex3_IntroduirBicicas** (amb **main**) que introduesca en **MongoDB** cada estació com un document en una col·lecció anomenada **bicicas**.
  * Fes un altre programa anomenat **T7Ex3_****MostrarBicicas** que agafe tots els documents de la col·lecció **bicicas** de la Base de Dades de MongoDb, que són totes les estacions, i traga la seua informació amb aquest aspecte (en la imatge només es mostren les 10 primeres, però han d'eixir totes)
```
01.- UJI - FCHS (5/27)  
02.- ESTACIÃ³N DE FERROCARRIL Y AUTOBUSES (11/28)  
03.- COLÃ³N (9/16)  
04.- PASEO BUENAVISTA-GRAO (4/13)  
05.- HOSPITAL GENERAL (5/18)  
06.- PLAZA DE LA LIBERTAD (0/12)  
07.- PLAZA TEODORO IZQUIERDO (4/13)  
08.- PLAZA PRIMER MOLÃ­ (6/13)  
09.- PATRONAT DESPORTS (7/14)  
10.- PLAZA DOCTOR MARAÃ±Ã³N (0/23)
```
<!--
## Exercici 7.5 (voluntari)

Fes un programa **T7Ex5.kt** que per mig d'una interfície gràfica agafe els
documents de Bicicas de l'exercici anterior:

  * Que acomplisquen la condició configurada amb el l'operador de comparació seleccionat en el JList anomenat **comparacio** i el valor numèric introduït en el JTextField anomenat **quantitat**. Si aquest últim està en blanc, no ha d'haver cap criteri de selecció (s'han d'agafar tots els documents)
  * Que estiguen ordenats pel camp seleccionat en el JList **ordenació** i amb el criteri ascendent o descendent triat en el JList **ascDesc**

Pots intentar-lo amb els mètodes **find()** i **sort()** , o amb el mètode
**aggregate()**.

Aquest seria l'esquelet del programa:

    
    
    import com.mongodb.MongoClient
    import org.bson.Document
    import java.awt.*
    import javax.swing.*
    
    class ConsultaAvançadaBicicas : JFrame() {
    
        val comparacio = JList(arrayOf("<=","=",">="))
        val quantitat = JTextField(10)
        val ordenacio = JList(arrayOf("id","ocupados"))
        val ascDesc = JList(arrayOf("Ascendent","Descentent"))
    
        val llEstacions = JTextArea()
    
        val con = MongoClient("localhost" , 27017)
        val bd = con.getDatabase("test")
    
        init {
            defaultCloseOperation = JFrame.EXIT_ON_CLOSE
            setBounds(100, 100, 350, 350)
            setLayout(GridLayout(1,2))
    
            val panell1 = JPanel(GridLayout(2,1))
            val panell1_1 = JPanel()
            panell1_1.layout=BoxLayout(panell1_1,BoxLayout.Y_AXIS)
            panell1_1.add(JLabel("       Comparació dels llocs ocupats"))
            val panell1_1_1 = JPanel(FlowLayout())
            panell1_1.add(panell1_1_1)
    
            val panell1_2 = JPanel()
            panell1_2.layout=BoxLayout(panell1_2,BoxLayout.Y_AXIS)
            panell1_2.add(JLabel("              Ordenació"))
            val panell1_2_1 = JPanel(FlowLayout())
            panell1_2.add(panell1_2_1)
    
            add(panell1)
            panell1.add(panell1_1)
            panell1.add(panell1_2)
    
            val panell2 = JPanel(BorderLayout())
            add(panell2)
    
            comparacio.fixedCellWidth=100
            val renderer = comparacio.getCellRenderer() as DefaultListCellRenderer
            renderer.horizontalAlignment = JLabel.CENTER
            val scrollComp = JScrollPane(comparacio)
    
            panell1_1_1.add(scrollComp)
            comparacio.selectedIndex=0
            panell1_1_1.add(quantitat)
    
            ordenacio.fixedCellWidth=100
            val renderer1 = ordenacio.getCellRenderer() as DefaultListCellRenderer
            renderer1.horizontalAlignment = JLabel.CENTER
            val scrollOrd = JScrollPane(ordenacio)
            panell1_2_1.add(scrollOrd)
            ordenacio.selectedIndex=0
    
            ascDesc.fixedCellWidth=100
            val renderer2 = ascDesc.getCellRenderer() as DefaultListCellRenderer
            renderer2.horizontalAlignment = JLabel.CENTER
            val scrollAscDesc = JScrollPane(ascDesc)
            panell1_2_1.add(scrollAscDesc)
    
            llEstacions.setForeground(Color.blue)
            val scroll = JScrollPane(llEstacions)
            panell2.add(scroll)
    
            setSize(600, 400)
            setVisible(true)
    
            // Quan canvien comparacio, quantitat, ordenacio i/o ascDesc és quan bisquem
            comparacio.addListSelectionListener{ valorCanviat()}
            quantitat.addActionListener(){ valorCanviat()}
            ordenacio.addListSelectionListener{ valorCanviat()}
            ascDesc.addListSelectionListener{ valorCanviat()}
            ascDesc.selectedIndex=0
    
        }
    
        fun valorCanviat() {
            // Ací és on heu de posar el codi per a seleccionar de MongoDB les estacions que acompleixen la condició i amb l'ordenació indicada
            // Les heu de col·locar en el JTextArea llEstacions
            // Els components que heu de comprovar són:
            //   - comparació (el JList amb l'operacdor de comparació seleccionat)
            //   - quantitat (el JTextField amb la quantitat que s'ha de comparar amb l'operador)
            //   - ordenacio (el JList amb el camp pel qual s'ha d'ordenar)
            //   - ascDesc (El JList amb el criteri d'ordenacoó, ascendent o descendent)
            // Si no s'ha posat res en el JTextField quantitat, no ha d'haver condició de selecció, és a dir, s'han de seleccionar totes les estacions
    
        }
    }
    
    fun main(args: Array<String>) {
        EventQueue.invokeLater {
            ConsultaAvançadaBicicas().isVisible = true
        }
    }

I aquest seria un exemple d'execució, en el qual s'han triat els documents amb
2 o menys llocs ocupats, i ordenats pel número de llocs ocupats de forma
descendent

![](T8_Ex5_1.png)
-->

Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
SenseObraDerivada 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)

