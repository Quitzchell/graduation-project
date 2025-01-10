# **Adviesdocument**

# Inleiding

Dit document biedt een uitgebreid advies gebaseerd op het uitgevoerde onderzoek naar het AllesOnline CMS, Filament en Statamic. Het onderzoek richt zich op het evalueren van de huidige situatie van het AllesOnline CMS en op het identificeren van de beste oplossingen om toekomstige uitdagingen te overwinnen. 

In dit onderzoek worden de sterke en zwakke punten van het huidige AllesOnline CMS geanalyseerd. Vervolgens worden de mogelijkheden en beperkingen van Filament en Statamic vergeleken. Op basis van deze analyse biedt het document concrete aanbevelingen en een roadmap, waarmee AllesOnline de transitie naar een toekomstbestendig systeem kan realiseren.

## Context

AllesOnline, een full-service bureau voor online en offline communicatie, maakt gebruik van een intern ontwikkeld Content Management Systeem (CMS) dat sinds 2015 de basis vormt voor de webapplicaties van het bedrijf. Door een gebrek aan regelmatig onderhoud en updates is het systeem echter verouderd geraakt. Dit beperkt de uitbreidbaarheid en vermindert de efficiëntie van het gebruik ervan.

# AllesOnline CMS

## Voor eindgebruikers

Het AllesOnline CMS is een gebruiksvriendelijk systeem dat goed aansluit bij de behoeften van de eindgebruikers. Het biedt hen de mogelijkheid om de content en objecten op een website op flexibele wijze te beheren. Een van de sterkste punten van het CMS is de drag-and-drop-functionaliteit, waarmee pagina's snel en eenvoudig binnen één weergave kunnen worden gerangschikt.

## Voor developers

Hoewel het systeem voor de developers vertrouwd is, zijn er in het huidige AllesOnline CMS struikelblokken door verouderde en soms onvolledige documentatie. Dit leidt vaak tot onduidelijkheid over de beschikbare functionaliteiten, wat het moeilijk maakt om efficiënt met het systeem te werken. De inconsistente en complexe codestructuur versterkt dit probleem, waardoor het niet alleen lastig is om zonder documentatie de functionaliteiten te begrijpen, maar ook om bugs effectief op te sporen en op te lossen.

Daarnaast ontbreken geautomatiseerde tests die garanderen dat functionaliteiten blijven werken na het debuggen, refactoren of doorontwikkelen. Het opzetten van deze geautomatiseerde tests wordt bemoeilijkt door het gebrek aan naleving van de SOLID-principes. Veel functies binnen het CMS hebben meerdere verantwoordelijkheden, waardoor bepaalde use cases niet geïsoleerd getest kunnen worden.

Bovendien zijn er inconsistenties tussen bepaalde inputvelden, die ondanks gelijke functionaliteit verschillende syntaxis vereisen, wat ook zonder duidelijk documentatie tot verwarring leidt. De inputvelden die zijn ontworpen om relaties tussen objecten te definiëren, bieden geen ondersteuning voor complexe relaties zoals polymorfe relaties. Dit beperkt het gebruiksgemak voor eindgebruikers, omdat zij meerdere inputvelden moeten hanteren om dergelijke relaties vast te leggen, wat ten koste gaat van het overzicht.

## Hoe te verbeteren 

De doorontwikkeling zou in eerste instantie moeten focussen op het modulair en testbaar maken van de `FormModules`. Deze modules functioneren op het hoogste niveau binnen het systeem en zijn relatief onafhankelijk van andere onderdelen. Door ze modulairder en meer single-responsible te ontwerpen, wordt het mogelijk om geautomatiseerde tests te implementeren. Deze tests waarborgen dat bestaande functionaliteiten intact blijven bij wijzigingen of verdere ontwikkeling. Dit creëert een solide basis waarmee stapsgewijs naar lagere niveaus kan worden gewerkt, zoals de code die verantwoordelijk is voor het persisteren van objecten en content.

Op deze manier kunnen developers de scope van de doorontwikkeling stapsgewijs uitbreiden, terwijl ze tegelijkertijd werken aan een stevig fundament.

# Filament

**Filament** is een gebruiksvriendelijke component library voor het bouwen van adminpanels, dat een breed scala aan componenten biedt, zoals formulieren, tabellen en notificaties. Deze componenten kunnen eenvoudig worden aangepast aan de specifieke behoeften van de klant, wat Filament tot een veelzijdige keuze maakt.

## Sterke integratie met Laravel

Filament is volledig geïntegreerd met Laravel en maakt gebruik van de Eloquent ORM, waardoor gegevensbeheer met objectgeoriënteerde technieken wordt vereenvoudigd. Omdat AllesOnline al ervaring heeft met Laravel, zal de leercurve voor het team minimaal zijn.

## Gegevensbeheer

In Filament zijn `Resource`-classes verantwoordelijk voor het definiëren van hoe een `Eloquent`-model wordt beheerd in de gebruikersinterface. Deze klassen bepalen hoe gegevens worden weergegeven, ingevoerd en bewerkt. Daarnaast bieden ze tools voor het beheren van de gegevenslevenscyclus, waardoor vergelijkbare datastructuren zoals die in het AllesOnline CMS, kunnen worden gerealiseerd. Aangezien deze classes volledig in PHP zijn gedefinieerd, kunnen developers eenvoudig extra logica toevoegen en profiteren van autocompletion.

Ook ondersteunen de inputvelden omtrent relationships polymorfe relaties, wat het mogelijk maakt om een overzichtelijkere weergave voor eindgebruikers te realiseren.

## Marketplace

De Filament Marketplace benadrukt de uitbreidbaarheid van Filament, doordat het zowel gratis als betaalde externe plugins en componenten aanbiedt. Dit maakt het eenvoudig om bestaande oplossingen snel te integreren. Daarnaast stelt AllesOnline in staat om eigen componenten te ontwikkelen en toe te voegen aan de marketplace, wat ruimte biedt voor maatwerk en continue groei.

## Geavanceerde toegangscontrole

Dankzij de ingebouwde Laravel-permissions biedt Filament geavanceerde mogelijkheden voor toegangscontrole, waarmee rollen kunnen worden toegewezen aan gebruikers, en aan deze rollen specifieke rechten of permissies kunnen worden gekoppeld.

## Kosten

Het gebruik van Filament is gratis. Het Filament-team biedt echter wel de mogelijkheid om bepaalde features of bugs, die vermeld staan in de GitHub-issues, financieel te ondersteunen door middel van een donatie.

## Beperkingen en overwegingen

Het is belangrijk op te merken dat Filament geen kant-en-klare CMS-oplossing biedt. Hoewel het voldoende flexibiliteit biedt om CMS-functionaliteiten te implementeren, vereist dit extra ontwikkeltijd in vergelijking met systemen die deze functionaliteiten standaard aanbieden. Bovendien ontbreekt een drag-and-drop-interface, wat voor sommige klanten mogelijk als minder intuïtief kan worden ervaren. Een eenvoudige oplossing is het implementeren van een aparte menu-manager, terwijl een complexere benadering het ontwikkelen van een nieuw drag-and-dropcomponent zou zijn, dat specifiek aansluit bij de verwachte functionaliteiten van AllesOnline.

Filament is afhankelijk van Livewire en Alpine.js, twee technologieën die mogelijk nog niet volledig beheerst worden door het AllesOnline-team, wat extra training en educatie kan vereisen.

# Statamic

**Statamic** is een ander potentieel CMS voor AllesOnline, dat vooral geschikt is voor klanten die zich richten op marketingwebsite, zonder de noodzaak voor complexe logica of datastructuren. Het biedt veel kant-en-klare functionaliteiten die de implementatie van een CMS aanzienlijk kunnen versnellen. Dit maakt Statamic een aantrekkelijke keuze voor klanten die snel willen schakelen.

## Laravel-ish

In tegenstelling tot Filament is Statamic niet volledig geïntegreerd met Laravel, vooral in de flat-file en Eloquent-driver configuraties, waarbij gegevens als documenten in een repository of JSON in een database worden opgeslagen. Pas wanneer gekozen wordt voor de Runway-configuratie, wordt het mogelijk om voor elke entiteit specifieke Eloquent-modellen en databasetabellen te creëren. Het opzetten van deze configuratie is echter relatief complexer.

## Flat-file versus Database

Met Statamic is het mogelijk om eenvoudig te kiezen tussen een flat-file of een databasegebaseerde datalaag. De flat-filearchitectuur is eenvoudig, snel en geschikt voor kleine tot middelgrote websites. Echter, bij grotere hoeveelheden data of meerdere gelijktijdige bewerkingen, is een van de databaseconfiguraties de betere keuze. Deze bieden namelijk betere schaalbaarheid en geavanceerder gegevensbeheer door het gebruik van een database.

## Gegevensbeheer

Zoals eerder vermeld, is het gegevensbeheer in Statamic in de flat-file- en eloquent-driverconfiguraties documentgeoriënteerd. Dit maakt het beheren van gegevens flexibeler en stelt gebruikers in staat om inputvelden met polymorfe relaties te gebruiken. Dit voordeel verdwijnt echter bij het gebruik van de Runway-configuratie.

Het wisselen tussen de flat-file- en eloquent-driverconfiguratie is volledig geautomatiseerd in Statamic en kan eenvoudig worden uitgevoerd via een CLI-command.

## Multi-site

Statamic biedt ondersteuning voor multisite-configuraties, wat nuttig is wanneer meerdere websites of subdomeinen voor klanten binnen één installatie beheerd moeten worden. Dit kan echter de flexibiliteit en schaalbaarheid van de website beperken.

## DIY

De gebruiksvriendelijke interface stelt niet-technische gebruikers in staat om gestandaardiseerde websites op te zetten met voorgedefinieerde componenten. Dit biedt vormgevers bij AllesOnline de mogelijkheid om eenvoudige whitelabel-websites te realiseren, terwijl developers volledige controle behouden over de onderliggende codebase.

## Kosten 

De kosten voor Statamic zijn relatief flexibel en biedt verschillende licentieopties op basis van het aantal websites. De keuze voor de **Statamic Pro** licentie is noodzakelijk voor commerciële toepassingen, maar de **Master License** biedt een goede mogelijkheid om meerdere websites te beheren tegen een gereduceerde prijs. Bij verdere groei kan het abonnementsmodel van de **Platform Subscription** voordeliger zijn.

## Beperkingen en overwegingen

Statamic maakt voor de interface van het control panel gebruik van Vue 2 dat al is sinds eind 2023 EOL is. Statamic heeft aangegeven in het voorjaar van 2025 over te stappen naar Vue 3, wat betekent dat eventuele custom Vue 2-componenten naar Vue 3 gemigreerd moeten worden.

Omdat Statamic gebruik maakt van YAML-bestanden voor de configuratie van schema’s, kan de leercurve aanvankelijk wat steiler zijn, vooral door de soms versplinterde structuur van de documentatie.

## Migreren van bestaande AllesOnline CMS

Statamic maakt in al zijn configuraties gebruik van een afwijkende datasctructuur, wat aanzienlijke aanpassingen vereist bij een migratie. Dit vergroot de complexiteit van het proces, waardoor het niet raadzaam is om bestaande AllesOnline-projecten naar Statamic te migreren. Wanneer het te migreren project echter niet te groot is, maar de voordelen voor de eindgebruiker bij Statamic doorslaggevend zijn. kan de backend van de website relatief eenvoudig opnieuw worden opgebouwd in Statamic met de Eloquent-driverconfiguratie. Met een goed voorbereide migratie zou in dit geval enkel de bestaande database kunnen worden overgezet en aan de frontend worden gekoppeld.

# Advies en aanbevelingen

Mijn advies aan AllesOnline is om zijn horizon te verbreden en gebruik te maken van oplossingen die door externe partijen zijn ontwikkeld. Hoewel AllesOnline trots mag zijn op het eigen CMS, dat lange tijd het fundament van zijn webapplicaties vormde, is het belangrijk te erkennen dat dit systeem in diezelfde tijd onvoldoende is onderhouden. De investering die nodig is om het CMS te refactoren en te moderniseren, weegt niet op tot de kosten en voordelen van het inzetten van een bestaand product. Vooral omdat AllesOnline, met een systeem van externe partijen, in plaats van tijd en middelen te besteden aan het onderhouden van het eigen CMS, meer focus kan leggen op zijn kerntaak: het realiseren van oplossingen voor klanten. Het is daarom niet realistisch om het huidige systeem verder te ontwikkelen.

Voordat ik verder inga op welke externe oplossing de beste keuze is, wil ik eerst de belangrijkste les benoemen die ik tijdens mijn HBO-ICT-opleiding van docent Marco Meulenbroeks heb meegekregen: 'It depends'. In het laatste gesprek dat ik over mijn onderzoek met onze senior developer Wilco Kuijper had, benoemde hij, net zoals Meulenbroeks, dat we ons niet moeten vastbijten in één enkele oplossing. Naar mijn idee heeft hij hier helemaal gelijk in, het is namelijk belangrijk om te kijken welke oplossing het beste past bij de wensen van de klant. Als we kijken naar de twee systemen die in dit onderzoek zijn bekeken, zien we twee heel verschillende systemen die elk op hun eigen manier voor- en nadelen kennen voor verschillende projecten.

## Filament

De mogelijkheid om bestaande componenten direct via PHP aan te passen in Filament biedt veel flexibiliteit en maakt het een uitstekende keuze voor klanten die een complex, op maat gemaakt systeem nodig hebben. Dit geldt bijvoorbeeld voor webapplicaties met een uitgebreid assortiment, grote hoeveelheden data of complexe klantenservicesystemen. Voorbeelden van AllesOnline-projecten die hiervan zouden kunnen profiteren, zijn onder andere Provimi Body Condition Score, Ingrado, HVA en Klinger.

## Statamic

Statamic zou daarentegen een geschikte keuze kunnen zijn voor klanten die slechts een marketingwebsite nodig hebben, waarbij de content dynamisch beheerd wordt, maar waar geen complexe relaties en datastructuren worden verwacht. Een CMS voor dit soort websites kan met Statamic relatief sneller gerealiseerd worden dan met Filament. Denk hierbij aan klanten zoals DPD, Vierstroom of zelfs Westfalia.

Daarnaast zou het mogelijk zijn om een boilerplate voor Statamic te ontwikkelen met gestandaardiseerde componenten. Dit zou de vormgevers bij AllesOnline in staat stellen om zelfstandig whitelabel-websites te creëren via Statamic, terwijl de developers directe technische ondersteuning kunnen bieden wanneer dat nodig is.

Het enige nadeel van Statamic is dat de kosten voor een licentie in eerste instantie hoger zijn. Echter, naarmate meer websites gebruikmaken van Statamic, zullen de kosten per website dalen door de kwantumkorting binnen het abonnementsmodel van de **Platform Subscription**. In dit model betaal je voor de eerste 25 websites een vast bedrag van $175. Voor de volgende 75 websites betaal je $7 per website per maand, en voor de volgende 400 websites $6 per website per maand. Dit betekent dat de prijs per website daalt naarmate je meer websites toevoegt. Als er bijvoorbeeld 25 websites met Statamic actief zijn, zullen de kosten voor een licentie niet meer dan $7 per website bedragen.

## Combineren

Voor klanten die zowel complexiteit als een marketingwebsite verwachten, kan gesteld worden dat het inzetten van beide oplossingen een realistische keuze is. In dit geval zou bijvoorbeeld een adminpaneel voor stakeholders in Filament gerealiseerd kunnen worden, terwijl de marketingwebsite – of mogelijk zelfs whitelabel-websites – in Statamic beheerd kunnen worden. Op deze manier kan een diverse verzameling systemen worden opgeleverd, die elk afgestemd zijn op hun specifieke use cases. Dit zou bijvoorbeeld nuttig kunnen zijn voor projecten zoals OpleidingVinden of ScamAdviser.

## Voor migratie van bestaande projecten

Voor de migratie van bestaande projecten is het raadzaam om te migreren naar **Filament**. Dit komt doordat Filament in staat is om relatief dicht bij de datastructuur van het AllesOnline CMS te blijven. De benodigde aanpassingen zijn minimaal en kunnen in grote lijnen geautomatiseerd worden met behulp van een op maat gemaakte migratietool.

Omdat Filament volledig compatibel is met **Eloquent ORM**, kunnen modellen en gegevens uit het huidige systeem vrijwel direct worden hergebruikt. Deze compatibiliteit zorgt ervoor dat de overgang naar Filament soepel kan verlopen, zonder dat er ingrijpende wijzigingen nodig zijn in de onderliggende entiteitslaag. 

Er zijn ook risico's voor de migratie naar Filament. Dit geldt vooral voor de diversiteit aan architectuurkeuzes en custom componenten die in de loop der jaren voor projecten van het AllesOnline CMS zijn gemaakt. Omdat dit per project drastisch kan verschillen, is het mogelijk dat er tussen de verschillende projectmigraties edgecases naar voren komen. Dit maakt de ontwikkeling van een volledig geautomatiseerde tool complexer en brengt dus extra tijd met zich mee.

Het migreren naar **Statamic** is, vooral voor grotere projecten, minder geschikt vanwege de afwijkende datastructuren. Bij Statamic is een aanzienlijke herstructurering van de data vereist, wat voor complexe projecten een tijdrovende en kostbare operatie kan zijn.


## Langetermijneffecten

Hoewel de keuze om externe systemen te integreren nog steeds een investering is, kan het AllesOnline in de toekomst veel voordelen opleveren. Door te vertrouwen op doorontwikkelde oplossingen van externe partijen kan AllesOnline zich blijven aanpassen aan technologische innovaties, zonder de last van veroudering van een eigen systeem. Dit bevordert de duurzaamheid van de bedrijfsvoering en voorziet dat de focus kan liggen op het voldoen aan klantbehoeften in plaats van het onderhouden van verouderde systemen.

Bovendien positioneert het bedrijf zich op deze manier als een flexibele en toekomstgerichte speler in de markt, waardoor het aantrekkelijk blijft voor zowel klanten als talentvolle, toekomstige medewerkers. Strategisch gezien kan deze beslissing bijdragen aan stabiele groei en biedt het een concurrentievoordeel, doordat de bedrijfsfocus volledig gericht blijft op klantgerichte innovatie en creatieve oplossingen.

# Roadmap

## **Fase 1: Voorbereiding, kennisopbouw en validatie (3 maanden)**
**Doel: Developers snel vertrouwd maken met Filament en Statamic en een werkend prototype creëren.**

### **Maand 1: Introductie en versneld leren**
* **Week 1-2: Introductie van de systemen**
    * Presentatie aan het team: Waarom Filament en Statamic?
    * Kijken van **Laracast-workshops** over Filament en Statamic.
    * Bespreken van eerste indrukken door het development-team.
  
* **Week 3-4: Hands-on kennismaking**
    * Organiseer een hackathon: 
        * [Bouw een klein prototype CMS met Filament en Statamic](../Bijlagen/HackatonBouwCMS.md)
    * **Output**: Eerste hands-on ervaring en input voor de volgende stappen.

### **Maand 2-3: Prototype en evaluatie**
* **Week 1-2: Training Livewire en Alpine.js**
    * Toewijzing van **Laracast-workshops** voor Livewire en Alpine.js.
    * Organiseer een hackathon:
        *  [Bouw een custom Filament component met Livewire en Alpine.js.](../Bijlagen/HackatonBouwFilamentComponent.md)

* **Week 3-4: Ontwikkel CMS-plugin voor Filament**
    * Realisatie van een eenvoudige generieke Filament-plugin met basisfunctionaliteiten voor:
        * Templates
        * Contentblocks
        * Menustructuur
        * Persisteren van CMS content
    * Focus op modulaire opzet.
    * Stel documentatie op voor gebruik binnen het team.

* **Week 4-6: Validatie van plugin**
  * Koppel het CMS aan een bestaand project met een *decoupled architectuur*.
  * Realiseer dat een deel van de templates, blokken en menustructuren goed gerenderd kunnen worden met de Filament-plugin. //todo

**Deliverables**:
* Gevalideerde CMS-plugin voor Filament.
* Documentatie voor het gebruik van de Filament-plugin.

## **Fase 2: Ontwikkeling van tools en eerste projecten (5-6 maanden)**
**Doel: Start met concrete projecten en verder ontwikkelen van systemen.**

### **Maand 4-6: Start ontwikkeling greenfield project**
* **Identificeer nieuwe projecten** die geschikt zijn voor Filament en/of Statamic.
* Begin met een **agile aanpak**: Start met het ontwikkelen van de CMS-functionaliteiten voor het eerste greenfield project.
* Werk met een minimal viable product (MVP): Begin met de basisfunctionaliteit van het CMS en breid deze iteratief uit.

### **Maand 6: Eerste project live en optimalisatie**
* **Launch het eerste Filament- en/of Statamic-project**.
* Verzamel feedback van klanten en gebruikers.
* Begin met de **optimalisatie** van de systemen en tools op basis van deze feedback.

**Deliverables**:
* Eerste projecten live en werkend met Filament en Statamic.
* Feedbackloops geïmplementeerd voor doorontwikkeling.

## **Fase 3: Migratie bestaande projecten (4-5 maanden)**
**Doel: Gecontroleerde migratie van bestaande projecten naar Filament.**

### **Maand 7-9: Start migratietool en kleine migraties**
* **Ontwikkel een migratietool** voor het omzetten van AllesOnline CMS-projecten naar Filament.
* Start met migratie van kleinere, eenvoudige projecten.
* Implementeer verbeteringen in de tool op basis van feedback van de eerste migratie(s).

### **Maand 10-12: Grotere migraties en optimalisatie**
* Begin met de migratie van grotere, complexere projecten.
* Verfijn de migratietool en pas deze aan op basis van de ervaringen uit de eerdere migraties.
* Zorg ervoor dat het migratieproces geoptimaliseerd wordt voor snelheid en efficiëntie.

**Deliverables**:
* Kleine migraties afgerond.
* Grotere projecten in migratieproces.

## **Fase 4: Opschaling en optimalisatie (2-3 maanden)**
**Doel: Brede adoptie en verdere verfijning van systemen en processen.**

### **Maand 13-15: Verbeteren en opschalen**
* Optimaliseer **ontwikkelde tools en plugin** voor meer efficiëntie.
* **Brede adoptie** van de nieuwe systemen voor andere projecten binnen het bedrijf.


### **Maand 16-18: Afronden en verdere verfijning**
* Zorg ervoor dat **alle bestaande projecten** succesvol zijn overgezet naar Filament. 
* Werk aan **lange termijnstrategie** om de systemen toekomstbestendig te houden.

**Deliverables**:
  * AllesOnline volledig over op Filament en Statamic.
  * Geoptimaliseerde tools en systemen voor toekomstig gebruik.


# Conclusie

In dit adviesdocument zijn, op basis van het uitgevoerde onderzoek, de mogelijkheden voor AllesOnline om van het interne CMS over te stappen naar **Filament** en **Statamic** geformuleerd. Het onderzoek onderbouwd dat het huidige AllesOnline CMS, hoewel het enkele voordelen biedt, beperkingen vertoont op het gebied van veroudering, onderhoud en uitbreidbaarheid. Het is daarom niet verantwoord om de capaciteit van het developmentteam te benutten voor het onderhouden en doorontwikkelen van dit systeem.

Filament is een uitstekende keuze voor projecten die complexe, op maat gemaakte systemen vereisen, zoals webapplicaties met een uitgebreid assortiment, grote hoeveelheden data of complexe klantenservicesystemen. Dit mede omdat de componenten vna Filament eenvoudig kunnen worden aangepast via PHP. Ook de sterke integratie met Laravel is een voordeel voor het developmentteam van AllesOnline.

Statamic, aan de andere kant, biedt een uitstekende oplossing voor marketingwebsites met dynamische contentbehoeften en kan snel en efficiënt worden geïmplementeerd voor deze minder complexe projecten. Het is een gebruiksvriendelijke keuze voor klanten die behoefte hebben aan een gestandaardiseerde websiteoplossing zonder veel technische diepgang.

Op basis van de analyse en de verschillende voordelen van Filament en Statamic wordt het aanbevolen om voor de migratie van bestaande projecten en nieuwe ontwikkelingen te kiezen voor Filament als de primaire oplossing. De compatibiliteit met het huidige AllesOnline CMS, de mogelijkheid om geautomatiseerde migraties te ontwikkelen en de relatief vergelijkbare datastructuren maken Filament de beste keuze. Statamic kan echter worden ingezet voor eenvoudigere marketingwebsites of als een alternatief voor specifieke klantbehoeften.