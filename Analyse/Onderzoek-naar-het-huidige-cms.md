# Onderzoek naar het huidige CMS
## Inleiding
In februari 2015 begon AllesOnline met de ontwikkeling van een eigen Content Management Systeem (CMS). De reden hiervoor was dat de bestaande CMS-pakketten niet goed aansloten bij het Laravel Framework, dat het bedrijf gebruikt om webapplicaties te ontwikkelen voor zijn stakeholders. Bij de ontwikkeling van dit CMS was het belangrijk dat het zowel flexibel genoeg was om aan de specifieke eisen van klanten te voldoen als eenvoudig te integreren in webapplicaties. Dit kwam doordat de belangrijkste inkomstenbron van AllesOnline de ontwikkeling van webapplicaties zelf is, en niet het (door)ontwikkelen van het CMS.

## Doel van het Onderzoek
Het doel van dit onderzoek is om een grondige evaluatie van het bestaande CMS te geven, de uitdagingen te identificeren die voortkomen uit de huidige staat van het systeem, en de noodzaak voor mogelijke modernisering of vervangingen te analyseren. 


## Beheren van Content
Het CMS biedt de mogelijkheid om de content van de website te beheren. Dit wordt gerealiseerd door verschillende modules en componenten die in het CMS samenwerken.
### ContentManagerController
De **ContentManagerController** beheert de content binnen het CMS en biedt functionaliteiten voor het toevoegen, bewerken, verwijderen en kopiëren van pagina's. De belangrijkste onderdelen zijn: 
1. **Contentbeheer**: 
	* **Toevoegen/Bewerken**: Pagina's worden toegevoegd via `getAdd` en `postAdd`, en bewerkt via `getEdit` en `postEdit`. Na elke actie worden wijzigingen opgelsagen en events zoals `PageWasUpdated` getriggerd.
	* **Verwijderen**: `postDelete` verwijderd een pagina
	* **Kopiëren**: `getCopy` maakt een kopie van een pagina en zijn content.
2. **Middleware en permissies:** Zorgt via `lang` en `can` middle dat acties afhankelijk zijn van de juiste taal en gebruikersrechten.
3. **Helpers**: 
	* **Templates:** Verkrijgt beschikbare templates voor een pagina.
	* **URL-generatie**: Biedt methodes voor het genereren van URL's voor content.
4. **Events en Models:** Werkt samen met modellen zoals `Page` en `ManagedContent` en gebruikt events zoals `PageWasUpdated` voor systeemreacties op wijzigingen.
### ContentManagerModule
De **ContentManagerModule** en de bijbehorende view zijn verantwoordelijk voor het tonen en beheren van hiërarchisch georganiseerde pagina's binnen het CMS. Deze module biedt mogelijkheden om pagina's aan te maken, te bewerken, te verwijderen en te ordenen. De weergave van de module wordt verzorgd door een **blade**-template, waarin de pagina-items en hun hiërarchie (zoals paginagroepen) overzichtelijk worden weergegeven in een drag-en-drop-interface. Afhankelijk van de gebruikersrechten worden bewerkings- en verwijderopties getoond, evenals knoppen om pagina's te bekijken of te kopiëren.
### Page (model)
Het **Page**-model vormt de kern van de contentstructuur van een website. Dit model vertegenwoordigt individuele pagina's en slaat essentiële informatie op, zoals de naam en het toegepaste template. Het **Page**-model bepaalt ook welke content aan specifieke pagina's is gekoppeld via relaties met andere tabellen, zoals het **ManagedContent**-model.

Het model maakt gebruik van **Eloquent**, het ORM-systeem van Laravel, om efficiënt met de database te communiceren. Met functies zoals **getTemplatePage** kan het **Page**-model pagina's vinden op basis van hun template, terwijl de functie **createForUri** nieuwe pagina's kan genereren op basis van een URI-structuur. Daarnaast biedt het model ondersteuning voor meertalige namen en dynamische URL-generatie op basis van de contenthiërarchie.
### Templates
In de codebase van de webapplicatie kunnen templates worden aangemaakt op basis van .xml-bestanden. Deze templates dienen als sjablonen om te bepalen op welke manier en welke informatie eindgebruikers kunnen toevoegen die op een pagina weergegeven moet worden. Hoe deze informatie ingevoerd kan worden, wordt bepaald door middel van **FormFields**.

Binnen templates is het mogelijk om naar andere templates te verwijzen. In dit geval worden ze binnen het systeem geen _templates_ meer genoemd, maar _blocks_. Deze structuur vormt de basis voor de verschillende soorten content die gebruikers op een pagina kunnen definiëren en organiseren.
### FormFields
De **FormField**-modules en de bijbehorende views zijn verantwoordelijk voor het genereren van invoervelden waarmee eindgebruikers content kunnen toevoegen of bewerken binnen het CMS. Deze module biedt een interface waarmee verschillende soorten gegevens kunnen worden ingevoerd, zoals tekst, afbeeldingen, en andere contenttypen. De velden worden op de pagina weergegeven via een gestandaardiseerde **blade**-template die de invoeropties overzichtelijk maakt en gebruiksvriendelijke interactie mogelijk maakt.

De weergave van de FormFields is afhankelijk van de configuratie van de specifieke template waarop ze worden toegepast. Elk formulier kan worden samengesteld uit verschillende FormFields, die flexibel ingezet worden om aan de specifieke vereisten van de pagina te voldoen.
## Beheren van Objecten
Met het CMS kun je ook objecten beheren. Dit omvat verschillende modellen die binnen een applicatie worden gebruikt. Bijvoorbeeld, in een webapplicatie voor een kookwebsite kunnen dit diverse recepten zijn. In een meer abstracte context kan het bijvoorbeeld gaan om transacties binnen een financiële applicatie.
### ObjectManagerController
De ObjectManagerController is verantwoordelijk voor het beheren van objecten. Deze controller extend de Laravel controller. Het biedt mogelijkheden zoals het bewerken en verwijderen van objecten, het beheren van relaties tussen modellen, en het afhandelen van permissies en autorisatie. De controller maakt gebruik van middleware voor authenticatie en taalbeheer en bevat methoden voor het dynamisch ophalen van modellen, het verwerken van relaties en het uitvoeren van acties zoals sorteren, zoeken en ordenen van gegevens. Eloquent maakt het mogelijk om relaties tussen modellen eenvoudig te definiëren en te beheren.
### Eloquent models
Voor de representatie van de objecten in het CMS wordt gebruikgemaakt van **Eloquent**-modellen. Eloquent is een krachtige objectgeoriënteerde tool die het werken met databasegegevens vereenvoudigt. Elk model komt overeen met een tabel in de database, waarbij elk record in die tabel de gegevens van een specifiek object bevat. Met Eloquent kun je eenvoudig gegevens ophalen, bijwerken, verwijderen en relaties tussen verschillende tabellen beheren.

Door Eloquent te gebruiken, kan het CMS op een flexibele manier omgaan met de objecten binnen een webapplicatie.
### Templates
In de codebase van de webapplicaties kunnen op basis van .xml-bestanden templates voor objecten worden aangemaakt. Deze templates dienen als blauwdruk voor de weergave en het beheer van objecten in het CMS. Hieronder vallen weergaven voor het aanmaken, bewerken van objecten en het weergeven van een overzicht hiervan. Voor het aanmaken en bewerken van objecten worden, net als bij content-templates, **FormFields** gebruikt. 

In de templates kan ook worden vastgelegd hoe relaties tussen objecten beheerd moeten worden. Denk hierbij aan het toevoegen, bewerken of loskoppelen van gerelateerde objecten vanuit een ander object.

### FormFields
De **FormField**-modules voor objecten biedt een interface voor het invoeren en beheren van gegevens binnen de object templates. Deze modules laten gebruikers toe om verschillende gegevensvelden toe te voegen aan objecten, vergelijkbaar met hoe het werkt voor pagina's. Elk veld wordt gepresenteerd op een gestandaardiseerde **blade**-template, die de invoeropties overzichtelijk maakt en de interactie vergemakkelijkt. De configuratie van de FormFields in object templates bepaalt hoe gegevens moeten worden ingevoerd en weergegeven, en kan variëren afhankelijk van de specifieke vereisten van de object template.