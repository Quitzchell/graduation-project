# Onderzoek naar het Statamic CMS

## Inleiding

Dit document beschrijft de analyse over het gebruik van Statamic voor het opzetten van een CMS voor de websites die AllesOnline ontwikkelt. Statamic is een commercieel CMS dat veel out-of-the-box functionaliteiten biedt waarmee ontwikkelaars en gebruikers eenvoudig een website kunnen bouwen en beheren.


## Over Statamic

Net zoals het AllesOnline CMS en Filament maakt Statamic gebruik van **Laravel**, waardoor het mogelijk is om de functionaliteiten van Laravel te benutten bij het ontwikkelen van een systeem.

De basisinstallatie van Statamic komt met een volledig CMS waarin een overvloed aan functionaliteiten beschikbaar is voor het beheren van content. De kernonderdelen zijn:

* Assets: voor het beheren van afbeeldingen en video's.
* Collections: voor het opzetten van structuur en het organiseren van content en objecten.
* Entries: records van objecten en/of content.
* Fields: verschillende soorten invoervelden voor content.
* Globals: herbruikbare informatie die globaal kan worden opgehaald.
* Search: out-of-the-box ondersteuning voor zoekfunctionaliteit, zowel on-the-fly als met behulp van Algolia.
* Navigation: beheren van navigatiemenu's via een drag & drop interface.
* Taxonomies: functionaliteit voor het categoriseren en taggen van content voor globale organisatie van content.
* Users: Voor het beheren van gebruikersgegevens.

> _Voor uitgebreide informatie over het ontwikkelen met Statamic kan de officiele documentatie geraadpleegd worden._
> * [Statamic Documentatie](https://statamic.dev/)

> _Er is ook een videocursus beschikbaar van de maker Jack McDade via Laracasts._
> * [Laracast cursus](https://laracasts.com/series/learn-statamic-with-jack)

## Uitbreidbaarheid van Statamic

Net zoals bij Filament heeft Statamic een marketplace waar oplossingen aangeboden worden die niet standaard in Statamic zijn geïmplementeerd. Deze in Statamic genoemde `Addons`, die zowel gratis als tegen betaling beschickbaar zijn, zijn ook hier ontwikkeld door het Statamic-team of door externe partijen. Ook binnen Statamic is het mogelijk om eigen componenten via de marketplace aan te bieden. Voor de ontwikkeling van deze Addons is het aan te raden om gebruik te maken van Vue, Laravel, Tailwind en Vite. 

> _Documentatie voor het realiseren van Addons voor Statamic_
> * [Addons documentatie](https://statamic.dev/extending/addons)

> _Laracast video over het realiseren van een Statamic Addon_
> * [Laracast video](https://laracasts.com/series/learn-statamic-with-jack/episodes/15)

## Beheren van content en objecten

Statamic biedt flexibiliteit rondom het persisteren en beheren van content en objecten. Standaard maakt het systeem gebruik van een `flat-file` architectuur, maar er kan gekozen worden om Statamic te configuereren om gebruik te maken van databases zoals MySQL of MongoDB. 

### Collections 

In Statamic worden `Collections` gebruikt om groepen gerelateerde content-items te organiseren. Elke collectie fungeert als een container voor `Entries`, zoals pagina's, blogposts of films. De entries binnen deze collections maken gebruik van voorgedefinieerde `Blueprints` die de datastructuur bepalen. 

In het geval dat er gebruik gemaakt wordt van de flat-file architectuur, worden alle entries van een collectie onder dezelfde directory in de repository opgeslagen. 

Collections kunnen via het Control Panel van Statamic of aan de hand van YAML-bestanden in de repository gedefinieerd worden.

> _Voorbeeld van een Collections YAML_
> [Collections yaml voor Pages collection](../Bijlagen/VoorbeeldStatamicCollectionsFile.md)

### Blueprints

Zoals bij collections al is benoemd, worden `Blueprints` gebruikt om te definiëren wat de datastructuur voor `Entries` binnen een collection is. Deze structuur wordt bepaald door velden aan de blueprints toe te voegen. Om eindgebruikers in staat te stellen op een dynamische manier gegroepeerde velden voor contentblokken te beheren, kan in Statamic gebruik worden gemaakt van het `Replicator`-veld. Dit stelt gebruikers in staat om onder andere voorgedefinieerde `Fieldsets` aan de content toe te voegen.

### FieldSets

`Fieldsets` zijn voorgedefineerde gegroepeerde velden en bieden de mogelijkheid om op een consistente manier herbruikbare contentblokken te creeren die eindgebruikers eenvoudig kunnen toevoegen en ordenen.

### Entries



### flat-files

De standaard confugiratie voor Statamic maakt gebruik van `flat-file`. Dit houdt in dat, inplaats van dat gegevens opgeslagen worden in een databse, gegevens gepersisteert worden in de codebase binnen markdown bestanden. 

> _voorbeeld van een flat-file markdown bestand_
> [Flat-file voor homepage](../Bijlagen/VoorbeeldStatamicFlatFile.md)

Wanneer wordt gekozen voor de `flat-file` architectuur, is het de bedoeling dat deze bestanden met behulp van Git worden beheerd. Door de flat-files op te nemen in een Git-repository kunnen wijzigingen worden bijgehouden en kunnen anderen eenvoudig terugkeren naar eerdere versies van de content.

### Via database en Eloquent

Het is ook mogelijk om de 

## Evaluatie van Statamic

### Documenatie en ondersteuning

Hoewel Statamic vrijwel al zijn functionaliteiten beschrijft in de documentatie, is deze vaak oppervlakkig of simpelweg incompleet. Tijdens de ontwikkeling van het prototype is veel informatie over de functionaliteiten uit GitHub-issues gehaald of is de door Statamic opgezette Discord-server geraapleegd. 

**aanbevelingen**:
- Ook voor Statamic is het nuttig om bij te dragen aan de community. Neem samen met het team deel aan de Discord-community, waar de ontwikkelaars van AllesOnline in contact komen met externe ontwikkelaars. Dit kan bijdragen aan het verbreden van kennis over Statamic, maar ook aan het verbreden van inzichten op het gebied van software- en webdevelopment.

### Gebruik van Vue 2

Statamic maakt gebruik van Vue 2 voor de views in het control panel. Dit is relatief opmerkelijk, aangezien Vue 2 op 31 december 2023 het einde van zijn levenscyclus (EOL) bereikte. Volgens de roadmap van Statamic is het de bedoeling om in de lente van 2025 over te stappen naar Vue 3. Voor ontwikkelaars die custom componenten in Statamic hebben gebouwd, betekent dit dat zij hun componenten ook moeten migreren naar Vue 3.

Daarnaast heeft momenteel slechts één van de ontwikkelaars bij AllesOnline ervaring met het werken met Vue.

**aanbevelingen**: Bied ontwikkelaars de gelegenheid om ervaring en kennis op te doen met Vue.
