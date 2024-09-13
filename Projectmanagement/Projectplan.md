## Revitalising Content Management
***Versie 1***

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

### Fase 1: Onderzoek
In de eerste fase van het project wordt een analyse uitgevoerd van het bestaande Content Management Systeem (CMS) van AllesOnline. Deze fase richt zich op het vaststellen van de vereisten voor modernisering. Het doel is om een goed onderbouwd advies te formuleren over de beste aanpak: ofwel een grondige refactor van het bestaande CMS of migratie naar een nieuw systeem.

Het onderzoek begint met een functionele evaluatie van het huidige CMS. Hierbij worden de bestaande functionaliteiten gedocumenteerd om inzicht in het systeem te krijgen. Vervolgens worden de criteria vastgesteld waaraan een nieuw CMS moet voldoen, zoals schaalbaarheid, onderhoudbaarheid, kosten, en ondersteuning voor moderne ontwikkelpraktijken. Parallel aan deze evaluatie wordt marktonderzoek verricht naar beschikbare commerciële en open-source CMS-oplossingen die aan de gedefinieerde criteria voldoen.Daarnaast worden de kosten van refactoring en migratie ingeschat. Dit omvat de tijd en middelen die nodig zijn voor de ontwikkeling van een nieuw CMS. 

De voordelen van elk scenario worden geëvalueerd, waarbij factoren zoals verbeterde prestaties, verhoogde stabiliteit, maar ook financiële aspecten worden meegenomen.

### Fase 2: Iteratieve ontwikkelingscyclus
In de tweede fase wordt een iteratieve ontwikkelingscyclus gevolgd om de haalbaarheid van de modernisering of migratie te testen. Deze fase omvat de volgende stappen:

1. **Specificaties opstellen**: De specificaties en functionaliteiten voor een prototypische webapplicatie, die de kernfunctionaliteiten van het huidige CMS bevat, worden vastgesteld.
     
     
1. **Ontwikkeling**: De webapplicatie wordt ontwikkeld met het huidige CMS, waarbij de ontwikkelings- en testprocessen worden gedocumenteerd.  
     
     
3. **Migratieplan ontwikkelen**: Een gedetailleerd migratieplan wordt opgesteld op basis van proefimplementaties van verschillende CMS-oplossingen. Dit plan bevat stappen voor de migratie van gegevens, integratie en testen.
     
     
4. **Uitvoering van migratieproeven**: Er worden migratieproeven uitgevoerd om te testen hoe goed de nieuwe systemen presteren bij het overzetten van bestaande gegevens en functionaliteiten.
     
     
5. **Functionele tests**: Functionele tests worden uitgevoerd om te controleren of de nieuwe of aangepaste CMS-oplossing alle gewenste functionaliteiten ondersteunt en correct functioneert.
     
     
6. **Prestatie- en stress tests**: De prestaties en belasting van de webapplicatie worden onder verschillende omstandigheden getest om te waarborgen dat het systeem aan de schaalbaarheidsvereisten voldoet.
     
     
7. **Gebruikerstest**: Eindgebruikers testen de nieuwe of gerefactoreerde CMS-oplossing om de gebruikerservaring te evalueren en eventuele problemen met de gebruiksvriendelijkheid te identificeren.
     
     

Op basis van de resultaten van deze tests en migratieproeven worden de prestaties, stabiliteit en kosten van de nieuwe oplossing vergeleken met die van het huidige CMS. Er worden concrete aanbevelingen geformuleerd over de vraag of AllesOnline moet investeren in de refactoring van het huidige CMS of moet overstappen naar een nieuw CMS. De bevindingen, aanbevelingen en het voorgestelde actieplan worden gedocumenteerd in een rapport, dat vervolgens aan de belanghebbenden binnen AllesOnline wordt gepresenteerd.

## Planning

### Voorbereiding en analyse (4 weken)
#### Week 3-4: Verkenning en evaluatie
- Verzamelen van gegevens en analyse van de huidige situatie.
- Documenteren van bestaande functionaliteiten en identificeren van beperkingen.
#### Week 5: Criteria en onderzoek
- Vaststellen van de eisen en criteria voor de nieuwe oplossing.
- Vergelijken van beschikbare opties en uitvoeren van marktonderzoek.
#### Week 6: Kosten en vergelijking
- Schatting van kosten en evalueren van de opties.
- Opstellen van een voorlopig rapport en verzamelen.
### Ontwikkeling en validatie (8 weken)
#### Week 7: Specificaties en ontwikkeling
- Definiëren van specificaties en ontwikkelen van prototypes of oplossingen.    
#### Week 8-9: Ontwikkeling
- Voortzetten van de ontwikkeling van de webapplicatie met het huidige CMS.
- Documenteren van de ontwikkelings- en testprocessen.
#### Week 10: Migratieplan en voorbereiding
- Opstellen van een gedetailleerd migratieplan voor verschillende CMS-oplossingen.
- Voorbereiden van de migratieproeven.
#### Week 11-12: Tests
- Uitvoeren van migratieproeven en functionele tests.
- Evalueren van prestaties en stabiliteit.
#### Week 13-15: Prestatie-, stress- en gebruikerstests
- Uitvoeren van prestatie- en stresstests om schaalbaarheid en stabiliteit te evalueren.
- Betrekken van eindgebruikers bij tests en verzamelen van feedback over de gebruiksvriendelijkheid.
- Oplossen van eventuele problemen op basis van feedback.
### Documentatie en presentatie (3 weken)
#### Week 16: Rapportage
- Documenteren van bevindingen, aanbevelingen en voorgestelde actieplan in een gedetailleerd rapport.
#### Week 17: Voorbereiding presentatie
- Inleveren van portfolio