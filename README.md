# Dojo Modules
**Dit is de basis voor de modules opdracht. De uitgewerkte applicatie is een map gebaseerd op de JavaScript API van ESRI met een spraakgestuurde zoekfunctie. In de eerste stappen wordt beschreven hoe je de basis applicatie aan de praat kunt krijgen. Daarna wordt stap voor stap de applicatie uitgebreid. Op het eind is de opdracht om de applicatie functionaliteiten in modules op te splitsen.**

## Instaleren van de applicatie
1. Installeer NodeJS versie 6 of hoger. ([https://nodejs.org/en/](https://nodejs.org/en/))
2. Zorg dat npm geïnstalleerd is. Wanneer je NodeJS installeerd krijg je deze er automatisch bij.
3. Clone deze repository op je laptop met Git. Via de Command line:
```
git clone https://github.com/jurgendevries/dojo-modules.git 
```
4. Ga via de Command Line naar de map waar de Github repository gecloned is.
5. Voer het volgende commando uit: (Hiermee wordt lite-server geïnstalleerd om de app te kunnen draaien).
```
npm install
```
6. Als het installeren van de dependencies klaar is voer je het volgende commando uit om de app daadwerkelijk te starten:
```
npm start
```
7. Je hebt nu een draaiende applicatie die een map toont van Nederland. Ga naar [http://localhost:3000/](http://localhost:3000/) om de applicatie te tonen, als deze nog niet automatisch geopend is.

## Toevoegen zoekfunctionaliteit
* In de index.html zie je dat https://cdnjs.cloudflare.com/ajax/libs/annyang/2.6.0/annyang.min.js met een script tag wordt ingeladen. Dit zorgt ervoor dat we spraakgestuurd kunnen gaan zoeken.
* Op [https://github.com/TalAter/annyang](https://github.com/TalAter/annyang) en [https://github.com/TalAter/annyang/blob/master/docs/README.md](https://github.com/TalAter/annyang/blob/master/docs/README.md) is te zien hoe je deze library kunt gebruiken.
* Voeg de volgende code onderaan de laatste script tag in **index.html** toe:
```diff
require([
    "esri/map",
    "dojo/domReady!"
], function(Map) {
    var map = new Map("map", {
        center: [6.56, 53.2193836],
        zoom: 8,
        basemap: "streets"
    });

+   if(annyang) {
+       function search(searchTerm) {
+           console.log(searchTerm); 
+       };
+        
+       var commands = {
+           '*searchTerm': function(searchTerm) {
+               search(searchTerm);
+           }
+       };
+
+       annyang.addCommands(commands);
+       annyang.setLanguage('nl-NL');
+
+       annyang.start();
+       console.log('started');
+   }
});
```
* Wanneer de annyang library goed ingeladen is en dus beschikbaar is, wordt de code uitgevoerd.
  * Eerst maken we een functie aan.
  * Vervolgens een commando in de Annyang syntax. Wordt er een spraakbericht opgevangen dat voldoet aan deze syntax dan wordt de hiervoor genoemde functie aangeroepen.
  * Het commando voegen we toe aan Annyang zodat deze zoekt naar de opgegeven syntax.
  * We zetten de taal op Nederlands.
  * En als laatste wordt Annyang gestart.

* De applicatie wordt na elk opgeslagen bestand automatisch opnieuw geladen. Wanneer je nu naar [http://localhost:3000/](http://localhost:3000/) zie je dat de applicatie vraagt om gebruik van je microfoon te maken, accepteer de melding. Als het goed is zie je in de console (F12) nu de tekst 'started' staan en staat er een rood bolletje in je tabblad (Werkt het niet probeer dan de applicatie in Chrome te openen).
* Probeer de applicatie uit en kijk of de tekst in je console tevoorschijn komt.
* We voegen nu de zoek functionaliteit toe. Dit doen we zonder interface aan de voorkant omdat we die in dit geval niet nodig hebben. Voeg de volgende code toe aan je **index.html**:
```diff
require([
    "esri/map",
+    "esri/dijit/Search",
    "dojo/domReady!"
+], function(Map, Search) {
    var map = new Map("map", {
        center: [6.56, 53.2193836],
        zoom: 8,
        basemap: "streets"
    });

+    var searchWidget = new Search({
+        enableLabel: true,
+        enableInfoWindow: false,
+        map: map
+    }, "");
+    searchWidget.startup();

    if(annyang) {
        function search(searchTerm) {
            console.log(searchTerm);
+            searchWidget.search(searchTerm);
        };
        
        var commands = {
            '*searchTerm': function(searchTerm) {
                search(searchTerm);
            }
        };

        annyang.addCommands(commands);
        annyang.setLanguage('nl-NL');

        annyang.start();
        console.log('started');
    }
});
``` 
* Als je de applicatie nu bekijkt en een zoek actie inspreekt zou er bij een resultaat ingezoomed moeten worden op die locatie.
* Je kunt ook testen of het werkt door in de console het commando annyang.trigger('Groningen') te typen.

# Opsplitsen in modules
* De bedoeling is nu dat je deze applicatie gaat opsplitsen in modules. Denk bijvoorbeeld aan een module waar Annyang opgezet wordt, een module waar de zoekwidget op spraak in opgezet wordt en een module die de map opzet en de spraakgestuurde zoekwidget inlaad. Een voordeel hiervan is dat je de spraak module kan hergebruiken voor andere widgets

* Hints...
  * De module die de map inlaad kan een simpele module worden. Je kunt daarbij de bestaande code in een apart bestand zetten.
  * De module voor de zoek widget kan een klas worden waar je het map element aan meegeeft om hier een instantie van te maken.
  * Om de spraak module te kunnen hergebruiken wil je de commando's niet binnen de module zelf bepalen maar deze meegeven vanuit de module die gebruik maakt van de spraak module. In dit geval de spraakgestuurde zoek widget. 
  * Kijk op [https://dojotoolkit.org/documentation/tutorials/1.10/modules/](https://dojotoolkit.org/documentation/tutorials/1.10/modules/) en [https://dojotoolkit.org/documentation/tutorials/1.10/modules_advanced/index.html](https://dojotoolkit.org/documentation/tutorials/1.10/modules_advanced/index.html) om voorbeelden van modules / classes te bekijken.
# Bonus
* Mocht je er aan toe komen, bedenk dan nog een widget die je spraak gestuurd kan maken en maak hier een nieuwe module voor.

# Mocht je er echt niet uit komen...


