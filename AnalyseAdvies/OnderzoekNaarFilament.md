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

--- 
### FormFields voor Objecten

De **FormFields**-modules voor objecten in Filament maken het mogelijk om verschillende gegevensvelden (zoals tekst, datum of relatievelden) te gebruiken voor het invoeren en beheren van gegevens. Elke formfield biedt de mogelijkheid om gegevens invoeropties aan te passen aan de vereisten van een specifiek object.

## Evaluatie van Filament

Hoewel het Filament een solide basis biedt voor het opzetten van contentbeheer, heeft het systeem beperkingen in termen van uitbreidbaarheid en schaalbaarheid. De onderstaande analyse richt zich op de codestructuur, gebruiksvriendelijkheid en mogelijkheden voor aanpassing binnen het Filament CMS.

### Gebrekkige Documentatie en Ondersteuning

Filament is relatief nieuw en de community-ondersteuning is nog groeiend. Hoewel de documentatie uitgebreid is, kunnen ontwikkelaars soms problemen ondervinden bij complexere configuraties.

**Aanbeveling**:
- Verhoog de betrokkenheid van de gemeenschap en voeg meer gedetailleerde voorbeelden toe voor complexe configuraties.

### Beperkingen in Complexe Structuren en Code-organisatie

De structuur van resources binnen Filament maakt het instellen van zeer complexe hiërarchieën of relationele structuren uitdagend. Dit kan de uitbreidbaarheid van het systeem beperken.

**Gevolgen van deze beperking**:
- De kans op bugs neemt toe naarmate meer custom code wordt toegevoegd.
- Mogelijkheden voor hergebruik en uitbreidbaarheid worden beperkt.

**Aanbeveling**:
- Overweeg om de code te modulariseren en aanvullende interfaces of abstracties te ontwikkelen voor complexere structuren.

## SOLID-Principes in de Filament Resources

Bij het analyseren van de Filament resources blijkt dat ze op sommige vlakken niet volledig voldoen aan de SOLID-principes, met name aan het **Single Responsibility Principle** (SRP) en het **Open/Closed Principle** (OCP).

* **Single Responsibility Principle (SRP)**:
    
    - De Filament resources zijn zodanig ontworpen dat ze vaak meerdere verantwoordelijkheden dragen, wat kan leiden tot verhoogde complexiteit en moeilijkheden bij onderhoud en debugging.
    - Wanneer één resource meerdere functies vervult, kan het doorvoeren van wijzigingen in één aspect van de resource onbedoeld effect hebben op andere delen van de code.
    
* **Open/Closed Principle (OCP)**:
    
    - Naarmate resources uitgebreider worden en specifieke functionaliteiten moeten ondersteunen, kunnen er met regelmatig wijzigingen in bestaande code nodig zijn.

Toch kan beargumenteerd worden dat deze overtredingen van de SOLID-principes in bepaalde situaties juist de developer-experience verbeteren. Hoewel er verschillende weergaves (views) in één resource worden gespecificeerd, zorgt dit ervoor dat alle views voor een bepaald objecttype op één plek geconcentreerd zijn, wat de overzichtelijkheid verhoogt.

Een bijkomend voordeel van de Filament-bibliotheek is dat veel van de functionaliteiten al zijn geënncapsuleert in specifieke functies die door de library zelf worden getest. Wanneer maatwerkfunctionaliteiten worden toegevoegd, kunnen deze onafhankelijk worden getest doormiddel van unit-en componenttests.

## Conclusie

Filament biedt een goed uitgangspunt voor het opzetten van een eenvoudig CMS, maar kent enkele beperkingen als het gaat om schaalbaarheid en uitbreidbaarheid. Het gebruik van Eloquent als ORM en de Laravel-resource-architectuur zorgt voor een solide basis, maar de mogelijkheden voor aanpassing zijn beperkter in vergelijking met meer volwassen CMS-oplossingen.

Hoewel Filament aantrekkelijk is vanwege de integratie met Laravel, zullen organisaties die een complexe contentstructuur of uitgebreide functionaliteiten nodig hebben, mogelijk tegen grenzen aanlopen van Filament. Daarom moet een balans worden gevonden tussen het gebruiksgemak van Filament en de potentieel hoge kosten voor het aanpassen van het systeem.