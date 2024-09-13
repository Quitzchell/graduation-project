# Onderzoek naar het huidige CMS
## Inleiding
In februari 2015 begon AllesOnline met de ontwikkeling van een eigen Content Management Systeem (CMS). De reden hiervoor was dat de bestaande CMS-pakketten niet goed aansloten bij het Laravel Framework, dat het bedrijf gebruikt om webapplicaties te ontwikkelen voor zijn stakeholders. Bij de ontwikkeling van dit CMS was het belangrijk dat het zowel flexibel genoeg was om aan de specifieke eisen van klanten te voldoen als eenvoudig te integreren in webapplicaties. Dit kwam doordat de belangrijkste inkomstenbron van AllesOnline de ontwikkeling van webapplicaties zelf is, en niet het (door)ontwikkelen van het CMS.

## Doel van het Onderzoek
Het doel van dit onderzoek is om een grondige evaluatie van het bestaande CMS te geven, de uitdagingen te identificeren die voortkomen uit de huidige staat van het systeem, en de noodzaak voor mogelijke modernisering of vervangingen te analyseren. 

## Opbouw
### Beheren van Content
Vanuit het CMS is het mogelijk om content van de website te beheren. Om dit mogelijk te maken zijn er verschillende modules en componenten in het CMS dat samenwerken.
#### ContentManagerModule
De **ContentManagerModule** is verantwoordelijk voor het beheer van alle pagina's in een webapplicatie. Dankzij deze module is het mogelijk om pagina's aan te maken, bewerken, verwijderen en ordenen. De weergave van deze module wordt gerealiseerd door een **Blade**-template, waarmee de bestaande pagina's geordend kunnen worden door middel van slepen van pagina's of paginagroepen.
#### Page (model)
Binnen het CMS wordt het **Page**-model gebruikt om pagina's te representeren. In dit model worden onder andere de naam van de pagina en het toegepaste template opgeslagen. Daarnaast dient het om via gerelateerde tabellen te bepalen welke content aan welke pagina gekoppeld is. Het model is gebaseerd op **Eloquent**, het ORM-systeem van het Laravel-framework.

de **page**-modellen vormen de kern van alle content op een website. Het concept is simpel: zonder pagina's is er geen content.
#### Templates
Binnen de code van de webapplicaties kunnen templates aangemaakt worden op basis van .xml-bestanden. Deze templates dienen als sjabloon om te bepalen op welke manier en wat voor informatie eindgebruikers kunnen meegeven die op een pagina weergegeven moet worden. Hoe de informatie ingevoerd kan worden, wordt bepaald aan de hand van **[FormFields**](#FormField).

Het binnen templates mogelijk om naar andere samenstellingen van **FormFields** te verwijzen. In dit geval heten ze binnen het systeem geen **Template** meer, maar spreken we over **blocks**. Deze structuur vormt de basis voor de verschillende soorten content die gebruikers op een pagina kunnen definiëren en organiseren.
#### FormFields
FormFields bepalen hoe de eindgebruikers informatie kunnen meegegeven die op pagina's weergegeven moet worden. Kortom, alle content van de website wordt doormiddel van FormFields ingegeven. 

De **FormFields** zijn gerealiseerd met zowel componenten voor de gebruikersinterface, als componenten met backend logica die verantwoordelijk zijn voor het persisteren van gegevens.
### Beheren van Objecten

