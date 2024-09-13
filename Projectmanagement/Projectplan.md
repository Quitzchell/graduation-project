## Revitalising Content Management
***Versie 2***

## AllesOnline
AllesOnline is een full-service bureau dat zich richt op zowel online als offline communicatie. Het bedrijf bedient een diverse groep stakeholders, van grote internationale ondernemingen tot regionale servicegerichte instanties. De diensten die ze aanbieden variëren van productiegerelateerde werkzaamheden, zoals DTO en vormgeving, tot het ontwerpen en realiseren van full-stack webapplicaties.

## Content Management Systeem

Een belangrijk onderdeel van deze webapplicaties is het Content Management Systeem, ontwikkeld door AllesOnline zelf. Dit systeem, waarvan de eerste commit dateert van 13 februari 2015, kan met recht als legacy worden beschouwd. Hoewel er sinds de start van het project voornamelijk functionele uitbreidingen zijn doorgevoerd, ontbreekt het aan grondige refactoring of verbeteringen op het gebied van onderhoudbaarheid, zoals geautomatiseerde tests. Daardoor is het CMS nog wel uitbreidbaar, maar sommige modules zijn onnodig complex geworden, wat verdere ontwikkeling lastig en arbeidsintensief maakt.

De huidige staat van het CMS heeft verschillende oorzaken. Ten eerste levert de doorontwikkeling van het CMS geen directe inkomsten op voor AllesOnline, terwijl het onderhoud en de ontwikkeling ervan wel aanzienlijke kosten met zich meebrengen. Bovendien zijn sommige ontwikkelaars binnen het bedrijf terughoudend om de bestaande ‘spaghetticode’ te refactoren, voornamelijk vanwege het gebrek aan een vangnet zoals geautomatiseerde tests. Hierdoor is er geen garantie dat wijzigingen in het systeem voor één website geen problemen veroorzaken voor andere websites die hetzelfde CMS gebruiken.

Het is dus duidelijk dat het CMS, het fundament van de door AllesOnline aangeboden webapplicaties, dringend aandacht nodig heeft.

## Noodzaak voor sterker fundament

Het ontbreken van een sterk fundament in het CMS beperkt de schaalbaarheid en flexibiliteit, wat innovaties moeilijker maakt zonder concessies te doen aan de stabiliteit en betrouwbaarheid van het systeem. De verouderde codebase vormt een structureel probleem dat niet eenvoudig kan worden opgelost met tijdelijke aanpassingen. Een grondige refactor van het CMS, in combinatie met geautomatiseerde tests, zou de kwaliteit en het onderhoud verbeteren, en een solide basis leggen voor toekomstige innovaties. Dit zou het CMS in lijn brengen met moderne ontwikkelpraktijken en AllesOnline in staat stellen huidige en toekomstige uitdagingen met vertrouwen aan te gaan.

Echter, gezien de omvang van zo’n refactor is het wellicht verstandiger om alternatieve systemen te overwegen die duurzamer en kostenefficiënter zijn. Deze systemen zouden al voldoen aan moderne ontwikkelpraktijken en betere ondersteuning bieden voor toekomstige innovaties. Daarom is het belangrijk om te bepalen of verdere investeringen in het huidige systeem gerechtvaardigd zijn, of dat het slimmer is om te kijken naar duurzame alternatieven van derden.

## Aanpak voor modernisering van het CMS

Om binnen een realistische termijn tot een goed onderbouwd advies te komen, ben ik van plan om gedurende dit project verschillende casestudies uit te voeren. Mijn doel is om met een concreet advies te komen over hoe AllesOnline de architectuur rondom het gebruik van een CMS kan moderniseren. Het is hierbij essentieel om rekening te houden met het feit dat bestaande webapplicaties, die momenteel het huidige CMS gebruiken, moeten kunnen worden gemigreerd naar de nieuwe architectuur.

Om tot een goed afgewogen advies te komen, ben ik van plan een eenvoudige webapplicatie te ontwikkelen met het huidige CMS. Deze applicatie zal de belangrijkste functionaliteiten van het CMS bevatten. Op basis van deze applicatie zal ik onderzoeken hoe de migratie van het huidige CMS naar andere CMS-pakketten kan plaatsvinden, of hoe een refactor van het bestaande CMS kan worden uitgevoerd.

Om te controleren of de migratie correct verloopt, zal ik tests uitvoeren om te verifiëren of het CMS nog steeds het gewenste resultaat levert. Dit omvat het testen van de rendering van webpagina's om te waarborgen dat alle bestaande inhoud behouden blijft. Ook zal ik aandacht besteden aan de integriteit van de data en de werking van de bestaande functionaliteiten, om te garanderen dat de overgang naar de nieuwe architectuur soepel verloopt zonder verlies van functionaliteit of gegevens.

Door deze aanpak hoop ik niet alleen te bepalen hoe de huidige CMS-architectuur geoptimaliseerd kan worden, maar ook praktische richtlijnen te bieden voor een efficiënte en effectieve migratie of herstructurering.

### Fase 1: Onderzoek en initieel advies
In de eerste fase van het project wordt een analyse uitgevoerd van het bestaande Content Management Systeem (CMS) van AllesOnline. Deze fase richt zich op het vaststellen van de vereisten voor modernisering. Het doel is om een goed onderbouwd advies te formuleren over de beste aanpak: ofwel een grondige refactor van het bestaande CMS of migratie naar een nieuw systeem.

Het onderzoek begint met een functionele evaluatie van het huidige CMS. Hierbij worden de bestaande functionaliteiten onder de loep genomen om inzicht in het systeem te krijgen. Vervolgens wordt vastgelegd aan welke criteria een nieuw CMS moet voldoen, zoals schaalbaarheid, onderhoudbaarheid, kosten, en ondersteuning voor moderne ontwikkelpraktijken. Parallel aan deze evaluatie wordt marktonderzoek verricht naar beschikbare commerciële en open-source CMS-oplossingen die aan de gedefinieerde criteria voldoen. 

In deze fase worden ook de specificaties en functionaliteiten voor een prototypische webapplicatie, die de kernfunctionaliteiten van het huidige CMS bevat, worden vastgesteld.

### Fase 2: Iteratieve ontwikkelingscyclus
In de tweede fase wordt een iteratieve ontwikkelingscyclus gevolgd om de haalbaarheid van de modernisering of migratie te testen. Deze fase omvat de volgende stappen:


1. **Ontwikkeling**: De webapplicatie wordt ontwikkeld met het huidige CMS, waarbij de ontwikkelings- en testprocessen worden gedocumenteerd.  In vervolgsprints worden dit gerealiseerd met een anders CMS pakket
     
     
2. **Migratieplan ontwikkelen**: Een gedetailleerd migratieplan wordt opgesteld op basis van proefimplementaties van verschillende CMS-oplossingen. Dit plan bevat stappen voor de migratie van gegevens, integratie en testen.
     
     
3. **Uitvoering van migratieproeven**: Er worden migratieproeven uitgevoerd om te testen hoe goed de nieuwe systemen presteren bij het overzetten van bestaande gegevens en functionaliteiten.
     
     
4. **Functionele tests**: Functionele tests worden uitgevoerd om te controleren of de nieuwe of aangepaste CMS-oplossing alle gewenste functionaliteiten ondersteunt en correct functioneert.
     
     
5. **Prestatie- en stress tests**: De prestaties en belasting van de webapplicatie worden onder verschillende omstandigheden getest om te waarborgen dat het systeem aan de schaalbaarheidsvereisten voldoet.
     
     
6. **Gebruikerstest**: Eindgebruikers testen de nieuwe of gerefactoreerde CMS-oplossing om de gebruikerservaring te evalueren en eventuele problemen met de gebruiksvriendelijkheid te identificeren.
     
     

Op basis van de resultaten van deze tests en migratieproeven worden de prestaties, stabiliteit en kosten van de nieuwe oplossing vergeleken met die van het huidige CMS. Er worden concrete aanbevelingen geformuleerd over de vraag of AllesOnline moet investeren in de refactoring van het huidige CMS of moet overstappen naar een nieuw CMS. De bevindingen, aanbevelingen en het voorgestelde actieplan worden gedocumenteerd in een rapport, dat vervolgens aan de belanghebbenden binnen AllesOnline wordt gepresenteerd.

## Planning

#### Sprint 1 (week 3 t/m 4)
- **Doel:** Eerste analyse en ontwerp, bouwen van prototype webapplicaite met het huidige CMS.
- **Activiteiten:** 
	- Functionele analyse huidige CMS.
	- Ontwerpen van initiële prototype webapplicatie.
	- implementatie en basisvalidatie.
- **Deliverables:** 
	- Initiële advies voortgang CMS.
	- Documentatie huidige CMS.
	- Prototype met huidige CMS.
### Sprint 2 t/m 7 (Week 5 t/m 14)
Per twee sprints het realiseren van een nieuwe prototype en architectuur. Uitkomst is om aan het eind van het project meerdere prototypen te hebben waar AllesOnline aan kan zien welke opties beschikbaar zijn en wat de voor en nadelen van deze opties zijn.
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