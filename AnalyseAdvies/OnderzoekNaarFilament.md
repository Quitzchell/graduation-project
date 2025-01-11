# **Onderzoek naar Filament**

# Inleiding

Dit document beschrijft de analyse van Filament en hoe het kan worden gebruikt voor het opzetten van een CMS. Filament is een Library met, zoals ze het zelf beschrijven, _"a collection of beautiful full-stack components"_. Deze componenten zijn geschikt voor het opzetten van adminpanelen en kunnen worden gebruikt voor het bouwen van een CMS. Deze analyse is uitgevoerd als onderdeel van een *Available Product Analysis*.

> _Andere onderdelen van het Available Product Analysis:_
> * [Onderzoek naar AllesOnline CMS](../AnalyseAdvies/OnderzoekNaarHetAOCms.md)
> * [Onderzoek naar Statamic](../AnalyseAdvies/OnderzoekNaarStatamicCMS.md)

> _De belangrijkste bevindingen over het Filament zijn samengebracht in een SWOT-analyse._
>  * [SWOT: CMS met Filament](./SwotFilamentCms.md)

# Over Filament

Net zoals het AllesOnline CMS, maakt Filement gebruik van **Laravel** en het **Eloquent ORM**. 

De beschikbare componenten zijn onderverdeeld in verschillende packages. Deze packages omvatten verzamelingen van modules voor tabellen, formulieren, acties, notificaties en een container om alle modules samen te brengen. Volgens de documentatie bevat de basisinstallatie van Filament de volgende packages:
* Panel Builder
* Form Builder 
* Table Builder 
* Notifications
* Actions
* Infolists 
* Widgets

> _Voor uitgebreide informatie over het ontwikkelen met Filament kan de officiële documentatie geraadpleegd worden._ 
> * [Filament Documentatie](https://filamentphp.com/docs)

> _Ook zijn er twee videocursussen, waarvan één met de maker Dan Harrin, beschikbaar via Laracast._ 
> * [Laracast cursussen](https://laracasts.com/topics/filament)

# Beheren van content

Hieronder worden een aantal standaard Filament-modules, maar ook zelf gerealiseerde classes benoemt om CMS-functionaliteiten te realiseren.

## Resources

Voor het beheren van gegevens maakt Filament gebruik van de `Resource`-classes. Deze stellen gebruikers in staat om objecten en content te beheren via een interactieve en gebruiksvriendelijke interface. 

1. **CRUD-operaties**: `Resources` maken gebruik van Laravel's Eloquent ORM voor het uitvoeren van CRUD-operaties.
2. **Views**: Standaard voorziet een `Resource` in een info-, create-, edit- en overview-weergave. Deze views kunnen worden aangepast om te voldoen aan specifieke eisen van de eindgebruikers.
3. **Componenten**: Binnen de `Resources` kunnen componenten gedefinieerd worden om de eerder genoemde views van een gebruikersinterface te voorzien. Denk hierbij onder andere aan invoervelden en/of gegevens zichtbaar zijn voor de eindgebruiker en hoe ze hiermee kunnen interacteren.
4. **Permissions en Middleware**: Filament integreert met Laravel's ingebouwde autorisatiesysteem, waardoor het mogelijk is om `policies` en `gates` te gebruiken voor autorisatie. Daarnaast kan de package `spatie/laravel-permission` worden geïntegreerd voor een uitgebreidere rolgebaseerde autorisatie. 

## Menu managent

Filament biedt standaard geen module die het mogelijk maakt om objecten hiërarchisch in te delen via een overview. Hierdoor ontbreekt de functionaliteit om op een intuïtieve manier een menustructuur voor een website te definiëren, zoals mogelijk is in het **AllesOnline CMS** met de `ContentManagerModule`. Desondanks zijn er plugins beschikbaar die dit wel mogelijk maken. Het is ook mogelijk om te werken met een `MenuManager`, waarin `Page`-objecten aan een `Menu`-object gekoppeld kunnen worden en waar recursief `child-pages` aan gekoppeld kunnen worden. Dit zou eventueel uitgebreid kunnen worden naar meerdere objecten door bijvoorbeeld een interface te introduceren die voorziet in de functionaliteiten die een `menuable`-object nodig heeft.

## Page (Model)

Net zoals in het AllesOnline CMS, kan in Filament een `Page`-model worden gerealiseerd. Ook hier is dit model verantwoordelijk voor het persisteren van de pagina's binnen een website. Binnen het model worden belangrijke gegevens, zoals de naam van de pagina, de URL en het toegepaste `Template` opgeslagen. Net zoals in het AllesOnline CMS wordt aan de hand van een `Template` bepaald welke content aan een pagina kan worden toegevoegd. Deze content wordt op zijn beurt gepersisteerd via een polymorfe relatie tussen objecten die content aanbieden en het `Content`-model.

## Templates, Blocks en FormComponents

Ook in Filament dient een `Template`, net zoals in het AllesOnline CMS, als het schema dat voorschrijft welke gegevens via het CMS aan een pagina meegegeven kan worden. Echter, kan een `Template` binnen Filament aan de hand van PHP-classes worden gedefineerd. Dit maakt het mogelijk om gebruik te maken van interfaces om bepaalde functionaliteiten van een `Template`-class te abstracteren. Denk hierbij aan het verplichten van het beschikbaar stellen van het `Template`-schema. 

De schema's voor `Templates` worden opgemaakt met de door Filament beschikbaar gestelde FormComponents. Denk hierbij aan invoervelden voor tekst, selecties, relaties of datums. Binnen een `Template` is het ook weer mogelijk om te verwijzen naar andere schema's waarin een samenvoeging van FormComponents beschikbaar wordt gesteld. Deze schema's noemen we binnen dit systeem ook weer `Blocks`, en ook deze kunnen geordend worden.

# Beheren van objecten

## Resources

Het beheer van objecten gaat in Filament ook aan de hand van `Resources`. Sterker nog, ook het eerder genoemde `Page`-model is een object dat door middel van een `Resource` beheerd wordt. Deze genieten dus dezelfde functionaliteiten die eerder in het stuk over het beheren van content benoemd zijn.

## Eloquent modellen

Filament maakt gebruik van `Eloquent`-modellen voor de representatie van objecten. Dit maakt het, net zoals in het AllesOnline CMS, gemakkelijk om op een eenvoudige, objectgeoriënteerde manier met objecten te werken.

## Schema's

In tegenstelling tot het AllesOnline CMS, worden de CMS-schema's voor objecten in Filament gedefinieerd in de `Resource` die bij de class van het object hoort. Dit betekent dat deze schema's, net zoals een `Templates`, in PHP gedefinieerd worden. Dit biedt verschillende voordelen. Zo kunnen developers onder andere inline de queries voor relatievelden uitbreiden, validaties toevoegen of velden reactief maken. Daarnaast profiteren ze ook van autocompletion.

Zoals al eerder benoemt wordt voor het definiëren van de velden in een schema gebruikt gemaakt van de door Filament beschikbaar gestelde `FormComponents`.

# Evaluatie van Filament

## Uitbreidbaarheid

Filament biedt naast de standaardcomponenten ook een marketplace waar aanvullende componenten beschikbaar zijn. Deze zogenaamde `Plugins`, die zowel gratis als tegen betaling worden beschikbaar zijn, worden ontwikkeld door het Filament-team en externe partijen.

Het is ook voor AllesOnline mogelijk om eigen, op maat gemaakte componenten via de marketplace aan te bieden. Voor de ontwikkeling van deze plugins wordt aanbevolen de TALL-stack (Tailwind CSS, Alpine.js, Laravel, Livewire) te gebruiken, aangezien deze technologieën ook door Filament zelf worden toegepast.

> * [De Filament marketplace](https://filamentphp.com/plugins)_

> _Documentatie voor het realiseren van een Filament Plugin_
> * [Plugin documentatie](https://filamentphp.com/docs/3.x/support/plugins/getting-started)

> _Er is ook een Laracastcursus beschikbaar waarin het realiseren van een plugin uitgelegd wordt_
> * [Laracast video](https://laracasts.com/series/build-advanced-components-for-filament/episodes/12)

## Flexibiliteit

Een van de belangrijkste voordelen van Filament is de flexibiliteit die het ontwikkelaars biedt, omdat vrijwel alles binnen Filament met PHP is gedefinieerd. Dit stelt ontwikkelaars in staat om eenvoudig hun eigen code te integreren en de standaardfunctionaliteit uit te breiden of aan te passen aan specifieke use cases.

## Documentatie en ondersteuning

Omdat Filament nog relatief nieuw is, kan het zijn dat het vinden van een oplossing voor een edgecase niet direct beschikbaar is vanuit de community of documentatie. Desondanks beschikt Filament over een actieve en groeiende open source community (FilamentPHP, 2025).

**Aanbeveling**: Draag bij aan het versterken van Filament. Door actief te zijn in de community wordt niet alleen de open-source software die je professioneel toepast sterker, maar komen de developers van AllesOnline ook in contact met externe developers. Dit kan bijdragen aan het verbreden van kennis over Filament, maar ook het verbreden van algemene inzichten over software- en webdevelopment.

## Livewire & Alpine.js

Van de tech-stack waar Filament gebruikt van maakt, de TALL-stack (Tailwind CSS, Alpine.js, Laravel, Livewire), zijn de AllesOnline developers relatief onbekend met LiveWire en Alpine.js. 

**aanbevelingen**: Bied developers de gelegenheid om ervaring en kennis op te doen met Livewire en Alpine.js.

## SOLID-Principes in de Filament Resources

Hoewel Filament voldoet aan de SOLID-principes, zien we in de `Resources` toch veel verantwoordelijkheden bij elkaar komen. Je zou kunnen stellen dat in dit geval het **Single Responsibility Principle (SRP)** niet wordt nageleefd. Deze classes bevatten namelijk meerdere verantwoordelijkheden, zoals het definiëren van verschillende views binnen het CMS.

Het negeren van dit principe kan worden verdedigd door te stellen dat het combineren van deze verschillende schema's op één plek voordelen biedt. Het zorgt voor een centrale organisatie van alle informatie voor een bepaald object. Dit kan de developers helpen door alle relevante informatie in één bestand te verzamelen, wat een repository minder groot en toegankelijker maakt. Daarnaast is het altijd nog mogelijk om de schema's te extraheren en in de `Resource`-classes te importeren. Deze keuze lijkt dus vooral pragmatisch.

# Conclusie

Filament biedt een solide basis voor het ontwikkelen van een CMS, doordat de standaardcomponenten goed aansluiten bij de vereisten van een typische AllesOnline-website. De flexibiliteit van Filament, in combinatie met PHP, maakt het mogelijk het systeem aan te passen aan specifieke use cases. Bovendien biedt de integratie met Laravel en Tailwind een vertrouwd ecosysteem voor het AllesOnline-team.

Het gebruik van Filament is gratis, maar het team heeft weinig ervaring met twee belangrijke aanvullende technologieën, Livewire en Alpine.js, die nodig kunnen zijn voor het uitbreiden van het systeem met maatwerkcomponenten. Het wordt aanbevolen dat het team zich hierin verder verdiept.

Hoewel Filament de nodige flexibiliteit biedt, biedt het geen kant-en-klare CMS-oplossing, wat betekent dat er extra ontwikkeltijd nodig is om CMS-functionaliteiten te implementeren. Bovendien ontbreekt een drag-and-drop-interface voor het beheren van de menu-structuur, wat mogelijk als minder intuïtief wordt ervaren. Dit kan opgelost worden door een aparte menu-manager of door een complexere drag-and-dropcomponent te ontwikkelen, vergelijkbaar met de functionaliteiten van het AllesOnline CMS.

Al met al biedt Filament een veelbelovende optie voor het ontwikkelen van een CMS, maar het vereist het nog wel de nodige investering in tijd en training om optimaal benut te worden.