# Onderzoek naar het Statamic CMS

## Inleiding

Dit document beschrijft de analyse over het gebruik van Statamic voor het opzetten van een CMS voor de websites die AllesOnline ontwikkelt. Statamic is een commercieel CMS dat veel out-of-the-box functionaliteiten biedt waarmee ontwikkelaars en gebruikers eenvoudig een website kunnen bouwen en beheren.


## Over Statamic

Net zoals het AllesOnline CMS en Filament maakt Statamic gebruik van **Laravel**, waardoor het mogelijk is om de functionaliteiten van Laravel te benutten bij het ontwikkelen van een systeem.

De basisinstallatie van Statamic komt met een volledig CMS waarin een overvloed aan functionaliteiten beschikbaar is voor het beheren van content. De kernonderdelen zijn:

* Assets: voor het beheren van afbeeldingen en video's.
* Configuration: aparte configuratie voor het instellen van Statamic.
* Collections: voor het opzetten van structuur en het organiseren van content en objecten.
* Fields: verschillende soorten invoervelden voor content.
* Globals: herbruikbare informatie die globaal opgehaald kan worden.
* Search: out-of-the-box ondersteuning voor zoekfunctionaliteit, inclusief Algolia.
* Navigation: beheren van navigatiemenu's via een drag & drop interface.
* Taxonomies: functionaliteit voor het categoriseren en taggen van content voor globale organisatie van content.
* Users: Voor het beheren van gebruikersgegevens.

Net zoals bij Filament is het mogelijk om zelf componenten te ontwikkelen voor Statamic. Statamic biedt, net als Filament, een marketplace waar developers hun aangepaste componenten beschikbaar kunnen stellen aan anderen, eventueel tegen betaling.

## Beheren van content

### Flat-files

De standaard confugiratie voor Statamic maakt gebruik van `flat-file`. Dit houdt in dat, inplaats van dat gegevens opgeslagen worden in een databse, gegevens gepersisteert worden in de codebase binnen markdown bestanden. 

> _voorbeeld van een flat-file markdown bestand_
> [Flat-file voor homepage](../Bijlagen/VoorbeeldStatamicFlatFile.md)

Wanneer wordt gekozen voor de `flat-file` architectuur, is het de bedoeling dat deze bestanden met behulp van Git worden beheerd. Door de flat-files op te nemen in een Git-repository kunnen wijzigingen worden bijgehouden en kunnen anderen eenvoudig terugkeren naar eerdere versies van de content.

### Via database en Eloquent


## Evaluatie van Statamic

### Documenatie en ondersteuning

Hoewel Statamic vrijwel al zijn functionaliteiten beschrijft in de documentatie, is deze vaak oppervlakkig of simpelweg incompleet. Tijdens de ontwikkeling van het prototype is veel informatie over de functionaliteiten uit GitHub-issues gehaald of is de door Statamic opgezette Discord-server geraapleegd. 

**aanbevelingen**:
- Ook voor Statamic is het nuttig om bij te dragen aan de community. Neem samen met het team deel aan de Discord-community, waar de ontwikkelaars van AllesOnline in contact komen met externe ontwikkelaars. Dit kan bijdragen aan het verbreden van kennis over Statamic, maar ook aan het verbreden van inzichten op het gebied van software- en webdevelopment.

### Gebruik van Vue 2

Statamic maakt gebruik van Vue 2 voor de views in het control panel. Dit is relatief opmerkelijk, aangezien Vue 2 op 31 december 2023 het einde van zijn levenscyclus (EOL) bereikte. Volgens de roadmap van Statamic is het de bedoeling om in de lente van 2025 over te stappen naar Vue 3. Voor ontwikkelaars die custom componenten in Statamic hebben gebouwd, betekent dit dat zij hun componenten ook moeten migreren naar Vue 3.

Daarnaast heeft momenteel slechts één van de ontwikkelaars bij AllesOnline ervaring met het werken met Vue.

**aanbevelingen**: Bied ontwikkelaars de gelegenheid om ervaring en kennis op te doen met Vue.
