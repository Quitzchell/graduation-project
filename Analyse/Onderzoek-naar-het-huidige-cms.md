# Onderzoek naar het huidige CMS
## Inleiding
In februari 2015 begon AllesOnline met de ontwikkeling van een eigen Content Management Systeem (CMS). De reden hiervoor was dat de bestaande CMS-pakketten niet goed aansloten bij het Laravel Framework, dat het bedrijf gebruikt om webapplicaties te ontwikkelen voor zijn stakeholders. Bij de ontwikkeling van dit CMS was het belangrijk dat het zowel flexibel genoeg was om aan de specifieke eisen van klanten te voldoen als eenvoudig te integreren in webapplicaties. Dit kwam doordat de belangrijkste inkomstenbron van AllesOnline de ontwikkeling van webapplicaties zelf is, en niet het (door)ontwikkelen van het CMS.

## Doel van het Onderzoek
Het doel van dit onderzoek is om een grondige evaluatie van het bestaande CMS te geven, de uitdagingen te identificeren die voortkomen uit de huidige staat van het systeem, en de noodzaak voor mogelijke modernisering of vervangingen te analyseren. 

## Opbouw
### Beheren van Content
Vanuit het CMS is het mogelijk om content van de website te beheren. Om dit mogelijk te maken zijn er verschillende modules en componenten in het CMS dat samenwerken.
#### ContentManagerController
De ContentManagerController beheert de content binnen het CMS en biedt functionaliteiten voor het toevoegen, bewerken verwijderen en kopiëren van pagina's. De belangrijkste onderdelen zijn: 
1. **Contentbeheer**: 
	* **Toevoegen/Bewerken**: Pagina's worden toegevoegd via `getAdd` en `postAdd`, en bewerkt via `getEdit` en `postEdit`. Na elke actie worden wijzigingen opgelsagen en events zoals `PageWasUpdated` getriggerd.
	* **Verwijderen**: `postDelete` verwijderd een pagina
	* **Kopiëren**: `getCopy` maakt een kopie van een pagina en zijn content.
2. **Middleware en permissies:** Zorgt via `lang` en `can` middle dat acties afhankelijk zijn van de juiste taal en gebruikersrechten.
3. **Helpers**: 
	* **Templates:** Verkrijgt beschikbare templates voor een pagina.
	* **URL-generatie**: Biedt methodes voor het genereren van URL's voor content.
4. **Events en Models:** Werkt samen met modellen zoals `Page` en `ManagedContent` en gebruikt events zoals `PageWasUpdated` voor systeemreacties op wijzigingen.
#### ContentManagerModule
De **ContentManagerModule** en bijhorende view zijn verantwoordelijk voor het tonen en beheren van hiërarchische georganiseerde pagina in het CMS. Deze module maakt het mogelijk om pagina's aan te maken, bewerken, verwijderen en te ordenen. De weergave van de module wordt gerealiseerd door een **blade**-template, waarin de pagina-items en hun hiërarchie (zoals paginagroepen) worden weergegeven in een gemakkelijk te ordenen drag-en-drop interface. Afhankelijk van de rechten van de gebruiker worden er bewerkings- en verwijderingopties getoond, evenals knoppen om pagina's te bekijken of te kopiëren.
#### Page (model)
Binnen het CMS wordt het **Page**-model gebruikt om pagina's te representeren. In dit model worden onder andere de naam van de pagina en het toegepaste template opgeslagen. Daarnaast dient het om via gerelateerde tabellen te bepalen welke content aan welke pagina gekoppeld is. Het model is gebaseerd op **Eloquent**, het ORM-systeem van het Laravel-framework.

de **page**-modellen vormen de kern van alle content op een website. Het concept is simpel: zonder pagina's is er geen content.
#### Templates
Binnen de code van de webapplicaties kunnen templates aangemaakt worden op basis van .xml-bestanden. Deze templates dienen als sjabloon om te bepalen op welke manier en wat voor informatie eindgebruikers kunnen meegeven die op een pagina weergegeven moet worden. Hoe de informatie ingevoerd kan worden, wordt bepaald aan de hand van **FormFields**.

Het binnen templates mogelijk om naar andere samenstellingen van **FormFields** te verwijzen. In dit geval heten ze binnen het systeem geen **Template** meer, maar spreken we over **blocks**. Deze structuur vormt de basis voor de verschillende soorten content die gebruikers op een pagina kunnen definiëren en organiseren.
#### FormFields
FormFields bepalen hoe de eindgebruikers informatie kunnen meegegeven die op pagina's weergegeven moet worden. Kortom, alle content van de website wordt doormiddel van FormFields ingegeven. 

De **FormFields** zijn gerealiseerd met zowel componenten voor de gebruikersinterface, als componenten met backend logica die verantwoordelijk zijn voor het persisteren van gegevens.
### Beheren van Objecten
Met het CMS kun je ook objecten beheren. Dit omvat verschillende modellen die binnen een applicatie worden gebruikt. Bijvoorbeeld, in een webapplicatie voor een kookwebsite kunnen dit diverse recepten zijn. In een meer abstracte context kan het bijvoorbeeld gaan om transacties binnen een financiële applicatie.

#### ObjectManagerController
Om de objecten te beheren, is er de ObjectManagerController, een controller die de Laravel controller extend. Het biedt mogelijkheden zoals het bewerken en verwijderen van objecten, het beheren van relaties tussen modellen, en het afhandelen van permissies en autorisatie. De controller maakt gebruik van middleware voor authenticatie en taalbeheer en bevat methoden voor het dynamisch ophalen van modellen, het verwerken van relaties en het uitvoeren van acties zoals sorteren, zoeken en ordenen van gegevens.

#### Eloquent models

#### Templates

#### FormFields
