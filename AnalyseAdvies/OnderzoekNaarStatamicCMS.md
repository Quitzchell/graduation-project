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

In de standaard flat-file configuratie voor Statamic worden `Entries` gebruikt om content als Markdown-bestand te beheren en persisteren binnen de bijhorende collection.  

> _Voorbeeld van entry binnen Page collection voor de hoempage_
> * [Markdown voor homepage](../Bijlagen/VoorbeeldStatamicFlatFile.md)


### Meer over flat-file 

In de standaard flat-file configuratie van Statamic worden `Entries` opgeslagen als Markdown-bestanden binnen de bijbehorende collecties. Deze configuratie biedt enkele voordelen:

* **Geen database**: een vereenvoudigde infrastructuur zonder afhankelijkheid van een database.
* **Snelheid**: directe toegang tot bestanden zorgt voor snellere laadtijden, aangezien er geen database-queries nodig zijn.
* **Versiebeheer**: omdat de bestanden worden meegenomen in de Git-repository, zijn wijzigingen eenvoudig te traceren en terug te draaien.

Uiteraard kent de flat-file configuratie ook enkele nadelen die in overweging moeten worden genomen:

**Beperkte schaalbaarheid**: bij grotere projecten met veel data of complexe queries blijven de prestaties van de flat-file architectuur achter bij die van een geoptimaliseerde database.
* **Conflicten**: als meerdere gebruikers tegelijkertijd bestanden bewerken, kan dit leiden tot mergeconflicten in het versiebeheer.
**Beperktere mogelijkheden voor complexe relaties**: objecten met complexere relaties, zoals many-to-many-relaties, kunnen momenteel niet goed worden ondersteund in de flat-file architectuur van Statamic. Het is mogelijk om via een Observer wijzigingen in beide objecten bij te houden om een many-to-many-relatie te realiseren, maar dat vrij omslachtig.

Voor projecten die een hogere schaalbaarheid vereisen, complexere relaties hebben, of waarbij gelijktijdige bewerkingen door meerdere gebruikers vaker voorkomen, biedt Statamic de mogelijkheid om een configuratie te gebruiken waarin een database wordt ingezet.

### Via database en Eloquent

Statamic kan met behulp van de officiële `eloquent-driver` worden geconfigureerd om een database te gebruiken via Laravel's Eloquent ORM. Deze aanpak biedt verbeterde schaalbaarheid en flexibiliteit. Hoewel Eloquent in deze configuratie wordt gebruikt, worden niet alle entiteiten binnen je systeem volledig als Eloquent-modellen geïmplementeerd. In plaats daarvan worden gegevens die voorheen als flat-files in Statamic werden opgeslagen, nu in de database bewaard, waarbij veel inhoud van Entries of instellingen voor een Collection als JSON wordt opgeslagen. 

Mocht je wel specifieke modellen en databasetabellen willen gebruikenb, dan is er de Runway addon, ontwikkeld door The Rad Pack, een groep ontwikkelaars die nauw samenwerken met het Statamic-team. Met Runway kun je Eloquent-modellen in Statamic weergeven en beheren, waardoor je meer flexibiliteit hebt bij het beheren van je inhoud. 

> _Runaway addon documentatie_
> [Offiële Runaway addon documentatie](https://runway.duncanmcclean.com/)


## Evaluatie van Statamic

### Documenatie en ondersteuning

Hoewel Statamic vrijwel al zijn functionaliteiten beschrijft in de documentatie, is deze vaak oppervlakkig of simpelweg incompleet. Tijdens de ontwikkeling van het prototype is veel informatie over de functionaliteiten uit GitHub-issues gehaald of is de door Statamic opgezette Discord-server geraapleegd. 

**aanbevelingen**:
- Ook voor Statamic is het nuttig om bij te dragen aan de community. Neem samen met het team deel aan de Discord-community, waar de ontwikkelaars van AllesOnline in contact komen met externe ontwikkelaars. Dit kan bijdragen aan het verbreden van kennis over Statamic, maar ook aan het verbreden van inzichten op het gebied van software- en webdevelopment.

### Gebruik van Vue 2

Statamic maakt gebruik van Vue 2 voor de views in het control panel. Dit is relatief opmerkelijk, aangezien Vue 2 op 31 december 2023 het einde van zijn levenscyclus (EOL) bereikte. Volgens de roadmap van Statamic is het de bedoeling om in de lente van 2025 over te stappen naar Vue 3. Voor ontwikkelaars die custom componenten in Statamic hebben gebouwd, betekent dit dat zij hun componenten ook moeten migreren naar Vue 3.

Daarnaast heeft momenteel slechts één van de ontwikkelaars bij AllesOnline ervaring met het werken met Vue.

**aanbevelingen**: Bied ontwikkelaars de gelegenheid om ervaring en kennis op te doen met Vue.
