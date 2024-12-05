# Onderzoek naar Filament

## Inleiding

Dit document beschrijft de analyse voor over Filament en hoe het eventueel gebruikt kan worden voor het opzetten van een CMS. Filament is Library met, zoals ze het zelf zeggen, _"a collection of beautiful full-stack components"_. Deze componenten zijn geschikt voor het opzetten van CRUD-applicaties en naar mijn idee geschikt voor het bouwen van een CMS. 

> _Andere onderdelen van het Available Product Analysis:_
> * _[Onderzoek naar AllesOnline CMS](../AnalyseAdvies/OnderzoekNaarHetAOCms.md)_

> *De belangrijkste bevindingen over het Filament zijn samengebracht in een SWOT-analyse. [hier kunt lezen](SwotFilamentCms).*

## Over Filament

Ook Filament maakt net zoals het AllesOnline CMS gebruikt van **Laravel** en het Eloquent ORM dat dit framework biedt. 

De beschikbare componenten zijn onderverdeeld in verschillende packages. Deze packages omvatten verzamelingen van modules voor tabellen, formulieren, acties, notificaties en een container om alle modules samen te brengen. Volgens de documentatie bevat de basisinstallatie van Filament de volgende pakketten:

>* Panel Builder
>* Form Builder 
>* Table Builder 
>* Notifications
>* Actions
>* Infolists 
>* Widgets

Naast de standaardcomponenten biedt Filament ook de mogelijkheid om zelf componenten te ontwikkelen die binnen Filament gebruikt kunnen worden. Daarnaast is het zelfs mogelijk om deze oplossingen via een door Filament opgezette marketplace beschikbaar te stellen aan anderen, eventueel tegen betaling. Voor het ontwikkelen van de componenten is het wel aan te raden om gebruik te maken van de door Filament gehanteerde TALL-stack (Tailwind CSS, Alpine.js, Laravel, Livewire).

> _De marketplace van Filament [vind je hier](https://filamentphp.com/plugins)_

## Beheren van content

### Resources

Voor het beheren van content met Filament kunnen we gebruik maken van de `Resource`-modules waarmee gebruikers zowel objecten kunnen beheren in een interactieve en gebruiksvriendelijke gebruikersinterface. De belangrijkste facetten van de `Resources`  zijn: 

1. **CRUD-operaties**: Resources maken gebruik van de standaard Laravel-controllers en Eloquent om objecten aan te maken, lezen, bewerken en verwijderen.
2. **Views**: De Resource biedt standaard de weergaven `list`, `create`, `edit` en `view`. Deze kunnen eenvoudig via de Resource worden aangepast om te voldoen aan de specifieke eisen van de eindgebruikers.
3. **Componenten**: Om te bepalen wat op de eerder genoemde views voor eindgebruikers zichtbaar is en waarmee zij kunnen interacteren, biedt Filament een breed scala aan componenten. Deze componenten definiëren bijvoorbeeld invoervelden op de `create`- en `edit`-views, of de kolommen van een tabel in een `list`-view.
4. **Permissions en Middleware**: In Laravel kunnen rechten worden toegekend aan specifieke rollen voor het beheren van de Resources. Dit kan zowel met ingebouwde methoden zoals policies en gates als met externe pakketten zoals `spatie/laravel-permission` voor uitgebreide toegangscontrole.

### Beheren van content 

Filament biedt in de `list`-weergave de mogelijkheid om tabellen te definiëren. Deze kunnen zowel statisch als interactief zijn, bijvoorbeeld door de records in de tabel herschikbaar te maken via _drag-and-drop_, of door switches (voor booleans) of select-velden in de kolommen op te nemen om bepaalde gegevens van de objecten die ze vertegenwoordigen te bewerken.

Wat Filament niet biedt, is de ondersteuning voor het in een tabel weergeven van hiërarchische informatie doormiddel van nested records. Deze feature die wel het AllesOnline CMS te vinden is maakt het daar mogelijk voor gebruikers om in één weergave te zien welke pagina's beschikbaar zijn en in welk menu ze meegenomen worden (main menu, footer menu of hidden).  Dat deze feature niet aanwezig is, maakt het echter niet onmogelijk om eindgebruikers een soortgelijke functionaliteit aan te bieden. Mocht het echter een wens zijn om dit wel op dezelfde manier beschikbaar te stellen, dat is er relatief veel maatwerk nodig om dit in Filament te realiseren.

### Page (Model)

Net zoals in het AllesOnline CMS kunnen we in Filament een `Page`-model realiseren. Deze zal in dit systeem ook de basis zijn voor het opzetten van de structuur van een website. Het model vertegenwoordigd individuele pagina's en slaat de basisinformatie op, zoals de naam van de pagina en het `Template` dat toegepast wordt om content mee te persisteren. Ook in dit systeem is het dan mogelijk om de content via een polymorfe relatie te koppelen aan objecten die content aanbieden.

### Templates en FormFields

Ook in het systeem met Filament kunnen - net zoals in het AllesOnline CMS - `Templates` gedefinieerd worden. Het grote voordeel van Filament is echter dat een schema voor een `Template` gedefinieerd kan worden in PHP-bestanden. Dit maakt het niet alleen mogelijk om autocompletion te benutten, maar ook om gebruik te maken van interfaces om de abstractie van bepaalde functionaliteiten van een `Template`-class te definiëren. Denk hierbij aan het standaardiseren van het aanbieden van een vormschema of het ophalen van gepersisteerde gegevens.

De vormschema's voor `Templates` worden opgemaakt met de door Filament beschikbaar gestelde componenten voor formulieren. Denk hierbij aan invoervelden voor tekst, selecties, relaties of datums.

Binnen de templates is ook het, net zoals in het AllesOnline CMS, mogelijk om verder te verwijzen naar andere schema's om een verzameling van componenten voor formuleren aan te bieden als een `block`.

## Beheren van objecten

### Resources

Het beheren van objecten kan in Filament ook gerealiseerd worden aan de hand van de `Resources`. Sterker nog, ook het `Page`-model is een object dat door middel van een `Resource` beheerd kan worden. Deze genieten dus dezelfde functionaliteiten die eerder in het stuk over Resources voor het beheren van content benoemd zijn.

### Eloquent Models

Filament maakt gebruik van `Eloquent`-modellen voor de representatie van objecten. Dit maakt het, net zoals in het AllesOnline CMS, gemakkelijk om  op een eenvoudige, objectgeoriënteerde manier met gegevens van objecten te werken. Denk hierbij aan het ophalen, bijwerken en verwijderen van gegevens, en het definiëren van relaties tussen verschillende objecten.

### CMS Schema's

In tegenstelling tot het AllesOnline CMS, worden de CMS-schema's voor objecten in Filament gedefinieerd in de `Resource` die bij de class van het object hoort. Dit betekent dat deze schema's, net zoals de `templates`, in PHP gedefinieerd kunnen worden. Dit zorgt er onder andere voor dat developers voorzien worden van autocompletions bij het definiëren van de schema's, maar ook dat bepaalde filters op bijvoorbeeld selectievelden gedefinieerd kunnen worden met behulp van Laravel zijn `Eloquent ORM` of `QueryBuilder`. Dit maakt het opstellen van een schema voor objecten een stuk flexibeler en meer uitbreidbaar.

Zoals al eerder benoemt worden voor het definiëren van de velden in een schema gebruikt gemaakt van de door Filament beschikbaar gestelde componenten voor formulieren.

## Evaluatie van Filament

Hoewel het Filament een solide basis biedt voor het realiseren van een CMS, heeft het systeem nog een aantal beperkingen.

### Documentatie en ondersteuning

Filament is relatief nieuw en de community-ondersteuning staat nog in kinderschoenen. Hoewel de documentatie uitgebreid is en qua basisinformatie compleet is, bestaat de kans dat er minder informatie beschikbaar is voor complexere oplossingen.

**Aanbeveling**:
- Draag bij aan het versterken van Filament. Door actief te zijn in de community wordt niet alleen de open-source software die je professioneel gebruikt verbeterd, maar komen de ontwikkelaars van AllesOnline ook in contact met externe ontwikkelaars. Dit kan bijdragen aan het verbreden van perspectieven op het gebied van software- en webdevelopment.

### Relatief onbekende technieken

Van de tech-stack waar Filament gebruik van maakt, zijn de meeste AllesOnline-ontwikkelaars alleen bekend met Laravel en Tailwind. Dit betekent dat, als er op maat gemaakte componenten ontwikkeld moeten worden, de ontwikkelaars zich moeten verdiepen in Livewire en Alpine.js.

## SOLID-Principes in de Filament Resources

Bij het onderzoeken van Filament kwam naar voren dat de ontwikkelaars zich niet overal strikt aan de SOLID-principes houden. 

In de `Resources` zien we dat het **Single Responsibility Principle (SRP)** niet volledig wordt nageleefd. Deze classes bevatten namelijk meerdere verantwoordelijkheden, zoals het definiëren van verschillende views en schema's. Dit betekent dat de class niet alleen verantwoordelijk is voor het beheren van gegevens, maar ook voor de structuur van de bijbehorende weergaven, wat leidt tot een hogere complexiteit in de class en verminderde modulariteit.

Het negeren van dit principe kan worden verdedigd door te stellen dat het combineren van verschillende schema's op één plek voordelen biedt. Het zorgt voor een centrale organisatie van alle informatie voor een bepaald objecttype, wat de overzichtelijkheid ten goede kan komen, vooral in kleinere of middelgrote applicaties. Dit kan de ontwikkelaar helpen door alle relevante informatie in één bestand te verzamelen, wat de leesbaarheid en toegankelijkheid verbetert, ondanks dat het niet strikt in lijn is met SRP.

Deze keuze lijkt dus vooral pragmatisch, waarbij wordt gekozen voor praktische voordelen waarbij eenvoud en snelle toegang vaak vooropstaan, zelfs als dit ten koste gaat van de strikte naleving van principes zoals SRP.

## Conclusie

Filament biedt een goed uitgangspunt voor het opzetten van een CMS. Niet alleen omdat het erop lijkt dat veel van de standaardcomponenten binnen de library al voldoen aan een groot deel van de vereisten voor een doorsnee AllesOnline-website, maar ook omdat het mogelijk is om zelf componenten en zelfs modules te ontwikkelen. Daarnaast is het voor de developers bij AllesOnline gunstig dat het gebaseerd is op Laravel en Tailwind, twee technieken waarmee het gehele team al bekend is. 

Wel is er de valkuil dat het AllesOnline team niet thuis is bij twee van de technieken die nuttig zijn bij het uitbreiden van het system met op maat gemaakte componenten. Mocht er voor Filament gekozen worden, dan is het verstandig voor het team om zich te verdiepen in de werking van Livewire en Alpine.js.