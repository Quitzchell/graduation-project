## Revitalising Content Management
***Versie 3***
## AllesOnline
AllesOnline is een full-service bureau dat zich richt op zowel online als offline communicatie. Het bedrijf bedient een diverse groep stakeholders, van grote internationale ondernemingen tot regionale servicegerichte instanties. De diensten die ze aanbieden variëren van productiegerelateerde werkzaamheden, zoals DTO en vormgeving, tot het ontwerpen en realiseren van full-stack webapplicaties.
## Content Management Systeem
Een belangrijk onderdeel van deze webapplicaties is het Content Management Systeem, ontwikkeld door AllesOnline zelf. Dit systeem, waarvan de eerste commit dateert van 13 februari 2015, kan met recht als legacy worden beschouwd. Hoewel er sinds de start van het project voornamelijk functionele uitbreidingen zijn doorgevoerd, ontbreekt het aan grondige refactoring of verbeteringen op het gebied van onderhoudbaarheid, zoals geautomatiseerde tests. Daardoor is het CMS nog wel uitbreidbaar, maar sommige modules zijn onnodig complex geworden, wat verdere ontwikkeling lastig en arbeidsintensief maakt.

De huidige staat van het CMS heeft verschillende oorzaken. Ten eerste levert de doorontwikkeling van het CMS geen directe inkomsten op voor AllesOnline, terwijl het onderhoud en de ontwikkeling ervan wel aanzienlijke kosten met zich meebrengen. Bovendien zijn sommige ontwikkelaars binnen het bedrijf terughoudend om de bestaande ‘spaghetticode’ te refactoren, voornamelijk vanwege het gebrek aan een vangnet zoals geautomatiseerde tests. Hierdoor is er geen garantie dat wijzigingen in het systeem voor één website geen problemen veroorzaken voor andere websites die hetzelfde CMS gebruiken.

Het is dus duidelijk dat het CMS, het fundament van de door AllesOnline aangeboden webapplicaties, dringend aandacht nodig heeft.
## Noodzaak voor sterker fundament
Het ontbreken van een sterk fundament in het CMS beperkt de schaalbaarheid en flexibiliteit, wat innovaties moeilijker maakt zonder concessies te doen aan de stabiliteit en betrouwbaarheid van het systeem. De verouderde codebase vormt een structureel probleem dat niet eenvoudig kan worden opgelost met tijdelijke aanpassingen. Een grondige refactor van het CMS, in combinatie met geautomatiseerde tests, zou de kwaliteit en het onderhoud verbeteren, en een solide basis leggen voor toekomstige innovaties. Dit zou het CMS in lijn brengen met moderne ontwikkelpraktijken en AllesOnline in staat stellen huidige en toekomstige uitdagingen met vertrouwen aan te gaan.

Echter, gezien de omvang van zo’n refactor is het wellicht verstandiger om alternatieve systemen te overwegen die duurzamer en kostenefficiënter zijn. Deze systemen zouden al voldoen aan moderne ontwikkelpraktijken en betere ondersteuning bieden voor toekomstige innovaties. Daarom zal het onderzoek zich richten op de haalbaarheid en voordelen van migreren naar een bestaand CMS-pakket, dat al voldoet aan moderne ontwikkel standaarden. Dit kan op lange termijn een duurzamere en kosten efficiëntere oplossing bieden dan verdere investeringen in het huidige systeem.
## Onderzoeksvraag
 Hoe kan AllesOnline een bestaand Content Management Systeem inzetten om de schaalbaarheid, onderhoudbaarheid en toekomstbestendigheid van haar webapplicaties te verbeteren?
### Deelvragen

1. **Wat zijn de belangrijkste technische en functionele beperkingen van het huidige CMS van AllesOnline?**
    
2. **Aan welke criteria moet een gemoderniseerd CMS voldoen om de huidige en toekomstige behoeften van AllesOnline te ondersteunen?**
    
3. **Welke commerciële en open-source CMS-oplossingen voldoen aan de vereisten voor modernisering en kunnen een haalbare vervanging bieden voor het huidige systeem?**
    
4. **Hoe verloopt de migratie van de bestaande webapplicaties naar een nieuw CMS, en welke technische uitdagingen komen hierbij kijken?**
    
5. **Welke prestatieverschillen en kosten zijn er tussen het huidige CMS en een nieuw systeem?**

## Aanpak voor modernisering van het CMS
Om binnen een realistische termijn tot een goed onderbouwd advies te komen, ben ik van plan om gedurende dit project verschillende casestudies uit te voeren. Mijn doel is om met een concreet advies te komen over hoe AllesOnline de architectuur rondom het gebruik van een CMS kan moderniseren. Het is hierbij essentieel om rekening te houden met het feit dat bestaande webapplicaties, die momenteel het huidige CMS gebruiken, moeten kunnen worden gemigreerd naar de nieuwe architectuur.

Om tot een goed afgewogen advies te komen, ben ik van plan een eenvoudige webapplicatie te ontwikkelen met het huidige CMS. Deze applicatie zal als blauwdruk dienen voor de functionaliteiten die nodig zijn voor een typisch AllesOnline webapplicatie. Op basis van deze applicatie zal ik andere CMS oplossingen uittesten. Dit stelt mij in staat om gemakkelijk te onderzoeken hoe de migratie van het huidige CMS naar andere CMS-pakketten kan plaatsvinden.

Om te controleren of de migratie correct verloopt, zal ik tests uitvoeren om te verifiëren of het CMS nog steeds het gewenste resultaat levert. Dit omvat het testen van de rendering van webpagina's om te waarborgen dat alle bestaande inhoud behouden blijft. Ook zal ik aandacht besteden aan de integriteit van de data en de werking van de bestaande functionaliteiten, om te garanderen dat de overgang naar de nieuwe architectuur soepel verloopt zonder verlies van functionaliteit of gegevens.

Door deze aanpak hoop ik vooral te bepalen of migratie naar een bestaand CMS-pakket een efficiëntere oplossing biedt dan het doorontwikkelen van het huidige CMS, en praktische richtlijnen te formuleren voor een soepele migratie.
### Fase 1: Onderzoek en initieel advies
In de eerste fase van het project wordt een analyse uitgevoerd van het bestaande Content Management Systeem (CMS) van AllesOnline. Deze fase richt zich op het vaststellen van de vereisten voor modernisering. Het doel is om een goed onderbouwd advies te formuleren over de beste aanpak, waarbij de nadruk ligt op de mogelijkheden van migratie naar een bestaand CMS-pakket dat voldoet aan moderne standaarden en een duurzamere oplossing biedt dan het huidige CMS.

Het onderzoek begint met een functionele evaluatie van het huidige CMS. Hierbij worden de bestaande functionaliteiten onder de loep genomen om inzicht in het systeem te krijgen. Vervolgens wordt vastgelegd aan welke criteria een nieuw CMS moet voldoen, zoals schaalbaarheid, onderhoudbaarheid, kosten, en ondersteuning voor moderne ontwikkelpraktijken. Parallel aan deze evaluatie wordt marktonderzoek verricht naar beschikbare commerciële en open-source CMS-oplossingen die aan de gedefinieerde criteria voldoen. 

In deze fase worden ook de specificaties en functionaliteiten voor een prototype webapplicatie, die de kernfunctionaliteiten van het huidige CMS bevat, vastgesteld.
### Fase 2: Iteratieve ontwikkelingscyclus
In de tweede fase wordt een iteratieve ontwikkelingscyclus gevolgd om te onderzoeken hoe de modernisering en migrate van het CMS kan plaatsvinden. Hier wordt voortgebouwd op de resultaten van Fase 1.

**Deze fase omvat de volgende stappen**

1. **Ontwikkeling**: De webapplicatie wordt ontwikkeld met het huidige CMS, waarbij de ontwikkelings- en testprocessen worden gedocumenteerd. De nadruk van de vervolgsprints ligt op het ontwikkelen van een webapplicatie met bestaande CMS-oplossingen, waarbij wordt onderzocht of deze systemen de vereiste functionaliteiten en flexibiliteit bieden zonder de noodzaak voor uitgebreide aanpassingen.
     
2. **Migratieplan ontwikkelen**: Een gedetailleerd migratieplan wordt opgesteld op basis van proefimplementaties van verschillende CMS-oplossingen. Dit plan bevat stappen voor de migratie van gegevens, integratie en testen.
     
3. **Uitvoering van migratieproeven**: Er worden migratieproeven uitgevoerd om te testen hoe goed de nieuwe systemen presteren bij het overzetten van bestaande gegevens en functionaliteiten.
     
4. **Functionele tests**: Functionele tests worden uitgevoerd om te controleren of de nieuwe of aangepaste CMS-oplossing alle gewenste functionaliteiten ondersteunt en correct functioneert.
     
5. **Prestatie- en stress tests**: De prestaties en belasting van de webapplicatie worden onder verschillende omstandigheden getest om te waarborgen dat het systeem aan de schaalbaarheidsvereisten voldoet.
     
6. **Gebruikerstest**: Eindgebruikers testen de nieuwe of gerefactoreerde CMS-oplossing om de gebruikerservaring te evalueren en eventuele problemen met de gebruiksvriendelijkheid te identificeren.

Op basis van de resultaten van deze tests en migratieproeven worden de prestaties, stabiliteit en kosten van de nieuwe oplossing vergeleken met die van het huidige CMS. Er worden concrete aanbevelingen geformuleerd die AllesOnline inzicht geven in de vraag of zij moeten investeren in de refactoring van het huidige CMS of kunnen overstappen naar een nieuw CMS-pakket. De bevindingen en aanbevelingen worden gedocumenteerd in een rapport, dat vervolgens aan de belanghebbenden binnen AllesOnline wordt gepresenteerd.

## ### Versiebeheer
Voor dit project wordt gebruikgemaakt van Git als versiebeheersysteem, waarbij elk project zijn eigen Git-historie bijhoudt. Dit zorgt voor een overzichtelijke en gestructureerde aanpak van de ontwikkeling, waarbij de voortgang en wijzigingen eenvoudig te traceren zijn.

#### Branchingstrategie
De volgende hoofdbranches gehanteerd:

- **Master Branch**: Deze branch vertegenwoordigt de meest recente stabiele versie van het project. Wijzigingen in deze branch zijn alleen toegestaan na uitgebreide testen en goedkeuring van nieuwe functies. Dit waarborgt dat de productieve code altijd betrouwbaar is en snel kan worden ingezet.
    
- **Staging Branch**: Deze branch fungeert als testomgeving voor gebruikersacceptatietests en kwaliteitscontrole. De staging branch is toegankelijk voor stakeholders en testers, wat de mogelijkheid biedt om nieuwe functionaliteiten te evalueren voordat ze in de productieomgeving worden geïmplementeerd. Indien er problemen worden ontdekt, kan deze branch eenvoudig worden verwijderd en opnieuw worden aangemaakt vanuit de master branch, waardoor een snelle rollback naar de laatste stabiele versie mogelijk is.
    
- **Development Branch**: Hier vindt de actieve ontwikkeling plaats. Nieuwe functies en verbeteringen worden toegevoegd of samengevoegd vanuit feature branches. De development branch is dynamisch en kan regelmatig worden bijgewerkt met nieuwe commits, wat een iteratieve ontwikkeling mogelijk maakt.
    
- **Feature Branches**: Voor elke nieuwe functie of wijziging wordt een aparte feature branch aangemaakt. Dit stelt de ontwikkelaar in staat om aan verschillende taken parallel te werken zonder invloed op de stabiele code in de master of staging branches. Na ontwikkeling en testen in de feature branch worden de wijzigingen samengevoegd met de development branch voor verdere integratie.
    

Deze gestructureerde aanpak van versiebeheer zorgt ervoor dat de ontwikkeling efficiënt en georganiseerd verloopt, en dat de kwaliteit van de code gewaarborgd blijft.

### CI/CD (Continuous Integration/Continuous Deployment)
Aangezien dit project zich richt op het vergelijken van verschillende CMS-pakketten, vindt er geen daadwerkelijke livegang van het project plaats. Desondanks is er wel het plan om geautomatiseerde CI/CD-pipelines op te zetten voor de development en het opzetten van staging omgevingen. Deze pipelines faciliteren een continue integratie en continue levering van de code, waarbij de codebase automatisch wordt geformatteerd en getest.

- **Automatische Testing**: Bij elke nieuwe commit in de development-, master- en staging-branch worden geautomatiseerde tests uitgevoerd. Dit omvat zowel unit tests als integratietests die controleren of de code de gewenste functionaliteit biedt. Dit vermindert het risico op regressies en vergroot de betrouwbaarheid van de software.
    
- **Deployment**: Zodra de code is gevalideerd via de CI-pipeline, wordt deze automatisch gedeployed naar de stagingomgeving. Dit zorgt voor een snellere feedbackloop en maakt het mogelijk om snel aanpassingen door te voeren op basis van gebruikersfeedback.
    

### Automatische Tests en Evaluaties
Om de functionaliteit en stabiliteit van de verschillende projecten te waarborgen, wil ik een reeks tests en evaluaties uitvoeren. Het is echter lastig om van tevoren te zeggen of alles wat hieronder wordt genoemd binnen de looptijd van het project haalbaar is. Toch noem ik een aantal tests en evaluaties die naar mijn mening nuttig kunnen zijn om te overwegen.

Houd er wel rekening mee dat ik gebruikmaak van bestaande CMS-pakketten. Daarom zullen de functionaliteiten van deze pakketten niet uitgebreid worden getest, omdat ik erop vertrouw dat de beheerders van deze pakketten dit al hebben gedaan.

1. **API tests (Automatisch)**
    - **Validatie van response-structuur**: Geautomatiseerde tests die controleren of API-responses de verwachte datatypes en structuren bevatten, door JSON-responses te valideren.
    - **Controle van statuscodes**: Verifiëren of API-aanroepen automatisch de juiste HTTP-statuscodes teruggeven (bijvoorbeeld 200 voor succesvolle verzoeken, 404 voor niet-gevonden resources).
2. **Frontend tests (Deels Automatisch)**
    - **Navigatietests (Handmatig/Automatisch)**: Automatische navigatietests kunnen uitvoeren of gebruikers zonder problemen tussen pagina's kunnen navigeren, hoewel handmatige controle nodig kan zijn voor visuele problemen.
    - **Formuliervalidatie (Automatisch)**: Geautomatiseerde tests die controleren of formulieren de juiste validatie toepassen en foutmeldingen correct afhandelen bij ongeldige invoer.
    - **Visuele regressietests (Handmatig/Automatisch)**: Tools kunnen geautomatiseerde visuele vergelijkingen uitvoeren, maar een handmatige controle is vaak nodig om subtiele of onverwachte UI-veranderingen te valideren.
3. **Unit tests (Automatisch)**
    - **Componenttests**: Automatische tests van frontend-componenten om te waarborgen dat ze correct werken en de juiste gegevens weergeven.
    - **Functietests**: Automatische tests van specifieke backend-functies, waarbij de uitvoer wordt gecontroleerd op basis van de invoer.
4. **Integratietests (Automatisch/Handmatig)**
    - **Migratietests (Handmatig/Automatisch)**: Migratieprocessen kunnen worden geautomatiseerd, maar de controle op dataverlies en integriteitsproblemen na de migratie vereist vaak handmatige verificatie. Automatische tests kunnen bijvoorbeeld controleren of belangrijke functies na migratie nog steeds werken.
5. **Performance benchmarking (Automatisch/Handmatig)**
    - **CMS-prestaties in basiscondities**: Automatische metingen van de basisprestaties van verschillende CMS-pakketten, zoals laadtijden en het verwerken van normale gebruikersinteracties. Een handmatige evaluatie kan nodig zijn om de gebruiksvriendelijkheid en functionaliteit in een reële situatie te bevestigen.
    - **Responstijdtests**: Automatische tests meten de snelheid van pagina’s en API-aanroepen, evenals de prestaties van caching en database-query's onder normale omstandigheden.

## Planning
#### Sprint 1 (week 3 t/m 4)
- **Doel:** Eerste analyse en ontwerp, bouwen van prototype webapplicaite met het huidige CMS.
- **Activiteiten:** 
	- Functionele analyse huidige CMS.
	- Ontwerpen van initiële prototype webapplicatie.
	- Implementatie en basis validatie.
- **Deliverables:** 
	- Initiële advies voortgang CMS.
	- Documentatie huidige CMS.
	- Prototype met huidige CMS.
### Sprint 2 t/m 7 (Week 5 t/m 14)
Per twee sprints het realiseren van een nieuw prototype en architectuur. Uitkomst is om aan het eind van het project meerdere prototypen te hebben waar AllesOnline aan kan zien welke opties beschikbaar zijn en wat de voor en nadelen van deze opties zijn.
#### Activiteiten voor iteratieve sprints
1. Analyse van het CMS-pakket.
2. Ontwikkeling van prototype.
3. Validatie aan de hand van testen.
4. Gebruikersfeedback.
#### Deliverables voor iteratieve sprints
 - Een werkend prototype van een CMS-pakket.
*  Documentatie en testresultaten.
 * Advies over het CMS-pakket in vergelijking met het huidige systeem.
### Sprint 8 (Week 15 t/m 16)
- **Doel:** Samenvoegen van deliverables en formuleren van eindadvies voor stakeholders.
- **Activiteiten:**
    - Analyse van alle prototypes en testresultaten.
    - Formuleren van een eindadvies op basis van de verzamelde data en feedback.
- **Deliverables:**
    - Eindadviesrapport voor stakeholders.
#### Week 17: Inleveren van portfolio

# Risicomanagement
Er zijn uiteraard enkele gevallen te bedenken waarin het niet mogelijk is om met het huidig voorgelegde planning door te werken. Hieronder een aantal risico en eventuele oplossingen. 

1. **Risico**: Vertragingen in de ontwikkeling
	- **Beschrijving**: Onvoorziene problemen tijdens de ontwikkeling van de prototypes kunnen leiden tot vertragingen.
	- **Impact**: Hoog
	- **Waarschijnlijkheid**: Gemiddeld
	- **Mitigatie**: Gebruik van Agile methodologie om flexibel om te gaan met veranderingen en vertragingen.    
     
2. **Risico**: Onvoldoende acceptatie van de nieuwe oplossing door gebruikers
	- **Beschrijving**: Eindgebruikers kunnen weerstand bieden tegen veranderingen of problemen ondervinden met de nieuwe CMS-oplossing.
	- **Impact**: Gemiddeld
	- **Waarschijnlijkheid**: Gemiddeld
	- **Mitigatie**: Betrekken van eindgebruikers in het test- en feedbackproces om hun behoeften en zorgen te begrijpen.
    
3. **Risico**: Technische onverenigbaarheid
	- **Beschrijving**: Technische incompatibiliteiten tussen het huidige CMS en de nieuwe oplossingen kunnen problemen veroorzaken.
	- **Impact**: Gemiddeld
	- **Waarschijnlijkheid**: Laag
	- **Mitigatie**: Voorafgaand onderzoek en evaluatie van de technische specificaties van nieuwe CMS-oplossingen.
    
4. **Risico**: Onvoldoende Technische Diepgang voor Eindexamen
	- **Beschrijving**: Het project kan mogelijk niet voldoen aan de vereiste technische diepgang die nodig is voor een succesvol eindexamen. Dit kan zich uiten in onvoldoende complexe technische oplossingen, beperkte innovatie, of een gebrek aan technische details die vereist zijn om een goede beoordeling te krijgen.
	- **Impact**: Hoog
	- **Waarschijnlijkheid**: Gemiddeld
	- **Mitigatie**: Identificeren van gebieden in het project waar extra technische diepgang kan worden toegevoegd, zoals complexere architectuur, diepgaande analyse, of innovatieve oplossingen.
# Persoonlijke leerdoelen
Tijdens dit project heb ik ook persoonlijke leerdoelen waarop ik me wil richten om mijn professionele vaardigheden verder te ontwikkelen. Hieronder worden de doelen beschreven die ik heb opgesteld in mijn Graduation Proposal.
### Tijdsmanagement en Prioritering
Uit eerdere projectfeedback blijkt dat mijn vaardigheden op het gebied van tijdmanagement en prioritering verbeterd kunnen worden. Dit heeft impact op het halen van deadlines en het effectiever maken van projectplanningen. Om dit te verbeteren, zal ik de volgende strategieën toepassen:

- **Opdelen van taken**: Grote taken splitsen in kleinere, beheersbare onderdelen om overzicht en controle te behouden.
- **Gebruik van productiviteitstools**: Tools inzetten om voortgang en deadlines bij te houden.
- **Reserveren van gefocuste werkuren**: Specifieke tijdsblokken inplannen voor geconcentreerd werk zonder onderbrekingen.

Met verbeterd tijdmanagement streef ik naar tijdige voltooiing van opdrachten en het minimaliseren van project vertragingen.
### Aanpassingsvermogen en Veerkracht
Om zowel persoonlijke als professionele uitdagingen effectiever aan te pakken, wil ik mijn aanpassingsvermogen en veerkracht verbeteren. Dit omvat:

- **Cultiveren van een positieve mindset**: Proactief leren van fouten en feedback, en veerkrachtig omgaan met tegenslagen.
- **Uitdagingen aangaan**: Activiteiten ondernemen die mijn comfort zone uitdagen en me helpen om flexibeler te reageren op veranderingen.
- **Zoeken naar begeleiding**: Regelmatig advies en ondersteuning zoeken van mentoren of counselors wanneer ik moeilijkheden ervaar.

Het versterken van deze vaardigheden zal me helpen om succesvoller te navigeren door uitdagingen en onzekerheden in zowel persoonlijke als professionele contexten.
### Communicatieve Vaardigheden
Ik wil mijn communicatieve vaardigheden verbeteren om effectiever te communiceren met teamleden, klanten en andere belanghebbenden. Dit omvat:

- **Heldere boodschappen**: Werken aan duidelijke en beknopte communicatie, zowel verbaal als schriftelijk.
- **Actief luisteren**: Meer aandacht besteden aan de behoeften en feedback van anderen om betere samenwerking te bevorderen.
- **Verbeteren van non-verbale communicatie**: Bewustzijn vergroten van lichaamstaal en andere non-verbale signalen om de communicatie te versterken.

Door deze vaardigheden te verbeteren, streef ik naar betere samenwerkingsverbanden, effectievere conflictoplossing en algehele verbetering van de communicatie binnen projecten.