# Onderzoek naar het AllesOnline CMS
# Inleiding
In dit document wordt de analyse van het door AllesOnline ontwikkelde CMS-systeem gepresenteerd. Het onderzoek richt zich op het identificeren van knelpunten binnen de huidige structuur en functionaliteit van het CMS. Het doel van dit onderzoek is om een evaluatie van het bestaande CMS te geven, de uitdagingen te identificeren die voortkomen uit de huidige staat van het systeem, en de noodzaak voor mogelijke modernisering of vervangingen te analyseren. 

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

Hoewel het door AllesOnline ontwikkelde CMS functioneel is en een solide basis biedt voor webontwikkeling, kent het systeem aanzienlijke beperkingen op het gebied van ontwikkelaar-vriendelijkheid, onderhoudbaarheid en uitbreidbaarheid. Deze evaluatie richt zich op de architectuur, codestructuur en documentatie, en onderzoekt hoe het huidige CMS voldoet aan de basisprincipes van softwareontwikkeling, zoals de SOLID-principes.

### Gebrekkige Documentatie

Een van de grootste obstakels in het AllesOnline CMS is de beperkte en verouderde documentatie. Hoewel er documentatie beschikbaar is over de basisfunctionaliteiten, zoals de velden in de XML-schema's, ontbreekt het aan meer gedetailleerde en up-to-date informatie over functionaliteiten. Dit zorgt voor onduidelijkheid bij ontwikkelaars die niet goed kunnen achterhalen welke parameters voor welke FormModules geschikt zijn. _Een voorbeeld van documentatie vind je in de bijlage: [voorbeeld documentatie AllesOnline CMS](bijlagen/VoorbeeldVanDocumentatieAllesOnlineCms.md)_

**Aanbeveling**:
- Een verbeteringsslag in de documentatie. Inclusief een up-to-date beschrijving van alle beschikbare modules en parameters met voorbeelden van gebruik in verschillende scenario’s voorkomt onduidelijkheid bij ontwikkelaars.
### Complexe Codestructuur

De huidige codestructuur van modules vormt ook een probleem. Deze modules bevatten vaak te veel verantwoordelijkheden, wat het onderhouden en uitbreiden ervan moeilijk maakt. De `FormTagsModule` bijvoorbeeld, verwerkt zowel de relatiebeheerfunctionaliteit als gegevensformattering, wat in strijd is met het **Single Responsibility Principle**.

**Gevolgen van deze complexiteit**:
- De kans op bugs neemt toe, aangezien wijzigingen in een deel van de module vaak onbedoeld andere delen beïnvloeden.
- Het ontbreken van duidelijke abstracties zorgt voor een lange leercurve voor nieuwe ontwikkelaars.

**Aanbeveling**:
- Refactor de modules door de verantwoordelijkheden van de module te scheiden in kleinere, meer specifieke classes. Bijvoorbeeld een aparte klasse voor relatiebeheer en een aparte klasse voor gegevensformattering.

### Tight Coupling 

Een groot probleem van het AllesOnline CMS is het niet naleven van de **SOLID-principes**, wat gevolgen heeft voor de onderhoudbaarheid, uitbreidbaarheid en testbaarheid van de code. Dit komt duidelijk naar voren in de `FormTagsModule`, maar geldt ook voor andere delen van het CMS.

Laten we specifiek ingaan op het **Single Responsibilty Principle (SRP)**, **Open/Closed Principe (OCP)** en **Dependency Inversion Principle (DIP)** en hoe het gebrek hieraan de onderhoudbaarheid, uitbreidbaarheid en testbaarheid van de code bemoeilijkt.

### Single Responsibility Principle (SRP)

Het Single Responsibility Principle (SRP) stelt dat:
- Een class slechts één reden tot verandering zou moeten hebben.
- Dit houdt in dat elke class zich moet richten op een enkele verantwoordelijkheid en niet meerdere functionaliteiten moet combineren.

In het AllesOnline CMS zijn veel voorbeelden te vinden van classes die meerdere verantwoordelijkheden samenbrengen. Dit leidt tot een verhoogde complexiteit, omdat ontwikkelaars gedwongen worden om met een module te werken die verantwoordelijk is voor alle logica. Hierdoor wordt het moeilijk om specifieke functionaliteiten in isolatie te testen of opnieuw te gebruiken in andere delen van de applicatie. 
### Open/Closed Principle (OCP)

Het Open/Closed Principle (OCP) stelt dat:
- Software-entiteiten (klassen, modules, functies, enz.) open moeten zijn voor uitbreiding, maar gesloten voor aanpassing.
- Dit betekent dat het mogelijk moet zijn om nieuwe functionaliteit toe te voegen aan bestaande systemen zonder de bestaande code te hoeven wijzigen.

Momenteel is de architectuur van het AllesOnline CMS niet in lijn met het OCP. Nieuwe functionaliteiten vereisen vaak wijzigingen in de bestaande code. Bijvoorbeeld, wanneer er nieuwe functionaliteiten aan modules moeten worden toegevoegd moeten ontwikkelaars vaak al bestaande methodes aanpassen of uitbreiden. Dit leidt niet alleen tot een verhoogd risico op bugs en regressies, maar maakt het ook moeilijker om de code te onderhouden en te begrijpen.

#### **Dependency Inversion Principle (DIP)**

Het **Dependency Inversion Principle (DIP)** stelt dat:
- Hogere niveau modules niet afhankelijk zouden moeten zijn van lagere niveau modules, maar van abstracties (interfaces of abstracte classes).
- Abstracties zouden niet afhankelijk moeten zijn van details; details moeten afhankelijk zijn van abstracties.

Momenteel worden in het AllesOnline CMS veel afhankelijkheden (dependencies) direct binnen de classes aangeroepen. Een voorbeeld hiervan is de `FormTagsModule`, waarbij de formatteringslogica hard-coded is, zonder gebruik te maken van interfaces of abstracties. Dit leidt tot een sterke afhankelijkheid van concrete, hardcoded implementaties, wat het moeilijk maakt om onderdelen van de code te vervangen of te testen zonder de volledige module te laden.
#### Gebrek aan onafhankelijkheid

Een van de meest significante tekortkomingen van de huidige architectuur is dat er uberhaupt geen losse classes zijn voor de verschillende functionaliteiten binnen de modules. Alle logica is ingebouwd in een enkele module, wat betekent dat verantwoordelijkheden niet goed zijn gescheiden. Dit maakt het onmogelijk om specifieke functionaliteiten in isolatie te testen of opnieuw te gebruiken in andere delen van de applicatie.

**Gevolgen:**
* **Verlies van Herbruikbaarheid**: Wanneer functionaliteiten zijn ingebouwd in één grote module, kunnen ze niet eenvoudig in andere modules of contexten worden hergebruikt. Dit leidt tot duplicatie van code wanneer ontwikkelaars vergelijkbare functionaliteiten in andere delen van het CMS willen implementeren. Over tijd veroorzaakt dit inconsistenties in de werking van een bepaalde functionaliteit.
    
* **Slechte Testbaarheid**: Aangezien alle logica vaak in één module is ondergebracht, is het moeilijk om individuele functionaliteiten te testen zonder de hele module te laden. Dit verhoogt de complexiteit van tests en maakt het onpraktisch om unit tests te schrijven, omdat ontwikkelaars gedwongen worden om met de volledige module te werken, inclusief al zijn afhankelijkheden.
    
* **Moeilijk Onderhoudbaar**: Het onderhoud van een grote module met een overvloed aan functionaliteiten is bijzonder uitdagend. Wanneer er wijzigingen of bugfixes moeten worden doorgevoerd, is de kans groter dat andere delen van de module worden beïnvloed, wat de kans op regressies vergroot en de totale stabiliteit van het systeem in gevaar brengt.
    
* **Moeilijk Uitbreidbaar:** Omdat de modules afhankelijk zijn van concrete implementaties in plaats van abstracties, is het moeilijk om nieuwe functionaliteiten toe te voegen zonder bestaande code aan te passen. Dit gaat direct in tegen het **Open/Closed Principle (OCP)**, wat stelt dat klassen open moeten zijn voor uitbreiding, maar gesloten voor aanpassing.
    
* **Tight Coupling:** De huidige architectuur creëert tight coupling tussen verschillende delen van de applicaties. Dit betekent dat wanneer een deel van de module verandert, andere delen ook beïnvloed kunnen worden, wat de kans op regressies vergroot. Dit maakt het moeilijk om snel aanpassingen of verbeteringen door te voeren zonder onverwachte bugs te introduceren.
    

**Aanbevelingen**
- **Herziening van de architectuur**:
    - Voer een uitgebreide herziening van de huidige architectuur uit om de naleving van de SOLID-principes te waarborgen. Dit kan inhouden dat bestaande modules worden ontleed in kleinere, zelfstandige classes met duidelijk gedefinieerde verantwoordelijkheden.
      
- **Implementatie van interface-gebaseerde ontwikkeling**:
    - Introduceer interface-gebaseerde ontwikkeling om de afhankelijkheid van concrete implementaties te verminderen. Dit helpt bij het creëren van een flexibeler en uitbreidbaar systeem dat beter kan inspelen op veranderende eisen.
      
- **Modularisatie van functionaliteiten**:
    - Split modules op in kleinere, onafhankelijke eenheden. Dit vergroot de herbruikbaarheid van code en maakt het eenvoudiger om specifieke functionaliteiten in isolatie te testen.
      
- **Unit Testing en Continuous Integration**:
    - Ontwikkel een uitgebreide suite van unit tests voor de verschillende modules. Combineer dit met een Continuous Integration (CI) proces om ervoor te zorgen dat elke wijziging in de codebasis wordt getest, wat de kans op regressies vermindert.
      
- **Documentatie en Training**:
    - Zorg voor adequate documentatie over de nieuwe architecturale richtlijnen en principes. Organiseer training voor ontwikkelaars om hen bekend te maken met de SOLID-principes en de nieuwe ontwikkelingsstandaarden.
      
- **Iteratieve Verbeteringen**:
    - Voer veranderingen iteratief door en evalueer regelmatig de impact van deze wijzigingen op de onderhoudbaarheid, uitbreidbaarheid en testbaarheid van het systeem. Dit zorgt ervoor dat het team kan inspelen op nieuwe inzichten en voortdurend kan verbeteren.
      