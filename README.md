# Lab 01: Introducció a tecnologies Web. Java Servlets i HttpClient

## Introducció

En aquesta primera de sessió de laboratori ens introduirem a la comunicació client/servidor amb HTTP. Més concretament, en el costat del servidor tindrem un Java Servlet executant-se en un servidor Apache Tomcat. A la part client, tindrem un programa Java que utilitzarà la llibreria Apache HttpClient per fer les peticions HTTP al Servlet esmentat.

## Tasques a Realitzar

### Prèvia

Se suposa que ja heu entrat als PCs escollint LINUX com a Sistema Operatiu. Les tasques s'han de dur a terme mantenint l'ordre seqüencial amb què són descrites a continuació.

### Tasca \#1 \(2 punts si es completa abans de la fi de la sessió de laboratori\)

#### 1.1 Instal·lació d'Apache Tomcat

Des del terminal de Linux, executeu la comanda insttomcat7. Comproveu que s'ha creat el directori tomcat dins del vostre directori $HOME.

#### 1.2 Posada en marxa d'Eclipse J2EE i configuració del Servidor Tomcat a Eclipse

Des del terminal de Linux, executeu la comanda eclipse. Tanqueu la pestanya "Welcome" d'eclipse per poder veure l'àrea de treball. A la part inferior hi veureu la pestanya "Servers". Aneu-hi i cliqueu "No servers are available. Click this link ...". A la finestra emergent seleccioneu "Apache&gt;Tomcat v7.0 Server" i cliqueu "Next &gt;". En aquesta nova pantalla, feu "Browse..." fins al vostre directori arrel de tomcat \(aquell que s'ha creat a l'apartat 1.1\) i cliqueu "Finish". Veureu que a la pestanya "Project Explorer" \(al _pane_ de més a l'esquerra\) ara hi teniu una nova carpeta "Servers".

#### 1.3 Fork & Clone del repositori "waslab01"

1. Des del vostre navegador, aneu primer a [https://bitbucket.org/](https://bitbucket.org/) i loguegeu-vos-hi \(com a mínim un dels membres de la parella haurà de tenir les credencials necessàries\).
2. Aneu ara a [https://bitbucket.org/fib\_was/waslab01](https://bitbucket.org/fib_was/waslab01) i feu-ne un "Fork": Cliqueu primer sobre el "+" de l'esquerra i després seleccioneu "Fork this repository". En el formulari que us apareixerà no cal que canvieu el nom del vostre repositori però sí que cal que **marqeu l'opció** "**This is a private repository**" \(Access Level\).
3. Un cop ja teniu creat el vostre repositori, aneu a Settings&gt;User and group access i **afegiu-hi \(Add\) l'usuari fib\_was** \(si no el reconeix, poseu **cfarre@gmail.com**\).
4. Torneu a la pàgina principal del vostre repositori, cliqueu sobre el botó Clone, seleccioneu HTTPS en comptes de SSH i copieu-ne l'URI de la comanda \(p.ex: https://el\_vostre\_username@bitbucket.org/el\_vostre\_username/waslab01.git\).
5. A Eclipse, feu "Window&gt; Show View &gt; Other...".  A la finestra emergent seleccionau "Git &gt; Git repositories" i feu "Open". Veureu que us surt una nova pestanya "Git repositories" al _pane_ inferior. Seleccioneu-hi "Clone a Git repository". A la finestra emergent copieu-hi l'URI del vostre repo, poseu-hi el vostre password a bitbucket.org i feu "Next &gt;". A la següent pantalla no toqueu res i feu "Next &gt;".  I a l'altra pantalla seleccioneu l'opció "Import all existing projects ..." i feu "Finish". Us haureu d'esperar una estoneta i finalment veureu que al pane de l'esquerra \("Project Explorer"\) hi teniu un dos nous projectes: "waslab01\_cs" i "waslab01\_ss".

#### 1.4 Execució de les aplicacions "waslab01\_cs" i "waslab01\_ss".

Comencem primer per "waslab01\_ss". Aquesta és una aplicació web també anomenada "Wal of Tweets". Per executar-la, només cal que cliqueu amb el botó dret del ratolí a sobre del projecte waslab01\_ss i seleccioneu Run As&gt;Run on Server. En principi us hauria de sortir com a preseleccionat el "Tomcat v7.0 Server at localhost". Llavors simplement premeu Finish. Veureu que en el pane central d'Eclipse s'obre una pestanya "Wall of Tweets" mostrant la pàgina web de l'aplicació. També la podeu veure des del vostre navegador si aneu a [http://localhost:8080/waslab01\_ss/](http://localhost:8080/waslab01_ss/). Hauríeu de veure quelcom semblant a [capture01.html](https://atenea.upc.edu/draftfile.php/20081/user/draft/115831728/capture01.html).

El projecte "waslab01\_cs" conté un simple classe Java que fa de client de l'aplicació anterior. Per executar-la,  cliqueu amb el botó dret a sobre del projecte waslab01\_cs i seleccioneu "Run As&gt;Java Application". A la consola d'eclipse hauríeu veure quelcom semblat a això: [capture02.txt](https://atenea.upc.edu/draftfile.php/20081/user/draft/115831728/capture02.txt.html).

#### 1.5 Primer Commit

1. Aneu a la pestanya "Git repositories" i deplegueu el vostre repositori "waslab01 \[master\]". Dins de "Working Tree" hi ha el fitxer README. Feu-hi doble click per editar-lo \(Markdown Source\) i escriure-hi els vostres noms. Llavors salveu-lo.
2. De nou a la pestanya "Git repositories", cliqueu amb el botó dret del ratolí a sobre l'arrel del vostre repositori "waslab01 \[master\]" i seleccioneu "Commit ...".  A la nova pestanya "Git Staging", cliqueu el botó "++" a "Unstaged Changes" perquè el fitxer README.md passi a "Staged Changes". On diu "Commit message" escriviu-hi "**tasca \#1 finalitzada**" i  premeu el botó "**Commit and Push**" \(així també s'actualitzarà el repositori que teniu bitbucktet.org\).

### Tasca \#2 \(1.5 punts si es completa abans de la fi de la sessió de laboratori\)

Tal com haureu pogut veure, el resultat que obté waslab01\_cs, més concretament la seva classe SimpleFluentClient.java, com a client de l'aplicació "Wall of Tweets" \(waslab01\_ss\) és un document en format HTML. L'objectiu d'aquesta tasca serà obtenir un contingut en format "text/plain", amb la mateixa informació però amb un format més adequat per ser mostrat al Console d'Eclipse. Per discriminar el tipus de contingut desitjat utilitzarem un mecanisme propi del protocol HTTP: els HTTP Header fields.

La idea en sí és bastant simple: SimpleFluentClient.java afegirà un Header field a la seva petició HTTP. Més concretament hi afegirà la capçalera "Accept: text/plain", on "Accept" és el nom del camp i "text/plain" n'és el valor. A la banda del servidor  \(waslab01\_ss\), la classe WoTServlet.java serà l'encarregada de comprovar la capçalera que li arriba en la petició GET per tal de generar el contingut en el format desitjat.

Per tant, us tocarà de modificar la implementació de la classe waslab01\_cs&gt;src&gt;asw01cs&gt;SimpleFluentClient.java i la del mètode doGet de la classe waslab01\_ss&gt;Java Resources&gt;src&gt;wallOfTweets&gt;WoTServlet.java. En el primer cas, haureu d'inserir la crida a l'operació [addHeader](http://hc.apache.org/httpcomponents-core-ga/httpcore/apidocs/org/apache/http/HttpMessage.html#addHeader%28java.lang.String,%20java.lang.String%29) tot just abans de la crida execute\(\). En el cas del mètode doGet de WoTServlet.java, haureu de cridar el mètode [getHeader](http://download.oracle.com/javaee/6/api/javax/servlet/http/HttpServletRequest.html#getHeader%28java.lang.String%29) de l'objecte HttpServletRequest req per saber si el client vol o no un "text/plain". En cas afirmatiu, caldrà invocar, i implementar, una operació alternativa, printPLAINresult, que s'encarregui de generar el contingut en el format desitjat.

Un cop fetes aquestes modificacions, el resultat generat per l'execució de SimpleClient.java \(Run As&gt;Java Application\) hauria de ser semblant a [capture03.txt](https://atenea.upc.edu/draftfile.php/20081/user/draft/115831728/capture03.txt). Nota 1: El número que apareix al costat de cada tweet \(p.ex. el '6' de 'tweet \#6'\) correspon al seu identificador; hi ha una operació, getTwid\(\), per obtenir-lo. Nota 2: Les modificacions que feu a la classe WoTServlet.java no haurien de tenir efectes "col·laterals". és a dir, la pàgina web  [http://localhost:8080/waslab01\_ss/](http://localhost:8080/waslab01_ss/) s'hauria de veure al navegador de la mateixa manera que ja es veia a l'apartat 1.5.

Un cop tot funcioni correctament, procediu a fer el **Segon Commit**. Torneu a la pestanya "Git Staging" i cliqueu el botó "++" a "Unstaged Changes" perquè els dos fitxers que heu modificat, waslab01\_cs/src/asw01cs/SimpleFluentClient.java i waslab01\_ss/src/wallOfTweets/WoTServlet.java, passin a "Staged Changes". On diu "Commit message" escriviu-hi "**tasca \#2 finalitzada**" i  premeu el botó "Commit and Push".

### Tasca \#3 \(1.5 punts si es completa abans de la fi de la sessió de laboratori\)

Qualsevol usuari que utilitzi l'aplicació "Wall of Tweets" amb el seu navegador hauria se ser capaç de publicar els seus propis tweets omplint el formulari html que a tal efecte hi ha al principi de la pàgina generada per l'aplicació. Malauradament, aquesta funcionalitat encara no està disponible. L'objectiu d'aquesta tasca serà implementar-la.

Bàsicament, allò que haureu de fer és afegir el codi necessari al mètode doPost de la classe waslab01\_ss&gt;Java Resources&gt;src&gt;wallOfTweets&gt;WoTServlet.java, sense eliminar la linia de codi que ja hi ha \(i que ha de romandre al final\). L'operació per insertar un nou tweet a la base de dades ja està implementada: Database.insertTweet\(String author, String text\). Òbviament per poder-la invocar caldrà que obtingueu abans els paràmetres necessaris \(author i text\). Aquests paràmetres es corresponen amb els dos camps definits al formulari html \("author" i "tweet\_text", respectivament\) que són enviats pel navegador un cop l'usuari prem el botó "Tweet!". Per obternir-los, heu de cridar el mètode [getParameter](http://download.oracle.com/javaee/6/api/javax/servlet/ServletRequest.html#getParameter%28java.lang.String%29) de l'objecte HttpServletRequest req.

Un cop ja pugueu inserir tweets des del navegador, procediu a fer el **Tercer Commit**. Torneu a la pestanya "Git Staging" i cliqueu el botó "++" a "Unstaged Changes" perquè el fitxer que heu modificat, waslab01\_ss/src/wallOfTweets/WoTServlet.java, passi a "Staged Changes". On diu "Commit message" escriviu-hi "**tasca \#3 finalitzada**" i  premeu el botó "Commit and Push".

### Tasca \#4 \(1.5 punts si es completa abans de la fi de la sessió de laboratori\)

En aquesta tasca fareu que el vostre client SimpleFluentClient.java pugui demanar la inserció d'un nou tweet tal com ho fa el navegador quan l'usuari omple el formulari html. Per comprovar que el tweet s'afegeix amb èxit, afegiu el troç de codi necessari abans del codi existent que fa la consulta del contingut del "Wall of Tweets".

Per implementar aquest troç de codi nou, inventeu-vos el valors dels paràmetres "author" i "tweet\_text" i adapteu la comanda Request.Post que trobareu al punt 4 de la pàgina [http://hc.apache.org/httpcomponents-client-ga/quickstart.html](http://hc.apache.org/httpcomponents-client-ga/quickstart.html).

Fetes aquestes modificacions, si executeu ara l'aplicació waslab01\_cs veureu que us surt l'error "Exception in thread "main" org.apache.http.client.HttpResponseException..." a la Console d'Eclipse. Tanmateix, si comproveu la pàgina web de l'aplicació "Wall of Tweets" haurieu de poder veure el tweet que volieu crear s'ha acabat afegint. El motiu de l'excepció que us surt a la Console és que el mètode doPost de la classe waslab01\_ss&gt;Java Resources&gt;src&gt;wallOfTweets&gt;WoTServlet.java fa un "redirect" al final del seu codi \(la linia que hi era des del principi\) que, de retruc, fa que peti el returnContent\(\) final del mètode Post del SimpleFluentClient.java. Per solucionar-ho, tornem a utilitzar l'estratègia d'afegir un Header "Accept" amb valor "text/plain" a la invocació Post de SimpleFluentClient.java. Això implica que caldrà modificar també el mètode doPost de la classe waslab01\_ss&gt;Java Resources&gt;src&gt;wallOfTweets&gt;WoTServlet.java perquè no faci el redirect si el Header "Accept" té el valor "text/plain". Així, en comptes de fer el redirect, el que ha de fer ara és retornar  l'identificador del tweet generat \(l'operació Database.insertTweet\(String author, String text\) que heu usat a la Tasca \#3 ja retorna aquest identificador\). D'aquesta manera, SimpleFluentClient.java podrà genera un output semblant a [capture04.txt](https://atenea.upc.edu/draftfile.php/20081/user/draft/115831728/capture04.txt), on el número del principi correspon a l'identificador del tweet acabat de creat i que coincideix, òbviament, amb el número del primer tweet que apareix a la llista.

Un cop tot funcioni correctament, procediu a fer el **Quart Commit**. Torneu a la pestanya "Git Staging" i cliqueu el botó "++" a "Unstaged Changes" perquè els dos fitxers que heu modificat, waslab01\_cs/src/asw01cs/SimpleFluentClient.java i waslab01\_ss/src/wallOfTweets/WoTServlet.java, passin a "Staged Changes". On diu "Commit message" escriviu-hi "**tasca \#4 finalitzada**" i  premeu el botó "Commit and Push".

### Tasca \#5 \(Fins a 3.5 punts si es completa abans de les 20:00 del dijous 20.02.2020\)

Ara us podeu relaxar una mica perquè per dur a terme aquesta darrera tasca disposareu de més temps. Com a contrapartida, les intruccions que us donaré no seran tan precises. Es tracta, bàsicament, que l'aplicació web "Wall of Tweets" \(waslab01\_ss\) i més concretament, la classe WoTServlet.java implementi la funcionalitat d'esborrar tweets i que tant el client web \(generat pel propi WotSevlet a través de l'operació printHTMLresult\) com el client Java SimpleFluentClient.java la puguin invocar amb èxit. Aquestes són les restriccions que haureu de satisfer:

* El format de la petició HTTP per demanar l'esborrat d'un determinat tweet és el mateix tant  si es fa des del client web \(navegador\) com des del client Java, de tal manera que la implementació des del punt de vista del servidor \(WotServlet.java\) serà la mateixa en ambdós casos.
* En el cas del client web, cada tweet mostrat per pantalla tindrà un enllaç o botó que en clicar-lo permetrà fer la petició per esborrar aquell tweet.
* En el cas del client Java, aquest inclourà al final un troç de codi que sol·licitarà l'esborrament del tweet que es crea al principi \(tasca \#4\). 
* Només es podran esborrar els tweets que prèviament s'han generat des del mateix client. A la pràctica això vol dir que haureu d'utilitzar cookies. Quan WoTServlet.java creï amb èxit un now tweet, retornarà al client una cookie que contindrà l'id del tweet creat. Quan WoTServlet.java rebi la petició per esborrar un determinat tweet, inspeccionarà les cookies que li passa el client per comprovar si alguna d'elles inclou l'id del tweet que aquest vol esborrar.
* No us heu d'amoïnar per la gestió de cookies en els clients: tant el navegador com la llibreria que usa el vostre client Java les gestiona automàticamet, reenviant al servidor totes les cookies que prèviament aquest li ha enviat abans en les seves respostes a peticions anteriors.
* Utilitzeu alguna funció de hash criptogràfica \(MD5, SHA-256, ...\) per codificar l'id del tweet dins de la cookie de tal manera que només el servlet que hagi creat aquella cookie sigui l'únic capaç de determinar la validesa de la cookie en el moment d'esborrar el tweet.

**Commits**. En aquesta darrera tasca no us limiteu a fer un únic "Commit & Push" final, sinó, al contrari, heu de fer Commit & Push" cada cop que modifiqueu un fitxer, tot i que no estigueu segurs de si aquell canvi funciona o no. Això em permetrà a mi de disposar d'una traçabilitat de com heu anat desenvolupant aquesta tasca.

## Lliurament de la Pràctica

Si heu afegit **fib\_was** com a usuari del vostre repositori de bitbucket.org tal com s'indicava a l'apartat 1.3.3, jo podré veure el codi i els commits. Per tant, el repositori en sí és el vostre "lliurament". Això sí, **aneu comprovant de tant en quant que els vostres commits es propaguen correctament al vostre repositori de bitbucket.org**.

## Avaluació

Al principi de cada tasca ja s'especifica quants punts val si es realitza en el termini establert. En el cas de les tasques \#1-4, si no s'acaben dins de la sessió de laboratori, es poden finalitzar amb posterioritat però, en aquest cas, **valdran 0.5 punts** cadascuna.

