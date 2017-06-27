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
* Voeg de volgende code onderaan je **index.html** toe:
```
<script>
    require([
        "esri/map",
        "dojo/domReady!"
    ], function(Map) {
        var map = new Map("map", {
            center: [6.56, 53.2193836],
            zoom: 8,
            basemap: "streets"
        });
    });

+    if(annyang) {
+        function search(searchTerm) {
+            console.log(searchTerm); 
+        };
+        
+        var commands = {
+            '*searchTerm': function(searchTerm) {
+                search(searchTerm);
+            }
+        };
+
+        annyang.addCommands(commands);
+        annyang.setLanguage('nl-NL');
+
+        annyang.start();
+        console.log('started');
+    }

</script>
```

```javascript
if(annyang) {
    function search(searchTerm) {
        console.log(searchTerm); 
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
```
* Wanneer de annyang library goed ingeladen is en dus beschikbaar is, wordt de code uitgevoerd.
  * Eerst maken we een functie aan.
  * Vervolgens een commando in de Annyang syntax. Wordt er een spraakbericht opgevangen dat voldoet aan deze syntax dan wordt de hiervoor genoemde functie aangeroepen.
  * Het commando voegen we toe aan Annyang zodat deze zoekt naar de opgegeven syntax.
  * We zetten de taal op Nederlands.
  * En als laatste wordt Annyang gestart.

* De applicatie wordt na elk opgeslagen bestand automatisch opnieuw geladen. Wanneer je nu naar [http://localhost:3000/](http://localhost:3000/) zie je dat de applicatie vraagt om gebruik van je microfoon te maken, accepteer de melding. Als het goed is zie je in de console (F12) nu de tekst 'started' staan en staat er een rood bolletje in je tabblad (Werkt het niet probeer dan de applicatie in Chrome te openen).