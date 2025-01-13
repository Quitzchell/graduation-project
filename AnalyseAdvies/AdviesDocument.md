# **Adviesdocument**

# Inleiding

Dit document biedt een uitgebreid advies gebaseerd op het uitgevoerde onderzoek naar het AllesOnline CMS, Filament en Statamic. Het onderzoek richt zich op het evalueren van de huidige situatie van het AllesOnline CMS en op het identificeren van de beste oplossingen om toekomstige uitdagingen te overwinnen. 

In dit onderzoek worden de sterke en zwakke punten van het huidige AllesOnline CMS geanalyseerd. Vervolgens worden de mogelijkheden en beperkingen van Filament en Statamic vergeleken. Op basis van deze analyse biedt het document concrete aanbevelingen en een roadmap, waarmee AllesOnline de transitie naar een toekomstbestendig systeem kan realiseren.

## Context

AllesOnline, een full-service bureau voor online en offline communicatie, maakt gebruik van een intern ontwikkeld Content Management Systeem (CMS) dat sinds 2015 de basis vormt voor de webapplicaties van het bedrijf. Door een gebrek aan regelmatig onderhoud en het doorvoeren van updates is het systeem echter verouderd geraakt. Dit beperkt de uitbreidbaarheid en vermindert de efficiëntie van het CMS.

# AllesOnline CMS

## Voor eindgebruikers

Het AllesOnline CMS is een gebruiksvriendelijk systeem dat goed aansluit bij de behoeften van de eindgebruikers. Het biedt hen de mogelijkheid om de content en objecten op een website op flexibele wijze te beheren. Eén van de sterkste punten van het CMS is de drag-and-drop-functionaliteit, waarmee pagina's snel en eenvoudig binnen één weergave kunnen worden gerangschikt.

## Voor developers

Hoewel het systeem voor de developers vertrouwd is, zijn er in het huidige AllesOnline CMS struikelblokken door verouderde en soms onvolledige documentatie. Dit leidt vaak tot onduidelijkheid over de beschikbare functionaliteiten, wat het moeilijk maakt om efficiënt met het systeem te werken. De inconsistente en complexe codestructuur versterkt dit probleem, waardoor het niet alleen lastig is om zonder documentatie de functionaliteiten te begrijpen, maar ook om bugs effectief op te sporen en op te lossen.

Daarnaast ontbreken geautomatiseerde tests die garanderen dat functionaliteiten blijven werken na het debuggen, refactoren of doorontwikkelen. Het opzetten van deze geautomatiseerde tests wordt bemoeilijkt door het gebrek aan naleving van de SOLID-principes. Veel functies binnen het CMS hebben meerdere verantwoordelijkheden, waardoor bepaalde use cases niet geïsoleerd getest kunnen worden.

Bovendien zijn er inconsistenties tussen bepaalde inputvelden, die ondanks gelijke functionaliteit verschillende syntaxis vereisen, wat zonder duidelijke documentatie tot verwarring leidt. De inputvelden die zijn ontworpen om relaties tussen objecten te definiëren, bieden geen ondersteuning voor complexe relaties zoals polymorfe relaties. Dit beperkt het gebruiksgemak voor eindgebruikers, omdat zij meerdere inputvelden moeten hanteren om dergelijke relaties vast te leggen, wat ten koste gaat van het overzicht.

## Hoe te verbeteren 

De doorontwikkeling zou zich in eerste instantie moeten focussen op het modulair en testbaar maken van de `FormModules`. Deze modules functioneren op het hoogste niveau binnen het systeem en zijn relatief onafhankelijk van andere onderdelen. Door ze modulairder en meer single-responsible te ontwerpen, wordt het mogelijk om geautomatiseerde tests te implementeren. Deze tests waarborgen dat bestaande functionaliteiten intact blijven bij wijzigingen of verdere ontwikkeling. Dit creëert een solide basis waarmee stapsgewijs naar lagere niveaus kan worden gewerkt, zoals de code die verantwoordelijk is voor het persisteren van objecten en content.

Op deze manier kunnen developers de scope van de doorontwikkeling stapsgewijs uitbreiden, terwijl ze tegelijkertijd werken aan een stevig fundament.

# Filament

**Filament** is een gebruiksvriendelijke component library voor het bouwen van adminpanels, dat een breed scala aan componenten biedt, zoals formulieren, tabellen en notificaties. Deze componenten kunnen eenvoudig worden aangepast aan de specifieke behoeften van de klant, wat Filament tot een veelzijdige keuze maakt.

## Sterke integratie met Laravel

Filament is volledig geïntegreerd met Laravel en maakt gebruik van de Eloquent ORM, waardoor gegevensbeheer met objectgeoriënteerde technieken wordt vereenvoudigd. Omdat AllesOnline al ervaring heeft met Laravel, zal de leercurve voor het team minimaal zijn.

## Gegevensbeheer

In Filament zijn `Resource`-classes verantwoordelijk voor het definiëren hoe een `Eloquent`-model wordt beheerd in de gebruikersinterface. Deze klassen bepalen hoe gegevens worden weergegeven, ingevoerd en bewerkt. Daarnaast bieden ze tools voor het beheren van de gegevenslevenscyclus, waardoor vergelijkbare datastructuren, zoals die in het AllesOnline CMS, kunnen worden gerealiseerd. Aangezien deze classes volledig in PHP zijn gedefinieerd, kunnen developers eenvoudig extra logica toevoegen en profiteren van autocompletion.

Daarnaast ondersteunen de inputvelden waarin relationships worden gedefinieerd in Filament wel polymorfe relaties, wat het mogelijk maakt om een overzichtelijkere weergave voor eindgebruikers te realiseren.

## Marketplace

De Filament Marketplace benadrukt de uitbreidbaarheid van Filament, doordat het zowel gratis als betaalde externe plugins en componenten aanbiedt. Dit maakt het eenvoudig om bestaande oplossingen snel te integreren. Daarnaast stelt het AllesOnline in staat om eigen componenten te ontwikkelen en toe te voegen aan de marketplace, wat ruimte biedt voor maatwerk en continue groei.

## Geavanceerde toegangscontrole

Dankzij de ingebouwde Laravel-permissions biedt Filament geavanceerde mogelijkheden voor toegangscontrole, waarmee rollen kunnen worden toegewezen aan gebruikers, en aan deze rollen kunnen specifieke rechten of permissies worden gekoppeld.

## Kosten

Het gebruik van Filament is gratis. Het Filament-team biedt echter de mogelijkheid om bepaalde features of bugs, die vermeld staan in de GitHub-issues, financieel te ondersteunen door middel van een donatie.

## Beperkingen en overwegingen

Het is belangrijk op te merken dat Filament geen kant-en-klare CMS-oplossing biedt. Hoewel het voldoende flexibiliteit biedt om CMS-functionaliteiten te implementeren, vereist dit extra ontwikkeltijd in vergelijking met systemen die deze functionaliteiten standaard aanbieden. Bovendien ontbreekt een drag-and-drop-interface voor het beheren van de menu-structuur. Dit kan voor sommige klanten mogelijk als minder intuïtief kan worden ervaren. Een eenvoudige oplossing is het implementeren van een aparte menu-manager. Een complexere benadering is het ontwikkelen van een custom drag-and-dropcomponent dat vergelijkbaar is met de functionaliteiten van het AllesOnline CMS.

Filament is afhankelijk van Livewire en Alpine.js, twee technologieën die mogelijk nog niet volledig beheerst worden door het AllesOnline-team, wat extra training en educatie kan vereisen.

# Statamic

**Statamic** is een ander potentieel CMS voor AllesOnline, dat vooral geschikt is voor klanten die zich richten op marketingwebsites, zonder de noodzaak voor complexe logica of datastructuren. Het biedt veel kant-en-klare functionaliteiten die de implementatie van een CMS aanzienlijk kunnen versnellen. Dit maakt Statamic een aantrekkelijke keuze voor klanten die snel willen schakelen.

## Laravel-ish

In tegenstelling tot Filament is Statamic niet volledig geïntegreerd met Laravel. Dit zie je vooral in de flat-file en Eloquent-driver configuraties,  waarbij gegevens als documenten binnen de repository of als JSON in een database worden opgeslagen. Pas wanneer gekozen wordt voor de Runway-configuratie, wordt het mogelijk om voor elke entiteit specifieke Eloquent-modellen en databasetabellen te creëren. Het opzetten van deze configuratie is echter relatief complexer.

## Flat-file versus Database

Met Statamic is het mogelijk om eenvoudig te kiezen tussen een flat-file of een database-gebaseerde architectuur. De flat-filearchitectuur is eenvoudig, snel en geschikt voor kleine tot middelgrote websites. Echter, bij grotere hoeveelheden data of meerdere gelijktijdige bewerkingen, is één van de databaseconfiguraties een betere keuze. Deze bieden namelijk betere schaalbaarheid en geavanceerder gegevensbeheer door het gebruik van een database.

## Gegevensbeheer

Zoals eerder vermeld, is het gegevensbeheer in Statamic in de flat-file- en eloquent-driverconfiguraties documentgeoriënteerd. Dit maakt het beheren van gegevens flexibeler en stelt gebruikers in staat om inputvelden met polymorfe relaties te gebruiken. Dit voordeel verdwijnt echter bij het gebruik van de Runway-configuratie.

Het wisselen tussen de flat-file- en eloquent-driverconfiguratie is volledig geautomatiseerd in Statamic en kan eenvoudig worden uitgevoerd via een CLI-command.

## Multi-site

Statamic biedt ondersteuning voor multisite-configuraties, wat nuttig kan zijn wanneer meerdere websites of subdomeinen voor klanten binnen één installatie beheerd moeten worden. Dit kan echter de flexibiliteit en schaalbaarheid van de website beperken.

## DIY

De gebruiksvriendelijke interface stelt niet-technische gebruikers in staat om gestandaardiseerde websites op te zetten met voorgedefinieerde componenten. Dit biedt vormgevers bij AllesOnline de mogelijkheid om eenvoudige whitelabel-websites te realiseren, terwijl developers volledige controle behouden over de onderliggende codebase.

## Kosten 

Afhankelijk van het aantal websites dat met Statamic in productie wordt genomen, biedt Statamic verschillende licentieopties. De keuze voor de **Statamic Pro** licentie is noodzakelijk voor commerciële toepassingen, maar de **Master License** biedt een goede mogelijkheid om meerdere websites te beheren tegen een gereduceerde prijs. Bij verdere groei kan het abonnementsmodel van de **Platform Subscription** voordeliger zijn.

## Beperkingen en overwegingen

Statamic maakt voor de interface van het control panel gebruik van Vue 2 dat al is sinds eind 2023 EOL is. Statamic heeft aangegeven in het voorjaar van 2025 over te stappen naar Vue 3, wat betekent dat eventuele custom Vue 2-componenten naar Vue 3 gemigreerd moeten worden.

Omdat Statamic gebruik maakt van YAML-bestanden voor de configuratie van schema’s, kan de leercurve aanvankelijk wat steiler zijn, vooral door de soms versplinterde structuur van de documentatie.

## Migreren van bestaande AllesOnline CMS

Statamic maakt in al zijn configuraties gebruik van een afwijkende datasctructuur, wat aanzienlijke aanpassingen vereist bij een migratie. Dit vergroot de complexiteit van het proces, waardoor het niet raadzaam is om bestaande AllesOnline-projecten naar Statamic te migreren. Wanneer het te migreren project echter niet te groot is, maar de voordelen voor de eindgebruiker bij Statamic doorslaggevend zijn, kan de backend van de website relatief eenvoudig opnieuw worden opgebouwd in Statamic met de Eloquent-driverconfiguratie. Met een goed voorbereide migratie zou in dit geval enkel de bestaande database nog hoeven te worden overgezet en aan de frontend worden gekoppeld.

# Advies en aanbevelingen

Mijn advies aan AllesOnline is om zijn horizon te verbreden en externe oplossingen te overwegen. Hoewel het bedrijf trots kan zijn op zijn eigen CMS, dat lange tijd het fundament van de webapplicaties vormde, heeft het systeem de afgelopen tijd te weinig onderhoud gekregen. De kosten en inspanningen die nodig zijn om het CMS te refactoren en moderniseren, wegen niet op tegen de voordelen van een extern product. Door een extern systeem in te zetten, kan AllesOnline meer tijd en middelen richten op zijn kerntaak: het realiseren van klantoplossingen. Daarom adviseer ik om niet verder te investeren in het huidige CMS, maar externe oplossingen te overwegen die het bedrijf verder kunnen helpen.

Voordat ik verder inga op de beste keuze voor een externe oplossing, wil ik eerst de belangrijkste les benoemen die ik in het tweede semester van mijn HBO-ICT-opleiding van docent Marco Meulenbroeks heb geleerd: 'It depends'. Dit werd ook bevestigd in een gesprek met onze senior developer Wilco Kuijper. Hij benadrukte, net zoals Meulenbroeks, dat we ons niet moeten vastbijten in één oplossing. Ik ben het daar volledig mee eens. Het is namelijk essentieel om te kijken welke oplossing het beste past bij de specifieke wensen van de klant. In dit onderzoek hebben we twee heel verschillende systemen bekeken. Beide systemen hebben hun eigen voor- en nadelen, afhankelijk van het type project.

## Filament

De mogelijkheid om bestaande componenten direct via PHP aan te passen in Filament biedt veel flexibiliteit en maakt het een uitstekende keuze voor klanten die een complex, op maat gemaakt systeem nodig hebben. Dit geldt bijvoorbeeld voor webapplicaties met een uitgebreid assortiment, grote hoeveelheden data of complexe klantenservicesystemen. Voorbeelden van AllesOnline-projecten die hiervan zouden kunnen profiteren, zijn onder andere Provimi Body Condition Score, Ingrado, HVA en Klinger.

## Statamic

Statamic zou daarentegen een geschikte keuze kunnen zijn voor klanten die slechts een marketingwebsite nodig hebben, waarbij de content dynamisch beheerd wordt, maar waar geen complexe relaties en datastructuren worden verwacht. Een CMS voor dit soort websites kan met Statamic relatief sneller gerealiseerd worden dan met Filament. Denk hierbij aan klanten zoals DPD, Vierstroom of zelfs Westfalia.

Daarnaast zou het mogelijk zijn om een boilerplate voor Statamic te ontwikkelen met gestandaardiseerde componenten. Dit zou de vormgevers bij AllesOnline in staat stellen om zelfstandig whitelabel-websites te creëren via Statamic, terwijl de developers directe technische ondersteuning kunnen bieden wanneer dat nodig is.

Een uitdaging van Statamic is dat de licentiekosten per website in eerste instantie hoger zijn. Echter, naarmate meer websites gebruikmaken van Statamic, zullen de kosten per website dalen door de kwantumkorting binnen het abonnementsmodel van de **Platform Subscription**. In dit model betaal je voor de eerste 25 websites een vast bedrag van $175. Voor de volgende 75 websites betaal je $7 per website per maand, en voor de volgende 400 websites $6 per website per maand. Dit betekent dat de prijs per website daalt naarmate je meer websites toevoegt. Als er bijvoorbeeld 25 websites met Statamic actief zijn, zullen de kosten voor een licentie niet meer dan $7 per website bedragen.

## Combineren

Voor klanten die zowel complexiteit als een marketingwebsite verwachten, kan gesteld worden dat het inzetten van beide oplossingen een realistische keuze is. In dit geval zou bijvoorbeeld een adminpaneel voor stakeholders in Filament gerealiseerd kunnen worden, terwijl de marketingwebsite – of mogelijk zelfs whitelabel-websites – in Statamic beheerd kunnen worden. Op deze manier kan een diverse verzameling systemen worden opgeleverd, die elk afgestemd zijn op hun specifieke use cases. Dit zou bijvoorbeeld nuttig kunnen zijn voor projecten zoals OpleidingVinden of ScamAdviser.

## Migratie van bestaande projecten

Voor de migratie van bestaande projecten is het raadzaam om te migreren naar **Filament**. Dit omdat Filament in staat is om relatief dicht bij de datastructuur van het AllesOnline CMS te komen. De benodigde aanpassingen zijn minimaal en kunnen in grote lijnen geautomatiseerd worden met behulp van een op maat gemaakte migratietool.

Omdat Filament volledig compatibel is met **Eloquent ORM**, kunnen modellen en gegevens uit het huidige systeem vrijwel direct worden hergebruikt. Deze compatibiliteit zorgt ervoor dat de overgang naar Filament soepel kan verlopen, zonder dat er ingrijpende wijzigingen nodig zijn in de onderliggende entiteitslaag. 

Er zijn ook risico's voor de migratie naar Filament. Dit geldt vooral voor de diversiteit aan architectuurkeuzes en custom componenten die in de loop der jaren voor projecten van het AllesOnline CMS zijn gemaakt. Omdat dit per project drastisch kan verschillen, is het mogelijk dat er tussen de verschillende projectmigraties edgecases naar voren komen. Dit maakt de ontwikkeling van een volledig geautomatiseerde tool complexer en brengt dus extra tijd met zich mee.

Het migreren naar **Statamic** is, vooral voor grotere projecten, minder geschikt vanwege de afwijkende datastructuren. Bij Statamic is een aanzienlijke herstructurering van de data vereist, wat voor complexe projecten een tijdrovende en kostbare operatie kan zijn.


## Langetermijneffecten

Hoewel de keuze om externe systemen te integreren nog steeds een investering is, kan het AllesOnline in de toekomst veel voordelen opleveren. Door te vertrouwen op doorontwikkelde systemen van externe partijen kan AllesOnline zich blijven aanpassen aan technologische innovaties, zonder last te ervaren van de veroudering van een eigen systeem. Dit bevordert de duurzaamheid van de bedrijfsvoering en voorziet dat de focus kan liggen op het voldoen in klantbehoeften in plaats van het onderhouden van verouderde systemen.

Bovendien positioneert het bedrijf zich op deze manier als een flexibele en toekomstgerichte speler in de markt, waardoor het aantrekkelijk blijft voor zowel klanten als talentvolle, toekomstige medewerkers. Strategisch gezien kan deze beslissing bijdragen aan stabiele groei en biedt het een concurrentievoordeel, omdat de bedrijfsfocus volledig gericht blijft op klantgerichte innovatie en creatieve oplossingen.

# Roadmap

Om een soepele overgang van het AllesOnline CMS naar Filament en Statamic te waarborgen, wordt aangeraden de transitie gefaseerd uit te voeren. Iedere fase is gericht op het opbouwen van de benodigde kennis over de nieuwe systemen, zodat de overstap naar volledige adoptie stapsgewijs en gecontroleerd kan plaatsvinden. Vanaf fase 3 kunnen er parallelle paden worden bewandeld, waarbij zowel gewerkt kan worden aan het migreren van bestaande projecten als het realiseren van de eerste greenfield-projecten met de nieuwe systemen.

## Fase 1: Voorbereiding, kennisopbouw en validatie
**Doel: Developers vertrouwd maken met Filament en Statamic**

<table width="100%">
<thead>
    <tr>
        <th width="32%">Activiteit</th>
        <th width="28%">Duur</th>
        <th width="40%">Beschrijving</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td>Introductie van systemen</td>
        <td>1 uur*</td>
        <td>Presentatie aan het team over Filament en Statamic</td>
    </tr>
    <tr>
        <td>Training: Statamic / Filament</td>
        <td>3 - 7 uur*</td>
        <td>Developers volgen naar eigen inzicht relevante workshops op Laracast</td>
    </tr>
    <tr>
        <td><a href="../Bijlagen/HackatonBouwCMS.md">Hackathon: Bouw van CMS met Filament en Statamic</a></td>
        <td>8 uur (1 dag)*</td>
        <td>Praktische ervaring opdoen door prototype CMS te bouwen. Hands-on ervaring met de systemen</td>
    </tr>
    <tr>
        <td>Training: Livewire / Alpine.js</td>
        <td>3 - 7 uur*</td>
        <td>Developers volgen naar eigen inzicht workshops voor Livewire en Alpine.js om dynamische functionaliteit in Filament beter te begrijpen</td>
    </tr>
    <tr>
        <td><a href="../Bijlagen/HackatonBouwFilamentComponent.md">Hackathon: Bouw een custom Filament component met Livewire en Alpine.js</a></td>
        <td>8 uur (1 dag)*</td>
        <td>Ervaring opdoen met het bouwen van custom Filament-componenten en het integreren van Livewire en Alpine.js</td>
    </tr>
</tbody>
</table>

\* _In deze fase geldt de tijdsindicatie per developer._ 

## Fase 2: Prototype en evaluatie
**Doel: Ontwikkelen van CMS-plugin voor Filament**

<table>
<thead>
    <tr>
        <th width="32%">Activiteit</th>
        <th width="28%">Duur</th>
        <th width="40%">Beschrijving</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td>Prototype CMS-plugin</td>
        <td>ca. 40 uur (5 dagen)</td>
        <td>Ontwikkeling van een generieke Filament-plugin voor CMS-functionaliteit</td>
    </tr>
    <tr>
        <td>Valideer prototype</td>
        <td>8 uur (1 dagen)</td>
        <td>Testen van prototype binnen een bestaand project met een decoupled architecture</td>
    </tr>
    <tr>
        <td>Documenteer plugin</td>
        <td>16 uur (2 dagen)</td>
        <td>Opstellen van documentatie voor het team</td>
    </tr>
</tbody>
</table>

## Fase 3a: Ontwikkeling van greenfield-projecten
**Doel: Realisatie van projecten met nieuwe CMS-systemen.**

<table>
<thead>
    <tr>
        <th width="32%">Activiteit</th>
        <th width="28%">Duur</th>
        <th width="40%">Beschrijving</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td>Greenfield project (Filament)</td>
        <td>ca. 40 uur (5 dagen)*</td>
        <td>Project met Filament CMS realiseren, inclusief data-opslag en contentbeheer</td>
    </tr>
    <tr>
        <td>Greenfield project (Statamic)</td>
        <td>ca. 24 uur (3 dagen)*</td>
        <td>Project met Statamic CMS realiseren, inclusief data-opslag en contentbeheer</td>
    </tr>
    <tr>
        <td>Feedback verzamelen</td>
        <td>8 uur (1 dag)</td>
        <td>Feedback van team en stakeholders over de gebruikerservaring verzamelen</td>
    </tr>
    <tr>
        <td>Systemen optimaliseren</td>
        <td>16 uur (2 dagen)</td>
        <td>Feedback gebruiken om verbeteringen door te voeren</td>
    </tr>
</tbody>
</table>

 \* _Bovenop reguliere werkuren voor projectuitvoering._

## Fase 3b: Migratie van bestaande projecten
**Doel: Gecontroleerde migratie van bestaande projecten naar Filament.**

<table>
<thead>
    <tr>
        <th width="32%">Activiteit</th>
        <th width="28%">Duur</th>
        <th width="40%">Beschrijving</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td>MVP migratietool ontwikkelen</td>
        <td>ca. 40 uur (5 dagen)</td>
        <td>Ontwikkelen van een MVP voor een migratietool</td>
    </tr>
    <tr>
        <td>Migratie van kleine projecten</td>
        <td>40 uur (5 dagen per project)*</td>
        <td>Migreren van kleine projecten</td>
    </tr>
    <tr>
        <td>Migratie van middelgrote projecten</td>
        <td>120 uur (15 dagen per project)*</td>
        <td>Migreren van middelgrote projecten</td>
    </tr>
    <tr>
        <td>Optimalisatie van migratietool</td>
        <td>40 uur (5 dagen)</td>
        <td>Feedback verwerken en de migratietool optimaliseren</td>
    </tr>
</tbody>
</table>

\* _Naar verwachting zullen volgende migraties minder tijd in beslag nemen._

## Fase ∞: Doorlopende optimalisatie, migratie en opschaling
**Doel: Continue verbetering, migratie van projecten en opschaling voor bredere adoptie.**

<table>
<thead>
    <tr>
        <th width="32%">Activiteit</th>
        <th width="28%">Duur</th>
        <th width="40%">Beschrijving</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td>Feedback verzamelen</td>
        <td>-</td>
        <td>Actief feedback verzamelen van klanten, gebruikers en het developmentteam over de ervaringen met de nieuwe systemen</td>
    </tr>
    <tr>
        <td>Optimalisatie</td>
        <td>-</td>
        <td>Analyseren van feedback en continu optimaliseren van de systemen. Verbeter de performance, gebruikerservaring en schaalbaarheid</td>
    </tr>
    <tr>
        <td>Migratie van grote projecten</td>
        <td>-</td>
        <td>Migreren van grote en complexe projecten</td>
    </tr>
    <tr>
        <td>Doorlopende cyclus</td>
        <td>-</td>
        <td>Deze fase is een doorlopend proces van optimalisatie, migratie, en opschaling, waarbij steeds nieuwe projecten worden toegevoegd en bestaande systemen verder worden verfijnd</td>
    </tr>
</tbody>
</table>

# Conclusie

In dit adviesdocument worden, op basis van het uitgevoerde onderzoek, de mogelijkheden voor AllesOnline besproken om over te stappen van het huidige interne CMS naar **Filament** en **Statamic**. Hoewel het AllesOnline CMS jarenlang een solide basis heeft geboden voor webapplicaties, blijkt uit dit onderzoek dat het systeem inmiddels verouderd is en niet voldoende wordt onderhouden om bij te blijven met de groeiende eisen van klanten en technologische vooruitgang. Het verbeteren van het huidige CMS, bijvoorbeeld door de SOLID-principes toe te passen, zou een langdurig proces zijn dat aanzienlijke middelen vereist en niet in verhouding staat tot de voordelen van het inzetten van een externe oplossing.

Daarom wordt AllesOnline aangeraden om niet verder door te ontwikkelen aan het huidige systeem, maar in plaats daarvan externe systemen te omarmen. Filament biedt een uitstekende keuze voor projecten die complexiteit en maatwerk vereisen, zoals webapplicaties met een uitgebreid assortiment, grote hoeveelheden data of complexe klantenservicesystemen. De flexibiliteit van Filament, gecombineerd met de sterke integratie met Laravel, maakt het een krachtige oplossing voor deze typen projecten.

Statamic biedt een meer gestandaardiseerde, gebruiksvriendelijke oplossing voor marketingwebsites met dynamische contentbehoeften. Dit maakt het ideaal voor eenvoudigere projecten die snel en efficiënt moeten worden geïmplementeerd.

Op basis van de analyse en de voordelen van zowel Filament als Statamic wordt het aangeraden om Filament als primaire oplossing te gebruiken voor zowel de migratie van bestaande projecten als voor nieuwe ontwikkelingen. De compatibiliteit van Filament met het huidige AllesOnline CMS, de mogelijkheid om geautomatiseerde migraties te realiseren en de relatief vergelijkbare datastructuren maken het de meest logische keuze. Statamic kan daarentegen een waardevolle aanvulling zijn voor marketingwebsites of als een alternatief voor specifieke klantbehoeften.

Door de overstap naar deze externe systemen kan AllesOnline zich richten op zijn kerntaak: het ontwikkelen van klantgerichte oplossingen, terwijl de technologische basis stevig en toekomstbestendig blijft. Dit bevordert zowel de efficiëntie als de flexibiliteit van het bedrijf, en maakt het mogelijk om de focus volledig op klantbehoeften te richten, zonder het zware onderhoud van een eigen systeem.