# **Adviesdocument**

# Inleiding

Dit document bevat een uitgebreid advies, gebaseerd op onderzoek, voor AllesOnline over het potentieel om één of meerdere nieuwe CMS-systemen verder te ontwikkelen of te implementeren voor de webapplicaties die zij ontwikkelen.

## Context

AllesOnline - een full-service bureau voor online en offline communicatie - haar huidige intern ontwikkelde Content Management Systeem (CMS), dat momenteel in gebruik is en sinds 2015 operationeel is, vormt de basis van de webapplicaties die het bedrijf ontwikkelt. Door een gebrek aan aandacht en onderhoud is het systeem echter verouderd. Dit maakt het moeilijk uit te breiden, waardoor het werken met het systeem steeds minder efficiënt wordt.

# AllesOnline CMS

## Voor eindgebruikers

Het AllesOnline CMS is een gebruiksvriendelijk systeem dat goed aansluit bij de behoeften van de eindgebruikers. Het biedt hen de mogelijkheid om de content en objecten op een website op flexibele wijze te beheren. Een van de sterke punten van het CMS is de drag-and-drop-functionaliteit, waarmee pagina's snel en eenvoudig binnen één weergave kunnen worden gerangschikt.

## Voor developers

Hoewel het systeem voor de developers vertrouwd is, zijn er toch enkele struikelblokken.

* De verouderde en soms zelfs onvolledige documentatie leidt vaak tot onduidelijkheid over welke functionaliteiten beschikbaar zijn in het CMS.

* De inconsistente en complexe codestructuur maakt het lastig om de functionaliteiten zonder documentatie te achterhalen.

* De inconsistente en complexe codestructuur maakt het ook moeilijk om bugs in de code op te sporen.

* Er zijn geen geautomatiseerde tests die valideren of bepaalde functionaliteiten na debuggen, refactoring of doorontwikkeling nog naar behoren werken.

* Het niet naleven van SOLID princpes maakt het lastig om onderdelen van de code te isoleren voor geautomatiseerde tests.

* Er bestaan inconsistenties tussen bepaalde inputvelden met dezelfde functionaliteiten, die soms een verschillende syntax vereisen om te functioneren.

* Het is niet mogelijk om bepaalde relatiestructuren binnen het CMS op te zetten, zoals polymorfische relaties.

* Er is geen mogelijkheid om CMS-content automatisch te verwijderen wanneer gekoppelde gegevens verwijderd worden.

## Hoe te verbeteren 

In grote lijnen zal het doorontwikkelen zich in eerste instantie moeten richten op het modulair en testbaar maken van de `FormModules`. Deze modules bevinden zich in principe op het hoogste niveau in het systeem en dragen minder verantwoordelijkheid voor de werking van de rest van het systeem. Door deze modules modulairder en meer single-responsible te maken, is het mogelijk om geautomatiseerde tests te realiseren die kunnen garanderen dat bestaande functionaliteiten niet breken wanneer er wijzigingen of doorontwikkeling plaatsvinden. Vanuit dit fundament kan verder worden doorgewerkt naar de lagere niveaus, waar onder andere het persisteren van objecten en content wordt uitgevoerd.

Op deze manier kunnen developers de scope van de doorontwikkeling stap voor stap uitbreiden, terwijl ze voortdurend bouwen aan een stevig fundament.

# Mogelijke externe opties
## Filament
Filament is geen CMS, maar een library met full-stack componenten die gebruikt kunnen worden om adminpanels te realiseren. Dit betekend dat functionaliteiten om content te beheren zelf ontwikkeld moeten worden. Desondanks is het een veelbelovende optie om een flexibel en uitbreidbaar CMS te bouwen.

### Voordelen en nadelen van Filament
* **Integratie met Laravel**: Filament is volledig geïntegreerd met Laravel en maakt gebruik van de Eloquent ORM. Dit biedt de mogelijkheid om eenvoudig gegevens te beheren via objectgeoriënteerde technieken. Omdat AllesOnline al bekend is met Laravel, zou de leercurve voor het team relatief laag moeten zijn.

**Polymorfische relaties**: Filament ondersteund de mogelijkheid om polymorpische relaties in de componenten te gebruiken.

* **Gebruiksvriendelijke Componenten**: Filament biedt een scala aan componenten voor adminpanels, zoals formulieren, tabellen, notificaties en meer. Deze componenten kunnen eenvoudig worden aangepast en uitgebreid, waardoor het een flexibele oplossing is die kan worden afgestemd op de specifieke behoeften van de klant.

* **Modulariteit en Uitbreidbaarheid**: Een groot voordeel van Filament is dat je externe plugins en componenten kunt importeren via de Filament Marketplace, waar zowel gratis als betaalde opties beschikbaar zijn. AllesOnline heeft ook de mogelijkheid om eigen componenten te ontwikkelen en aan de marketplace toe te voegen.

* **Beheer van Content via Resources**: De Resource-classes zijn de modules die het beheren van content en objecten mogelijk maken. Omdat deze volledig in PHP worden gedefinieerd, is het gemakkelijk om extra logica toe te voegen en profiteren developers van autocompletion.

* **Flexibele Template Structuur**: Filament biedt de mogelijkheid om de lifecycle van gegevens gemakkelijk te manipuleren. Dit maakt het mogelijk om een datastructuur te realiseren die vergelijkbaar is met die van AllesOnline. 

* **Permissions en Middleware:** Filament maakt gebruik van de ingebouwde Laravel-permissions en biedt uitgebreide mogelijkheden voor toegangscontrole. Dit maakt het mogelijk om specifieke rollen en rechten toe te wijzen aan gebruikers.

* **Geen Kant-en-Klare CMS Functionaliteit:** Filament is geen kant-en-klare CMS-oplossing. Los daarvan is het wel flexibel genoeg om zonder al te grote moeite CMS-functionaliteiten te implementeren.

* **Geen drag-and-drop-interface:** Door het ontbreken van kant-en-klare CMS-functionaliteiten ontbreekt ook een drag-and-drop-interface. In plaats daarvan kan een aparte menu-manager worden geïmplementeerd, waarin menu-objecten kunnen worden gekoppeld aan objecten die als pagina's gerenderd kunnen worden. Dit kan door klanten als minder intuïtief worden ervaren.

* **Beperkte Community en Ondersteuning**: Filament is een relatief nieuw project, en hoewel de documentatie goed is, is de community nog in ontwikkeling. 

* **Afhankelijkheid van Livewire en Alpine.js**: Filament maakt gebruik van Livewire en Alpine.js, twee technologieën die AllesOnline mogelijk nog niet volledig onder de knie heeft. Het kan nodig zijn om extra training of tijd in te plannen voor het team om hiermee vertrouwd te raken.


## Statamic
Statamic is een op Laravel gebaseerd CMS met veel kant-en-klare functionaliteiten voor eindgebruikers. Het is een goed alternatief voor webapplicaties die zich voornamelijk op content richten en weinig complexe logica bevatten. Het is mogelijk om de datalaag van Statamic op verschillende manieren in te richten. Zo heb je een configuratie voor flat-files, waarbij gegevens in de repository worden opgeslagen, de Eloquent-driver, waarbij deze documenten als JSON in een database worden opgeslagen, en de Runway-configuratie, waarbij je aparte Eloquent-modellen en databastabellen kunt gebruiken.

### Voordelen en nadelen van Statamic
**Kant-en-klare API**: Statamic biedt een gebruiksklare REST API die developers in staat stelt om snel en eenvoudig gegevens op te halen. 

**Polymorfische relaties**: Statamic ondersteund in de flat-file en eloquent-driver configuratie de mogelijkheid tot het implementeren van polymorfische relaties. Helaas is dit nog niet mogelijk in de Runway-configuratie. 

**Userinterface voor het opzetten van een CMS**: Het opzetten van het CMS kan in Statamic volledig via een gebruikersinterface. Dit maakt het ook voor niet-technische gebruikers mogelijk om een CMS op te zetten of in te richten, terwijl developers nog steeds volledige controle behouden over de onderliggende systemen.

**Volledig File-based CMS**: Statamic slaat de inhoud en configuraties op als bestanden, zoals YAML en markdown. Voor websites waar niet veel gegevens nodig zijn, kan dit het systeem sneller maken, omdat er geen databasequery's nodig zijn om de gegevens op te halen.

--- ---
