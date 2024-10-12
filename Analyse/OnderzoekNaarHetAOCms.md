# Onderzoek naar het AllesOnline CMS
# Inleiding
In dit document wordt de analyse van het door AllesOnline ontwikkelde CMS-systeem gepresenteerd. Deze evaluatie is uitgevoerd als een **Available Product Analysis**, gericht op het identificeren van knelpunten binnen de huidige structuur en functionaliteit van het CMS. Het doel van dit onderzoek is niet alleen om een evaluatie van het bestaande CMS te geven, maar ook om de uitdagingen te identificeren die voortkomen uit de huidige staat van het systeem. Daarnaast dient het als basis voor vergelijking met andere CMS systemen, om te bepalen welke oplossingen het beste aansluiten bij de behoeften van onze developers en klanten.

# Beheren van content
Het CMS biedt de mogelijkheid om de content van de website te beheren. Dit wordt gerealiseerd door verschillende modules en componenten die in het CMS samenwerken. Hieronder een uiteenzetting van de belangrijkste onderdelen binnen dit systeem.

## ContentManagerController
De **ContentManagerController** regisseert de content binnen het CMS en biedt functionaliteiten voor het toevoegen, bewerken, verwijderen en kopiëren van pagina's. De belangrijkste taken zijn: 
1. **Contentbeheer**: 
	* **Toevoegen/Bewerken**: Pagina's worden toegevoegd via `getAdd` en `postAdd`, en bewerkt via `getEdit` en `postEdit`. Na elke actie worden de wijzigingen opgeslagen en kunnen events zoals `PageWasUpdated` worden getriggerd.
	* **Verwijderen**: `postDelete` verwijderd een pagina.
	* **Kopiëren**: `getCopy` maakt een kopie van een pagina en zijn content.
2. **Middleware en permissies:** Zorgt via `lang` en `can` middleware dat acties voorzien zijn van de juiste taal en gebruikersrechten.
3. **Helpers**: 
	* **Templates:** Verkrijgt beschikbare templates voor paginas.
	* **URL-generatie**: Biedt methodes voor het genereren van URL's voor content.
4. **Events en Models:** Werkt samen met modellen zoals `Page` en `ManagedContent` en gebruikt events zoals `PageWasUpdated` voor systeemreacties op wijzigingen.

## ContentManagerModule
De **ContentManagerModule** en de bijbehorende view zijn verantwoordelijk voor het tonen en beheren van hiërarchisch georganiseerde pagina's binnen het CMS. Deze module biedt mogelijkheden om pagina's aan te maken, te bewerken, te verwijderen en te ordenen. De weergave van de module wordt verzorgd door een **blade**-template, waarin de pagina-items en hun hiërarchie (zoals paginagroepen) overzichtelijk worden weergegeven in een drag-en-drop-interface. Afhankelijk van de gebruikersrechten worden bewerkings- en verwijderopties getoond, evenals knoppen om pagina's te bekijken of te kopiëren.

## Page (model)
Het **Page**-model vormt de kern van de contentstructuur van een website. Dit model vertegenwoordigt individuele pagina's en slaat informatie op, zoals de naam en het toegepaste template. Het **Page**-model bepaalt ook welke content aan specifieke pagina's is gekoppeld via relaties met andere tabellen, zoals het **ManagedContent**-model.

Het model maakt gebruik van **Eloquent**, het Object-Relational Mapping (ORM) systeem van **Laravel**, om met de database te communiceren. Met verschillende functies kan het **Page**-model pagina's genereren op basis van hun template en op basis van de structuur van de URI. Bovendien ondersteunt het model meertalige namen en dynamische URL-generatie die is gebaseerd op de hiërarchie van de content.

## Templates
In de codebase van de webapplicatie kunnen schema's voor pagina's worden aangemaakt op basis van xml-bestanden. Deze zogenoemde templates dienen als sjablonen om te bepalen op welke manier eindgebruikers informatie kunnen toevoegen die op een pagina weergegeven wordt. Hoe deze informatie ingevoerd kan worden, wordt bepaald door middel van **FormFields**.

Binnen templates is het mogelijk om naar andere templates te verwijzen. In dit geval worden ze binnen het systeem geen _templates_ meer genoemd, maar _blocks_. Deze structuur vormt de basis voor de verschillende soorten content die gebruikers op een pagina kunnen definiëren en organiseren.

## FormFields
De **FormField**-modules en bijhorende views bieden een gebruikersinterface waarmee eindgebruikers content kunnen toevoegen of bewerken binnen het CMS. Denk hierbij aan gegevens zoals tekst, afbeeldingen en andere contenttypen.

De weergave van de FormFields gebeurt via **blade**-templates, die de invoer en interactie faciliteren. De specifieke configuratie van een template bepaalt hoe de FormFields worden weergegeven. Elk template kan uit verschillende FormFields bestaan, zoals tekstvelden en keuzelijsten. Dit zorgt voor flexibiliteit die het mogelijk maakt dat pagina's aan de unieke vereisten van de eindgebruiker voldoen.

# Beheren van objecten
Met het CMS kun je ook objecten beheren. Dit omvat verschillende modellen die binnen een applicatie worden gebruikt. Bijvoorbeeld, in een webapplicatie voor een kookwebsite kunnen dit diverse recepten zijn. In een meer abstracte context kan het bijvoorbeeld gaan om transacties binnen een financiële applicatie.

## ObjectManagerController
De ObjectManagerController is verantwoordelijk voor het beheren van objecten. Deze controller extend de Laravel controller. Het dirigeert de mogelijkheden zoals het bewerken en verwijderen van objecten, het beheren van relaties tussen modellen, en het afhandelen van permissies en autorisatie. De controller maakt gebruik van middleware voor authenticatie en taalbeheer en bevat methoden voor het dynamisch ophalen van modellen, het verwerken van relaties en het uitvoeren van acties zoals sorteren, zoeken en ordenen van gegevens binnen het CMS. Eloquent maakt het mogelijk om relaties tussen modellen eenvoudig te definiëren en te beheren.

## Eloquent models
Voor de representatie van de objecten in het CMS wordt gebruikgemaakt van **Eloquent**-modellen. Eloquent is een krachtige objectgeoriënteerde tool die het werken met databasegegevens vereenvoudigt. Elk model komt overeen met een tabel in de database, waarbij elk record in die tabel de gegevens van een specifiek object bevat. Met Eloquent kun je eenvoudig gegevens ophalen, bijwerken, verwijderen en relaties tussen verschillende objecten beheren.

Door Eloquent te gebruiken, kan het CMS op een flexibele manier omgaan met de objecten binnen een webapplicatie.

## CMS schema's
In de codebase van de webapplicaties kunnen op basis xml-bestanden ook CMS-schema's voor objecten worden aangemaakt. Deze schema's dienen als sjabloon voor de weergave en het beheer van objecten in het CMS. Hieronder vallen weergaven voor het aanmaken, bewerken van objecten en het weergeven van een overzicht hiervan. Voor het aanmaken en bewerken van objecten worden, net als bij content-templates, **FormFields** gebruikt. 

In de templates kan ook worden vastgelegd hoe relaties tussen objecten beheerd moeten worden. Denk hierbij aan het toevoegen, bewerken of loskoppelen van gerelateerde objecten vanuit een ander object.

## FormFields
De **FormField**-modules voor objecten biedt een interface voor het invoeren en beheren van gegevens binnen de object templates. Deze modules geven ontwikkelaars de mogelijkheid om verschillende gegevensvelden toe te voegen aan objecten. Dit is vergelijkbaar met hoe het werkt voor pagina's. Elk veld wordt gerepresenteerd doormiddel van een **blade**-template, die de invoeropties en de interactie mogelijk maakt. De configuratie van de FormFields in object templates bepaalt hoe gegevens kunnen worden ingevoerd en weergegeven, en kan variëren afhankelijk van de specifieke vereisten van het object.

# Evaluatie van het CMS

Hoewel het door AllesOnline ontwikkelde CMS functioneel is en een solide basis biedt voor webontwikkeling, kent het systeem aanzienlijke beperkingen op het gebied van ontwikkelaarsvriendelijkheid, onderhoudbaarheid en uitbreidbaarheid. Deze evaluatie richt zich op de architectuur, codestructuur en documentatie, en onderzoekt hoe het huidige CMS voldoet aan de basisprincipes van softwareontwikkeling, zoals de SOLID-principes.

### Gebrekkige Documentatie

Een van de grootste obstakels in het AllesOnline CMS is de beperkte en verouderde documentatie. Hoewel er documentatie beschikbaar is, zoals over de velden die in het CMS beschikbaar zijn via de XML-schema's, ontbreekt het aan meer gedetailleerde en up-to-date informatie over functionaliteiten verschillende functionaliteiten. Dit zorgt voor onduidelijkheid bij ontwikkelaars die niet goed kunnen achterhalen welke parameters voor welke FormModules geschikt zijn. 

_Een voorbeeld van documentatie vind je in de bijlage: [voorbeeld documentatie AllesOnline CMS](Bijlagen/VoorbeeldVanDocumentatieAllesOnlineCms.md)_

**Aanbeveling**:
- Een verbeteringsslag in de documentatie. Inclusief een up-to-date beschrijving van alle beschikbare modules en parameters met voorbeelden van gebruik in verschillende scenario’s voorkomt onduidelijkheid bij ontwikkelaars.
### Complexe Codestructuur

De huidige codestructuur van modules vormt ook een probleem. Deze modules bevatten vaak te veel verantwoordelijkheden, wat het onderhouden en uitbreiden ervan moeilijk maakt. De `FormTagsModule` bijvoorbeeld, verwerkt zowel de relatiebeheerfunctionaliteit als gegevensformattering, wat in strijd is met het **Single Responsibility Principle**.

**Gevolgen van deze complexiteit**:
- De kans op bugs neemt toe, aangezien wijzigingen in een deel van de module vaak onbedoeld andere delen beïnvloeden.
- Het ontbreken van duidelijke interfaces tussen modules beperkt de mogelijkheid om de applicatie uit te breiden of om componenten opnieuw te gebruiken.

**Aanbeveling**:
- Een grondige refactoring van de modules, waarbij de verantwoordelijkheden worden opgesplitst en er duidelijke interfaces worden gedefinieerd.
### Tight Coupling en SOLID principes

Een belangrijk probleem van het AllesOnline CMS is het niet naleven van de **SOLID-principes**, wat de onderhoudbaarheid, uitbreidbaarheid en testbaarheid van de code aanzienlijk belemmert.
#### Kernprincipes

1. **Single Responsibility Principle (SRP)**: Dit principe stelt dat een klasse slechts één reden tot verandering moet hebben. In het AllesOnline CMS zijn veel klassen verantwoordelijk voor meerdere functionaliteiten, wat leidt tot complexiteit en bemoeilijkt het isoleren en hergebruiken van functionaliteiten.
    
2. **Open/Closed Principle (OCP)**: Software-entiteiten moeten open zijn voor uitbreiding, maar gesloten voor aanpassing. In de huidige architectuur vereisen nieuwe functionaliteiten vaak wijzigingen in bestaande code, wat de kans op bugs vergroot en de onderhoudbaarheid bemoeilijkt.
    
3. **Dependency Inversion Principle (DIP)**: Hogere niveau modules moeten afhankelijk zijn van abstracties in plaats van concrete implementaties. In het AllesOnline CMS worden veel afhankelijkheden direct binnen klassen aangeroepen.
     

In de `FormTagsModule` worden deze SOLID-principes geschonden: het **Single Responsibility Principle (SRP)** door gecombineerde verantwoordelijkheden, het **Open/Closed Principle (OCP)** door noodzakelijke aanpassingen voor nieuwe functionaliteiten, en het **Dependency Inversion Principle (DIP)** door directe afhankelijkheden van concrete klassen, wat de onderhoudbaarheid en testbaarheid van de code vermindert.
#### Gevolgen van niet naleven SOLID principes

- **Verlies van herbruikbaarheid**: Functionaliteiten zijn ingebed in grote modules, waardoor hergebruik in andere delen van het CMS lastig is. Dit leidt tot duplicatie van code en inconsistenties in functionaliteit.
    
- **Slechte testbaarheid**: Omdat logica vaak in één module is ondergebracht, is het moeilijk om individuele functionaliteiten te testen zonder de hele module te laden. Dit maakt het schrijven van unit tests onpraktisch.
    
- **Moeilijk onderhoudbaar**: Het onderhoud van een grote module met veel functionaliteiten is uitdagend. Wijzigingen kunnen andere delen van de module beïnvloeden, wat de stabiliteit van het systeem in gevaar brengt.
    
- **Moeilijk uitbreidbaar**: Nieuwe functionaliteiten kunnen niet eenvoudig worden toegevoegd zonder bestaande code aan te passen, wat indruist tegen het OCP.
    
- **Tight coupling**: De huidige architectuur creëert sterke afhankelijkheden tussen verschillende delen van de applicatie, wat de kans op regressies vergroot bij wijzigingen.
    

#### Aanbevelingen voor verbetering

1. **Herziening van de architectuur**: Voer een grondige herziening uit om de SOLID-principes te waarborgen. Ontleed bestaande modules in kleinere, zelfstandige klassen met duidelijke verantwoordelijkheden.
    
2. **Implementatie van interface-gebaseerde ontwikkeling**: Verminder de afhankelijkheid van concrete implementaties door interface-gebaseerde ontwikkeling te introduceren. Dit maakt het systeem flexibeler en beter uitbreidbaar.
    
3. **Modularisatie van functionaliteiten**: Split modules op in kleinere, onafhankelijke eenheden om de herbruikbaarheid en testbaarheid te vergroten.
    
4. **Unit Testing en Continuous Integration**: Ontwikkel een uitgebreide suite van unit tests en implementeer een Continuous Integration (CI) proces om regressies te minimaliseren.
    
5. **Documentatie en training**: Zorg voor documentatie van de nieuwe architecturale richtlijnen en organiseer training voor ontwikkelaars in de SOLID-principes.
    
6. **Iteratieve verbeteringen**: Voer veranderingen iteratief door en evalueer regelmatig de impact op de onderhoudbaarheid, uitbreidbaarheid en testbaarheid van het systeem.

## Conclusie

Het is evident dat het huidige AllesOnline CMS potentie heeft voor verbetering en refactoring. Ondanks de huidige tekortkomingen in de architectuur en de documentatie is het zeker mogelijk om de functionaliteiten van het systeem te optimaliseren. Het is echter belangrijk om te begrijpen dat het verbeteren van de codestructuur en het implementeren van de SOLID-principes ook aanzienlijke investeringen in tijd en middelen vereisen.

Het is aan te raden om deze wijzigingen gefaseerd door te voeren. Dit is noodzakelijk omdat er niet alleen geïnvesteerd moet worden in de ontwikkeling van nieuwe modules, maar ook in het opstellen van uitgebreide documentatie, het aanmaken van geautomatiseerde tests en het trainen van ontwikkelaars in het begrijpen van de SOLID-principes.

Hoewel de voordelen van refactoring aantoonbaar zijn, is het ook noodzakelijk dat AllesOnline, mocht het deze weg willen inslaan, zich voorbereidt op een aanzienlijke inspanning van het ontwikkelaarsteam. Het is dus belangrijk om een balans op te maken tussen de benodigde investeringen en de verwachte voordelen wanneer het project is afgerond. De belangrijkste vraag is: hoe belangrijk is het dat AllesOnline een eigen beheert CMS heeft, als het hoogstwaarschijnlijk voor een kleine investering gebruik kan maken van bestaande en goed onderhouden third-party CMS-pakketten?