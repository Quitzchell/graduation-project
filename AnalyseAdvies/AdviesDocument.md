# **Adviesdocument**

# Inleiding

Dit document bevat een uitgebreid advies, gebaseerd op onderzoek, voor AllesOnline over het potentieel om één of meerdere nieuwe CMS-systemen verder te ontwikkelen of te implementeren voor de webapplicaties die zij ontwikkelen.

## Context

AllesOnline - een full-service bureau voor online en offline communicatie - haar huidige intern ontwikkelde Content Management Systeem (CMS), dat momenteel in gebruik is en sinds 2015 operationeel is, vormt de basis van de webapplicaties die het bedrijf ontwikkelt. Door een gebrek aan aandacht en onderhoud is het systeem echter verouderd. Dit maakt het moeilijk uit te breiden, waardoor het werken met het systeem steeds minder efficiënt wordt.

# AllesOnline CMS

## Voor eindgebruikers

Het AllesOnline CMS is een gebruiksvriendelijk systeem dat goed aansluit bij de behoeften van de eindgebruikers. Het biedt hen de mogelijkheid om de content en objecten op een website op flexibele wijze te beheren. Een van de sterke punten van het CMS is de drag-and-drop-functionaliteit, waarmee pagina's snel en eenvoudig binnen één weergave kunnen worden gerangschikt.

## Voor developers

Hoewel het systeem voor de developers vertrouwd is, zijn er in het huidige AllesOnline CMS struikelblokken door verouderde en soms onvolledige documentatie. Dit leidt vaak tot onduidelijkheid over de beschikbare functionaliteiten, wat het moeilijk maakt om efficiënt met het systeem te werken. De inconsistente en complexe codestructuur versterkt dit probleem, waardoor het niet alleen lastig is om zonder documentatie de functionaliteiten te begrijpen, maar ook om bugs effectief op te sporen en op te lossen.

Daarnaast ontbreken geautomatiseerde tests die garanderen dat functionaliteiten blijven werken na het debuggen, refactoren of doorontwikkelen. Het opzetten van deze geautomatiseerde tests wordt bemoeilijkt door het gebrek aan naleving van de SOLID-principes. Veel functies binnen het CMS hebben meerdere verantwoordelijkheden, waardoor bepaalde use cases niet geïsoleerd getest kunnen worden.

Bovendien zijn er inconsistenties tussen bepaalde inputvelden, die ondanks gelijke functionaliteit verschillende syntaxis vereisen, wat ook zonder duidelijk documentatie leidt tot verwarring. De inputvelden van het CMS ondersteunen ook geen complexe relaties, zoals polymorfe relaties, wat het overzicht voor eindgebruikers vermindert omdat meerdere inputvelden gebruikt moeten worden.

## Hoe te verbeteren 

In grote lijnen zal het doorontwikkelen zich in eerste instantie moeten richten op het modulair en testbaar maken van de FormModules. Deze modules bevinden zich in principe op het hoogste niveau in het systeem en dragen minder verantwoordelijkheid voor de werking van de rest van het systeem. Door deze modules modulairder en meer single-responsible te maken, is het mogelijk om geautomatiseerde tests te realiseren die kunnen garanderen dat bestaande functionaliteiten niet breken wanneer er wijzigingen of doorontwikkeling plaatsvinden. Vanuit dit fundament kan verder worden doorgewerkt naar de lagere niveaus, waar onder andere het persisteren van objecten en content wordt uitgevoerd.

Op deze manier kunnen developers de scope van de doorontwikkeling stap voor stap uitbreiden, terwijl ze voortdurend bouwen aan een stevig fundament.

# Filament

**Filament** is een gebruiksvriendelijke component library voor het bouwen van adminpanels, dat een breed scala aan componenten biedt, zoals formulieren, tabellen en notificaties. Deze componenten kunnen eenvoudig worden aangepast aan de specifieke behoeften van de klant, wat Filament tot een veelzijdige keuze maakt.

## Sterke integratie met Laravel

Filament is volledig geïntegreerd met Laravel, waarbij gebruik wordt gemaakt van de Eloquent ORM, wat het beheer van gegevens via objectgeoriënteerde technieken vergemakkelijkt. Aangezien AllesOnline al bekend is met Laravel, zal de leercurve voor het team relatief laag zijn. 

## Gegevensbeheer

In Filament zijn de Resource-classes verantwoordelijk voor het definiëren van hoe een Eloquent-model wordt beheerd binnen de gebruikersinterface. Deze classes bepalen hoe gegevens worden weergegeven, ingevoerd en bewerkt. Daarnaast bieden ze tools voor het beheren van de gegevenslevenscyclus, wat het mogelijk maakt om vergelijkbare datastructuren te gebruiken, zoals die in het AllesOnline CMS. Aangezien deze classes volledig in PHP worden gedefinieerd, kunnen developers eenvoudig extra logica toevoegen en profiteren van autocompletion. 

Ook ondersteunen de inputvelden omtrent relationships polymorfe relaties, wat het mogelijk maakt om een overzichtelijkere weergave voor eindgebruikers te realiseren.

## Marketplace

Met de mogelijkheid om externe plugins en componenten te importeren via de Filament Marketplace, waar zowel gratis als betaalde opties beschikbaar zijn. AllesOnline heeft zelfs de mogelijkheid om eigen componenten te ontwikkelen en aan de marketplace toe te voegen.

## Geavanceerde toegangscontrole

Met de ingebouwde Laravel-permissions biedt Filament geavanceerde mogelijkheden voor toegangscontrole, zodat specifieke rollen en rechten aan gebruikers kunnen worden toegewezen.

## Kosten
Aan het gebruik van Filament zijn geen kosten verbonden. Wel biedt het team de mogelijkheid om bepaalde features of bugs die in de github-issues te vinden zijn te sponsoren met een gelddonatie. 

## Beperkingen en overwegingen

Het is belangrijk te vermelden dat Filament geen kant-en-klare CMS-oplossing biedt. Hoewel het flexibel genoeg is om CMS-functionaliteiten te implementeren, vereist dit extra ontwikkeltijd in vergelijking met systemen die deze functionaliteiten standaard aanbieden. Daarnaast ontbreekt een drag-and-drop-interface, wat voor sommige klanten mogelijk als minder intuïtief kan worden ervaren. Een alternatief is het implementeren van een aparte menu-manager.

Filament afhankelijk van Livewire en Alpine.js, technologieën die mogelijk nog niet volledig beheerst worden door het team van AllesOnline, wat extra training kan vereisen.

# Statamic

**Statamic** is een ander potentieel CMS voor AllesOnline, dat vooral geschikt is voor klanten die zich richten op marketingwebsite, zonder de noodzaak voor complexe logica of datastructuren. Het biedt veel kant-en-klare functionaliteiten die de implementatie van een CMS versnellen.
Dit maakt Statamic een aantrekkelijke keuze voor klanten die snel willen schakelen, zonder concessies te doen aan de flexibiliteit.

## DIY

De gebruiksvriendelijke interface maakt het mogelijk voor niet-technische gebruikers om met voorgedefineerde componenten, gestandaardiseerde websites op te zetten. Dit maakt het mogelijk om vormgevers van AllesOnline eenvoudige whitelabel websites te realiseren, terwijl de developers volledige controle houden over de onderliggende codebase.

## Laravel-ish

In tegenstelling tot Filament is Statamic niet volledig geintegreert met Laravel, vooral niet in de flat-file en eloquent-driver configuraties waar gegevens als documenten in de repository of JSON in een database worden gepersisteerd. Pas wanneer er gekozen wordt voor de Runway-configuratie is het mogelijk om per entiteit specifieke eloquent-modellen en databasetabellen te realiseren. Echter is het opzetten van deze configuratie relatief complexer. 

## Flat-file versus Database

Met Statamic is het mogelijk om gemakkelijk te kiezen tussen een flat-file of databasegebaseerde datalaag. De flat-filearchitectuur is eenvoudig, snel en geschikt voor kleine tot middelgrote websites. Echter, bij grotere hoeveelheden data of meerdere tegelijktijdige bewerkingen, is een van de databaseconfiguraties een betere keuze. Deze zijn namelijk beter schaalbaar en hebben door het gebruik van een database geavanceerdere gegevensbeheer.

Voor de databasegebaseerde datalaag is het aan te raden om te kiezen tussen twee opties. De eloquent-driver, die de flat-file documenten als JSON in de database opslaat, of Runway die het mogelijk maakt om gegevens in de interface doormiddel van specifieke eloquent-modellen te beheren.

## Gegevensbeheer

Zoals eerder genoemd, is gegevensbeheer van Statamic is in de flat-file- en eloquent-driverconfiguratie documentgeoriënteerd. Dit maakt het beheren van gegevens flexibeler, waardoor inputvelden met polymorfe relaties mogelijk zijn. Dit voordeel gaat echter verloren in de Runway-configuratie.

## Multi-site

Statamic ondersteuning voor multisite-configuraties, wat handig kan zijn wanneer je meerdere websites of subdomeinen voor klanten binnen één installatie wilt beheren. Dit kan echter wel de flexibiliteit en schaalbaarheid van de website beperken.

## Kosten 

De kostens voor Statamic is relatief flexibel en biedt verschillende licentieopties op basis van het aantal websites. De keuze voor de **Statamic Pro** licentie is noodzakelijk voor commerciële toepassingen, maar de **Master License** biedt een goede mogelijkheid om meerdere websites te beheren tegen een gereduceerde prijs. Bij verdere groei kan het abonnementsmodel van de **Platform Subscription** voordeliger zijn.

## Beperkingen en overwegingen

Statamic maakt voor de interface van het control panel gebruik van Vue 2 dat al is sinds eind 2023 EOL is. Statamic heeft aangegeven in het voorjaar van 2025 over te stappen naar Vue 3, wat betekent dat aangepaste Vue 2-componenten naar Vue 3 gemigreerd moeten worden.

Omdat Statamic gebruik maakt van YAML-bestanden voor de configuratie van schema’s, kan de leercurve aanvankelijk wat steiler zijn, vooral door de soms versplinterde structuur van de documentatie.

## Migreren van bestaande AllesOnline CMS

Statamic maakt in al zijn configuraties gebruik van een afwijkende datastructuur. Hierdoor is het niet raadzaam om bestaande AllesOnline-projecten naar Statamic te migreren. Als het te migreren project echter niet al te groot is, is het mogelijk om de backend van de website relatief weinig moeite opnieuw op te bouwen in Statamic met de eloquent-driver configuratie. Met een goed voorbereide migratie kan de bestaande database worden overgezet en aan de frontend worden gekoppeld.

# Advies en aanbevelingen

Mijn advies aan AllesOnline is om zijn horizon te verbreden en gebruik te maken van oplossingen die door externe partijen zijn ontwikkeld. Hoewel AllesOnline trots mag zijn op het eigen CMS, dat lange tijd het fundament van zijn webapplicaties vormde, is het belangrijk te erkennen dat dit systeem in diezelfde tijd onvoldoende is onderhouden. De investering die nodig is om het CMS te refactoren en te moderniseren, weegt niet op tot de kosten en voordelen van het inzetten van een bestaand product. Vooral omdat AllesOnline, met een systeem van externe partijen, in plaats van tijd en middelen te besteden aan het onderhouden van het eigen CMS, meer focus kan leggen op zijn kerntaak: het realiseren van oplossingen voor klanten. Het is daarom niet realistisch om het huidige systeem verder te ontwikkelen.

Voordat ik verder inga op welke externe oplossing de beste keuze is, wil ik eerst de belangrijkste les benoemen die ik tijdens mijn HBO-ICT-opleiding van docent Marco Meulenbroeks heb meegekregen: 'It depends'. In het laatste gesprek dat ik over mijn onderzoek met onze senior developer Wilco Kuijper had, benoemde hij, net zoals Meulenbroeks, dat we ons niet moeten vastbijten in een oplossing. Naar mijn idee heeft hij hier helemaal gelijk in, het is namelijk belangrijk om te kijken welke oplossing het beste past bij de wensen van de klant. Als we kijken naar de twee systemen die ik in dit onderzoek heb bekeken, zien we twee heel verschillende systemen die elk op hun eigen manier voor- en nadelen kennen voor verschillende projecten.

## Filament

De vrijheid om op maat gemaakte logica toe te passen in Filament maakt het een uitstekende keuze wanneer een klant complexere relaties en logica voor zijn systeem nodig heeft. Dit geldt bijvoorbeeld voor webapplicaties met een uitgebreid winkelassortiment, grote hoeveelheden data of complexe klantenservicesystemen. Voorbeelden van AllesOnline-projecten die hiervan zouden kunnen profiteren, zijn onder andere Provimi Body Condition Score, Ingrado, HVA en Klinger.

## Statamic

Statamic zou daarentegen een geschikte keuze kunnen zijn voor klanten die slechts een marketingwebsite nodig hebben, waarbij de content dynamisch beheerd wordt, maar waar geen grote hoeveelheden data of complexe logica worden verwacht. Een CMS voor dit soort websites kan met Statamic relatief sneller gerealiseerd worden dan met Filament. Denk hierbij aan klanten zoals DPD, Vierstroom of zelfs Westfalia.

Daarnaast zou het mogelijk zijn om voor Statamic een boilerplate te ontwikkelen met gestandaardiseerde componenten. Dit zou onze vormgevers in staat stellen om via Statamic zelfstandig whitelabel websites te creëren, terwijl onze developers direct bij de code kunnen wanneer dat nodig is. 

Het enige nadeel voor Statamic is dat de kosten voor een licentie in eerste instantie hoger zijn. Echter, naarmate meer websites gebruikmaken van Statamic, zullen de kosten per website dalen door de kwantumkorting binnen het abonnementsmodel van de **Platform Subscription**.

## Combineren

Voor klanten waar zowel complexiteit als een marketingwebsite wordt verwacht, zou gesteld kunnen worden dat het inzetten van beide oplossingen een realistische keuze is. In dit geval zou bijvoorbeeld een adminpaneel voor stakeholders in Filament gerealiseerd kunnen worden, terwijl de marketingwebsite, of wellicht zelfs whitelabel-websites, in Statamic beheerd kunnen worden. Dit zou bijvoorbeeld nuttig zijn voor projecten zoals OpleidingVinden of ScamAdviser.


## Voor migratie van bestaande projecten
--> gebruik Filament

## Langetermijneffecten

Hoewel de keuze om externe systemen te integreren nog steeds een investering is, kan het AllesOnline in de toekomst veel voordelen opleveren. Door te vertrouwen op doorontwikkelde oplossingen van externe partijen kan AllesOnline zich blijven aanpassen aan technologische innovaties, zonder de last van veroudering van een eigen systeem. Dit bevordert de duurzaamheid van de bedrijfsvoering en voorkomt dat technologische achterstanden de groei belemmeren. 

Bovendien positioneert het bedrijf zich als een flexibele en toekomstgerichte speler in de markt, waardoor het aantrekkelijk blijft voor zowel klanten als talentvolle medewerkers. Op strategisch niveau kan deze keuze bijdragen aan stabiele groei en een concurrentievoordeel op de lange termijn, omdat de focus van het bedrijf optimaal kan worden benut voor klantgerichte innovatie en creatie.

# Roadmap
## Fase 1: Voorbereiding, kennisopbouw en validatie (3 maanden)
**Doel: Developers vertrouwd maken met Filament en Statamic en inzicht krijgen in hoe de systemen werken.**

### Maand 1: Introductie
* **Week 1-2 Introductie van de systemen**
    * Presentatie van de plannen aan het team: waarom Filament en Statamic?
    * Toewijzigen van **Laracast-workshops** voor Filament en Statamic.
    * Bespreken van eerste indrukken op basis van eigen onderzoek van development-team. 
 
* **Week 3-4 Hands-on kennismaking**
    * Organiseer een hackaton:
        * [Hackathon: Bouw van CMS met Filament en Statamic](../Bijlagen/OpzetHackatonBouwCMS.md)
    * **Output**: Eerste hands-on ervaring en input voor volgende stappen.

**Maand 2-3: Prototype en evaluatie**
* **Week 1-2: Training Livewire en Alpine.js**
    * Toewijzigen van **Laracast-workshops** voor Livewire en Alpine.js.
    * Organiseer een hackaton: 
        * [Hackaton: Bouw een custom Filament component met Livewire en Alpine.js](../Bijlagen/HackatonBouwFilamentComponent.md) 
* **Week 2-8: Ontwikkel pluggin met CMS-functionaliteiten Filament**
    * Realisatie van generieke Filament-plugin met kernfunctionaliteiten voor: 
        * Templates
        * Contentblocks
        * Menustructuur
        * Persisteren van CMS content
    * Focus op modulariteit en opzet.
    * Stel documentatie op voor gebruik binnen het team.

* **Week 8-12: Validatie van plugin**
    * Gebruik een bestaand project met een *decoupled architectuur* om de plugin te valideren. 
        * Koppel de frontend aan nieuwe CMS.
        * Realiseer dat een aantal van de templates, blocken en menustructuur gerendeerd kan worden. 
    
**Deliverables**
* Gevalideerde plugin voor Filament met functionaliteiten voor CMS.
* Documentatie over Filament plugin. 

## Fase 2: Ontwikkeling van tools en eerste projecten (6 maanden)
**Doel: Praktische toepassing van nieuwe systemen in projecten.**

**Maand 5-12: Start ontwikkeling van greenfield project met Filament en Statamic**
    * Indentificeer nieuwe projecten die geschikt zijn voor een eerste project met Filament en/of Statamic.
    * Werk met een agile aanpak om de CMS-functionaliteiten te realiseren in een praktische omgeving. 
    
**Deliverable**
* Eerste Filament -en Statamic-projecten live.

## Fase 3: Migratie bestaande projecten (12 - 18 maanden)
**Doel**: Gefaseerd migreren van bestaande projecten naar Filament.

* **Maand 6-9: Start met migratietool** 
    * Ontwikkel een tool voor het omzetten van een AllesOnline CMS projecten naar Filament.
    * Begin met de migratie van een klein, eenvoudig project.
* **Maand 9-10**: 
    * Verbeter de tool op basis van feedback van de eerste migratie(s).

* **Maand 10-15: Migratie van grotere projecten**:
    * Start migratie van grotere projecten, gebaseerd op ervaring uit de eerste migraties.

**Deliverables**
* Migraties afgerond voor kleinere projecten.
* Migratie van grotere projecten in uitvoering.

## Fase ∞: Opschaling en optimalisatie
**Doel: Brede adoptie van de nieuwe systemen en verdere optimalisatie.**
    * Optimaliseer ontwikkelde tools en plug-ins voor meer efficiëntie.
    
**Deliverables:**
* AllesOnline volledig over op de nieuwe systemen.


# Conclusie

