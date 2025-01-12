# **Leeswijzer**

> * [Evidence in verhouding tot leeruitkomsten](../Bijlagen/EvidenceInVerhoudingTotLeeruitkomsten.md)

# Inleiding

Dit verslag beschrijft het verloop van het project _'Revitalising Content Management'_, waarin wordt onderzocht hoe AllesOnline, een full-service bureau voor online en offline communicatie, zijn zelfontwikkelde Content Management Systeem (CMS) kan moderniseren. Het CMS, dat sinds 2015 operationeel is en nog altijd in gebruik is, vormt de basis van de webapplicaties die het bedrijf ontwikkelt. Door een gebrek aan aandacht en onderhoud is het systeem verouderd geraakt en onderhoudsintensief geworden.

Het onderzoek richt zich op het formuleren van een advies over twee mogelijke strategieën voor het CMS: het doorontwikkelen van het huidige systeem of het overstappen naar een systeem van een externe partij. In het geval van de laatste strategie maakt de mogelijkheid om bestaande projecten met het AllesOnline CMS naar het nieuwe systeem te migreren deel uit van het onderzoek.

## Onderzoeksvraag

Hoe kan AllesOnline een bestaand Content Management Systeem inzetten om de schaalbaarheid, onderhoudbaarheid en toekomstbestendigheid van haar webapplicaties te verbeteren?

### Deelvragen

1. **Wat zijn de belangrijkste technische en functionele beperkingen van het huidige CMS van AllesOnline?**
    
2. **Aan welke criteria moet een gemoderniseerd CMS voldoen om de huidige en toekomstige behoeften van AllesOnline te ondersteunen?**
    
3. **Welke commerciële en open-source CMS-oplossingen voldoen aan de vereisten voor modernisering en kunnen een haalbare vervanging bieden voor het huidige systeem?**
    
4. **Hoe verloopt de migratie van de bestaande webapplicaties naar een nieuw CMS, en welke technische uitdagingen komen hierbij kijken?**
    
5. **Welke prestatieverschillen en kosten zijn er tussen het huidige CMS en een nieuw systeem?**
     

# Onderzoek naar huidige CMS en opzet eerste prototype

In de eerste weken van dit project heb ik me gericht op het analyseren van het AllesOnline CMS. Om een goed beeld te krijgen en een onderbouwd advies te kunnen formuleren, heb ik verschillende acties ondernomen. Zo ben ik in gesprek gegaan met andere developers binnen AllesOnline, heb ik de codebase van het CMS geanalyseerd en heb ik mijn ervaringen van ongeveer 2 jaar werken bij AllesOnline meegenomen.

Uit de gesprekken met developers bleek dat zij over het algemeen tevreden zijn met het CMS. Hoewel niet alles volgens *Best Practices* verloopt en er soms onduidelijkheden bestaan over de werking van het CMS, slagen zij er meestal in om op een pragmatische manier tot oplossingen te komen. Er werd ook aangegeven dat zij een verbeterde developer experience zouden waarderen. Hierbij kan gedacht worden aan meer abstractie van functionaliteiten, zodat complexiteit beter kan worden verborgen en herbruikbare oplossingen eenvoudiger te implementeren zijn. Dit zou in de toekomst kunnen bijdragen aan het sneller en eenvoudiger realiseren van specifieke klantwensen.

> * [Gesprekken met Developers](../AnalyseAdvies/GesprekkenEnErvaringenMetDevelopers.md)
> * [Onderzoek naar het AllesOnline CMS](../AnalyseAdvies/OnderzoekNaarHetAOCms.md)
> * [SWOT: AllesOnline CMS](../AnalyseAdvies/SwotAOCms.md)

Parallel aan de analyse heb ik requirements opgesteld waaraan een CMS moet voldoen om te voorzien in de behoeften van een typische AllesOnline-website. Op basis van deze requirements heb ik een **Multi-Criteria Decision Making**-analyse in **DOT**-frameworkstijl opgesteld, waarmee ik eenvoudig kan bijhouden hoe de frameworks zich tot de gestelde requirements verhouden.

> * [Requirements](../AnalyseAdvies/Requirements.md)
> * [Checklist voor CMS criteria](../AnalyseAdvies/ChecklistVoorCMSCriteria)

Op basis van de requirements en mijn bevindingen over een typische AllesOnline-website ben ik begonnen met het ontwikkelen van een prototype. Aanvankelijk zou dit een stamboomapplicatie worden, gebaseerd op de familie Bonaparte. Toen echter de incestueuze geschiedenis van de Europese koningshuizen naar voren kwam en de complexiteit van het verwerken daarvan duidelijk werd, besloot ik in plaats daarvan om een eenvoudige blog-website te maken, geschreven vanuit het perspectief van Napoleon Bonaparte.

> _Meer over waarom ik van plan ben met prototypen te werken en hoe deze eruit gaan zien lees je in het onderstaande document._ 
> * [Opzet van de prototypes](../DesignRealisatie/OpzetVanDePrototypes.md)


> _Repositories van de frontend en backend met het AllesOnline CMS zijn te vinden via onderstaande links._
> * [Repository: Frontend Prototype](https://github.com/Quitzchell/graduation-frontend)
> * [Repository: Backend AO CMS](https://github.com/Quitzchell/graduation-ao-cms/)

Om ervoor te zorgen dat mijn collega's de prototypes op hun eigen systeem kunnen draaien, heb ik voor alle prototypes **Docker**-containers voorbereid. Voor de backends gebruik ik de AllesOnline-container, met een uitgebreiding om **SQLite** te ondersteunen. Dit maakt het mogelijk om E2E-tests uit te voeren, zowel tijdens het programmeren als binnen een CI-pipeline, zonder de reguliere database te beïnvloeden. Voor de frontend wordt gebruik gemaakt van een standaard **Node.js**-container.

### Conclusies onderzoek AllesOnline CMS en realisatie eerste prototype

Dankzij bovenstaand onderdeel van het onderzoek en de ontwikkeling van het prototype kan ik vanuit verschillende invalshoeken al antwoorden geven op enkele deelvragen.
 
* __Wat zijn de belangrijkste technische en functionele beperkingen van het huidige CMS?__
	* **Beperkte documentatie:** De documentatie is verouderd, waardoor ontwikkelaars onnodig lang bezig zijn met het begrijpen van bepaalde functionaliteiten en parameters.
	* **Complexe codestructuur:** Veel modules hebben te veel verantwoordelijkheden, wat onderhoud en uitbreiding bemoeilijkt.
	* **Sterke afhankelijkheid:** Modules en functionaliteiten zijn te afhankelijk van elkaar, waardoor wijzigingen in één module andere modules kunnen beïnvloeden.
	* **Niet-naleven van SOLID-principes:** Het systeem voldoet niet aan de SOLID-principes, wat leidt tot hogere complexiteit en lagere testbaarheid en onderhoudbaarheid.

* __Aan welke criteria moet een gemoderniseerd CMS voldoen?__
	* **Uitgebreide documentatie:** Het CMS beschikt over actuele documentatie die modules en functionaliteiten helder beschrijft.
	* **Modulariteit:** Het systeem moet bestaan uit goed afgebakende modules met specifieke verantwoordelijkheden, waarbij deze modules loosely-coupled zijn en onafhankelijk kunnen functioneren. Dit maakt het systeem gemakkelijker uit te breiden, aan te passen en te onderhouden.

* __Wat zijn de prestatieverschillen en kosten tussen het huidige CMS en een nieuw systeem?__
	* **Prestatie:** Het gebrek aan modulariteit en documentatie resulteert in hogere onderhoudskosten en meer bugs, wat de prestaties en stabiliteit beïnvloedt.
	* **Kosten:** De investeringen in het opschonen en onderhouden van een eigen systeem moet goed worden afgewogen tegen de voordelen van een al bestaande oplossing, die doorgaans lagere onderhoudskosten met zich meebrengt.

# Frontend tests met Cypress 

Nadat het prototype met het AllesOnline CMS was afgerond, ben ik begonnen met het ontwikkelen van een testsuite in **Cypress**. Deze is bedoeld om de communicatie tussen de backend en frontend te valideren, zodat ik kan controleren of de frontend de juiste informatie van de verschillende backend-prototypen ontvangt.

> * [Cypress test-suite in frontend repository](https://github.com/Quitzchell/graduation-frontend/tree/main/src/cypress)

-- oud 

Omdat Cypress niet in een Linux Alpine-container kan draaien, heb ik het mogelijk gemaakt dat Cypress vanuit zijn eigen container draait en kan communiceren met de frontend-applicatie. Het voordeel hiervan is dat de container voor de frontend niet onnodig groot wordt door de toevoeging en extra benodigdheden voor Cypress. Het nadeel is echter dat er een timeout tussen de requests nodig is, omdat de frontend een exception teruggeeft doordat het te snel opvolgende verzoeken van hetzelfde adres ontvangt.

-- nieuw 

Omdat Cypress niet in een Linux Alpine-container kan draaien, heb ik ervoor gezorgd dat Cypress in zijn eigen container draait en kan communiceren met de frontend-applicatie. Dit voorkomt dat de frontend-container onnodig groot wordt door de toevoeging van Cypress en zijn benodigdheden. Het nadeel is echter dat een timeout tussen de requests nodig is, omdat de frontend een exception teruggeeft bij te snel opvolgende verzoeken van hetzelfde adres.

> _Hieronder een kleine afwijking van het chronologische verhaal, met een aantal runs van de Cypress-testsuite waarin de verschillende CMS-oplossingen aan de frontend zijn gekoppeld._
> * [Video: Cypress tests met AllesOnline CMS](../Bijlagen/CypressTestsAOCms.md)
> * [Video: Cypress tests met Filament CMS](../Bijlagen/CypressTestsFilamentCms.md)
> * [Video: Cypress tests met Statamic CMS: Flat-file](../Bijlagen/CypressTestsStatamicFlatFileCms.md)
> * [Video: Cypress tests met Statamic CMS: Eloquent-driver](../Bijlagen/CypressTestsStatamicEloquentDriverCms.md)
> * [Video: Cypress tests met Statami CMS: Runway-addon](../Bijlagen/CypressTestsStatamicRunwayCms.md)

# Onderzoek naar CMS met Filament

Het tweede prototype dat ik ben gaan realiseren, was een CMS met Filament. Filament is niet perse een CMS, maar eerder een library met _"a collection of beautiful full-stack components"_, althans, als we hun eigen website moeten geloven. Met de componenten die het biedt, is het relatief eenvoudig om een CRUD-applicatie op te zetten en met een beetje creativiteit ook een CMS.

> * [Onderzoek naar Filament](../AnalyseAdvies/OnderzoekNaarFilament.md)
> * [SWOT: Filament](../AnalyseAdvies/SwotFilamentCms.md)

Tijdens het onderzoek ben ik ook aan de slag gegaan met het ontwerpen en realiseren van het CMS gebaseerd op Filament. Voor het ontwerp van de CMS-functionaliteiten heb ik een iteratieve aanpak gehanteerd. Deze werkwijze stelde me in staat om tijdens elke ontwikkelstap te reflecteren op wat ik had gerealiseerd en wat de volgende logische stap zou zijn. Daarnaast ben ik begonnen met het documenteren van de werking van de verschillende systemen.

> * [Design voor CMS functionaliteit in Filament](../Bijlagen/UmlEntiteitenDiagramContentManagementFilament.md)
> * [Repository: Backend Filament CMS](https://github.com/Quitzchell/graduation-filament-cms)
> * [Technische documentatie van CMS Prototypes](../DesignRealisatie/TechnischeDocumentatieCmsPrototypes.md)

### Conclusies onderzoek Filament en realisatie Filament CMS

* **Welke commerciële en open-source CMS-oplossingen voldoen aan de vereisten voor modernisering en kunnen een haalbare vervanging bieden voor het huidige systeem?**
	- Omdat Filament flexibel en modulair te gebruiken is, is het zeer geschikt als basis voor het realiseren van een CMS die gebruikt kan worden voor AllesOnline websites.

* **Wat zijn de prestatieverschillen en kosten tussen het huidige CMS en een nieuw systeem?**
	* Met Filament is het eenvoudiger om een CRM of andere SaaS-oplossing voor klanten te realiseren. Dit is aanzienlijk eenvoudiger dan het AllesOnline CMS in bochten te wringen om te voldoen aan vereisten waarvoor het praktisch gezien niet is ontworpen.
	* Filament is open-source en wordt aangeboden onder de MIT-licentie. Dit betekent dat er geen directe licentiekosten zijn verbonden aan het gebruik ervan.
	* Filament heeft relatief uitgebreide documentatie en een snel groeiende community. Dit maakt het eenvoudiger voor ontwikkelaars om ondersteuning en voorbeelden te vinden.
	* Omdat Filament nog relatief nieuw is, zijn er nog wel enkele kinderziektes in het geval van edgecases.
	* Filament is gebaseerd op Laravel, een framework waarmee het huidige development-team van AllesOnline al goed bekend is.

# Onderzoek migratie van AllesOnline naar Filament CMS

Nadat ik zowel een AllesOnline-prototype als een Filament CMS-prototype had gerealiseerd, kon ik aan de slag met een belangrijke requirement van AllesOnline: onderzoeken of het mogelijk is om bestaande projecten met het AllesOnline CMS op een geautomatiseerde manier te migreren naar een nieuw systeem.

Om tijd te besparen, heb ik besloten niet het volledige proces uit te werken, maar een Proof of Concept (PoC) te realiseren. Het doel van de PoC is om aan te tonen dat de kern van het AllesOnline CMS, namelijk de XML-schema's omgezet kunnen worden naar de schema's die in Filament worden gebruikt. Voor het realiseren van deze tool heb ik gekozen om gebruik te maken van Symfony's Console-componenten.

> _Verantwoording voor de keuze om Symfony voor de tool gebruiken lees je hier_:
> * [Onderzoek voor Tool CMS Migratie](../AnalyseAdvies/OnderzoekVoorCmsMigratie.md)

> Proof of Concept: waarin een XML-block omgezet kan worden naar een PHP-class die gebruikt kan worden door het Filament CMS. 
> * [Repository PoC: Transcription (migratietool)](https://github.com/Quitzchell/poc-transcription)

### Conclusies onderzoek migratie AllesOnline CMS naar Filament CMS

* **Hoe verloopt de migratie van de bestaande webapplicaties naar een nieuw CMS, en welke technische uitdagingen komen hierbij kijken?**
	* De migratie tussen systemen kan plaatsvinden via een op maat gemaakte CLI-tool, bijvoorbeeld met Symfony. Deze tool kan bestanden uitlezen en op basis van regels nieuwe bestanden genereren voor het nieuwe CMS.
	* De grootste uitdagingen zijn vooral te vinden in de verschillende architecturen en functionaliteiten tussen het originele en de nieuwe systemen. Dit betekend dat dat het belangrijk is dat er een goede planning en zorgvuldig vooronderzoek gedaan wordt voordat de migratie wordt uitgevoerd. 
	
# Onderzoek naar Statamic CMS

Na het afronden van de eerste Proof of Concept (PoC) voor de migratietool, ben ik begonnen met het opzetten van een prototype met Statamic. Dit is een commercieel CMS waarvoor een licentie vereist is bij commercieel gebruik. In tegenstelling tot Filament is Statamic specifiek ontworpen om als CMS te fungeren.

> * [Onderzoek naar Statamic](../AnalyseAdvies/OnderzoekNaarStatamicCMS.md)
> * [SWOT: Statamic](../AnalyseAdvies/SwotStatamicCms.md)

Ook tijdens dit onderzoek ben ik aan de slag gegaan met het ontwerpen en realiseren van het prototype met Statamic. Omdat Statamic in meerdere verschillende configuraties kan worden gebruikt, heb ik ook meerdere designs gerealiseerd. Deze designs zijn vrij strikt gebonden aan de specifieke configuraties, terwijl het ook mogelijk is om verschillende configuraties te combineren.

// todo: add repos
* Design voor Statamic CMS met flat-file //todo
* Design voor Statamic met eloquent-driver //todo
* Design voor Statamic met Runway-addon //todo
* [Technische documentatie van CMS prototypes](../DesignRealisatie/TechnischeDocumentatieCmsPrototypes.md)

### Conclusies onderzoek naar Statamic CMS

* **Welke commerciële en open-source CMS-oplossingen voldoen aan de vereisten voor modernisering en kunnen een haalbare vervanging bieden voor het huidige systeem?**
	- De flat-file en eloquent-driver configuraties van Statamic zijn snel op te zetten, maar kunnen tekort komen bij projecten waar meer complexiteit verwacht wordt.
	- De flat-file en eloquent-driver configuraties zijn, ondanks tekortkomingen, uiterst geschikt voor simpele marketing- en whitelabelwebsites.
	- Het opzetten van de Runway configuratie kost meer tijd en kan ook bij complexere projecten tekort schieten.
	- Hoewel Statamic een breed scala aan functionaliteiten biedt, zullen voor sommige oplossingen die in het AllesOnline CMS zijn ingebouwd, aangepaste componenten toegevoegd moeten worden.

* **Wat zijn de prestatieverschillen en kosten tussen het huidige CMS en een nieuw systeem?**
	* Statamic biedt verschillende licenties voor het bouwen van websites:
		* **Statamic Core**: Gratis voor persoonlijke projecten.
		* **Statamic Pro**: $259 per jaar per website voor professionele toepassingen.
		* **Master License**: $1250 per jaar voor organisaties met vijf websites, inclusief kortingen op extra licenties.
		* **Platform Subscription**: Abonnement vanaf $175 per maand voor de eerste 25 websites, met lagere kosten per website naarmate het aantal stijgt.
	* Voor AllesOnline is minimaal **Statamic Pro** nodig. Voor meerdere websites is de **Master License** voordelig tot 10 websites, daarna is het voordeliger om over te schakelen naar het abonnementsmodel.
	* Omdat er bij de **Master License** voor de eerste 25 websites een vast maandelijks tarief van $175 geldt, komen de jaarlijkse kosten uit op $2100.

# Terugkoppeling migratie naar nieuw CMS

Omdat uit de analyse van Statamic bleek dat een migratie van het AllesOnline CMS naar Statamic niet wenselijk is, heb ik besloten geen Proof of Concept (PoC) hiervoor op te zetten. In plaats daarvan heb ik een aanvullend adviesdocument geschreven over de migratie van het AllesOnline CMS naar Filament.

> * [Advies CMS migratie](../AnalyseAdvies/AdviesCMSMigratie.md)

### Conclusies over migratie naar nieuw CMS

* **Hoe verloopt de migratie van de bestaande webapplicaties naar een nieuw CMS, en welke technische uitdagingen komen hierbij kijken?**
	* De projecten binnen AllesOnline variëren sterk in opzet, zoals monolitische projecten, quasi-monolitische projecten en projecten met een decoupled architecture. Elk van deze benaderingen heeft verschillende technieken en systemen voor de verwerking van informatie binnen het CMS. Dit zorgt ervoor dat per project zorgvuldig onderzocht moet worden welke technieken gebruikt zijn en hoe deze overgezet kunnen worden naar het nieuwe CMS.
	* In het **AllesOnline CMS** worden validatieregels voor invoervelden gedefinieerd in de Eloquent-modellen, terwijl in **Filament** deze regels aan de `Form`-componenten worden gekoppeld via schema’s. Het migreren van deze validatieregels is complex omdat de migratietool bestanden één voor één overzet, maar de validatieregels en schema’s nu uit verschillende bestanden moeten worden samengevoegd. Dit maakt het ontwikkelen van de migratietool uitdagender.
	* Er wordt aangeraden om de migratie in kleinere projecten te beginnen, zodat een iteratief proces kan plaatsvinden. Hierdoor wordt het fundament van de migratietool stapsgewijs gebouwd en verbeterd.
	* Streef bij migraties naar het creëren van meer uniformiteit in de architectuur. Dit kan onder andere door gebruik te maken van abstracties en is nuttig wanneer projecten in de toekomst geüpdatet moeten worden.

# Advies en conclusie

Op basis van het onderzoek naar het huidige CMS van AllesOnline en de mogelijke alternatieven, heb ik een advies document geschreven en kan ik het volgende antwoord op de onderzoeksvraag formuleren.

> * [Adviesdocument](../AnalyseAdvies/AdviesDocument.md)

### Hoe kan AllesOnline het huidige CMS verbeteren of vervangen door een extern systeem zoals Filament of Statamic?

AllesOnline zou het huidige CMS niet verder moeten ontwikkelen, aangezien het onvoldoende onderhouden wordt en daardoor verouderd is. In plaats daarvan wordt aanbevolen over te stappen op een extern systeem. Beide onderzochte systemen, **Filament** en **Statamic**, bieden aanzienlijke voordelen die kunnen bijdragen aan een efficiëntere en toekomstbestendige bedrijfsvoering. **Filament** is met name geschikt voor projecten die complexere, op maat gemaakte webapplicaties vereisen, terwijl **Statamic** een uitstekende keuze is voor marketingwebsites en meer gestandaardiseerde klantbehoeften.

Naast de conclusie dat **Filament** de beste keuze is voor complexere projecten, heeft het vanwege de compatibiliteit met het huidige AllesOnline CMS, ook de voorkeur voor het migreren van bestaande AllesOnline-projecten.

# Reflectie 

> * [Reflectie]()