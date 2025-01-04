
> * [Evidence in verhouding tot leeruitkomsten](../Bijlagen/EvidenceInVerhoudingTotLeeruitkomsten.md)

# Inleiding

Dit verslag beschrijft het verloop van het project _'Revitalising Content Management'_, waarin wordt onderzocht hoe AllesOnline - een full-service bureau voor online en offline communicatie - haar huidige intern ontwikkelde Content Management Systeem (CMS) kan moderniseren. Het CMS, dat momenteel in gebruik is en sinds 2015 operationeel is, vormt de basis van de webapplicaties die het bedrijf ontwikkelt. Door een gebrek aan aandacht en onderhoud is het systeem echter verouderd. Dit maakt het moeilijk uit te breiden, wat het werken ermee steeds minder efficiënt maakt en zowel de schaalbaarheid als de toekomstbestendigheid belemmert.

Concreet richt het onderzoek zich op het formuleren van een advies over twee mogelijke strategieën voor het CMS: het doorontwikkelen van het huidige systeem of het overstappen naar een door een derde partij ontwikkeld CMS. In het geval van de laatste strategie hoort daar ook de mogelijkheid bij om het CMS van de bestaande websites naar het nieuwe systeem te migreren.

## Onderzoeksvraag

Hoe kan AllesOnline een bestaand Content Management Systeem inzetten om de schaalbaarheid, onderhoudbaarheid en toekomstbestendigheid van haar webapplicaties te verbeteren?

### Deelvragen

1. **Wat zijn de belangrijkste technische en functionele beperkingen van het huidige CMS van AllesOnline?**
    
2. **Aan welke criteria moet een gemoderniseerd CMS voldoen om de huidige en toekomstige behoeften van AllesOnline te ondersteunen?**
    
3. **Welke commerciële en open-source CMS-oplossingen voldoen aan de vereisten voor modernisering en kunnen een haalbare vervanging bieden voor het huidige systeem?**
    
4. **Hoe verloopt de migratie van de bestaande webapplicaties naar een nieuw CMS, en welke technische uitdagingen komen hierbij kijken?**
    
5. **Welke prestatieverschillen en kosten zijn er tussen het huidige CMS en een nieuw systeem?**
     

# Onderzoek naar huidige CMS en opzet eerste Prototype

In de eerste weken van dit project heb ik mij gericht op het analyseren van het AllesOnline CMS. Om een goed beeld te krijgen en hier gericht advies over uit te kunnen brengen, heb ik gesprekken gevoerd met mijn collega-developers van AllesOnline over hun ervaringen en de uitdagingen die zij tegenkomen bij het werken met het CMS. Daarnaast heb ik mijn eigen ervaringen met het CMS meegenomen in deze analyse en ben ik de code van het CMS ingedoken om op een dieper niveau te begrijpen hoe het systeem is opgebouwd.

Uit de gesprekken en de analyse bleek dat de meeste developers tevreden zijn met het CMS. Hoewel niet alles volgens Best Practices verloopt en er onduidelijkheden bestaan over de werking van het CMS, slagen de meeste developers erin om op een pragmatische manier tot oplossingen te komen. Toch valt op dat de developers binnen AllesOnline graag een verbeterde developer experience zouden zien. Hierbij kan gedacht worden aan meer abstractie van functionaliteiten, waardoor complexiteit beter kan worden verborgen en herbruikbare oplossingen eenvoudiger te implementeren zijn. Dit kan bijdragen aan het sneller en eenvoudiger realiseren van specifieke klantwensen.

> * [Gesprekken met Developers](../AnalyseAdvies/GesprekkenEnErvaringenMetDevelopers.md)
> * [Onderzoek naar het AllesOnline CMS](../AnalyseAdvies/OnderzoekNaarHetAOCms.md)
> * [SWOT: AllesOnline CMS](../AnalyseAdvies/SwotAOCms.md)

Parallel aan het onderzoek heb ik requirements opgesteld voor wat het CMS moet bieden om te voldoen aan de eisen van een doorsnee AllesOnline-website. Op basis van deze requirements heb ik een DOT-frameworkesque `Multi-Criteria Decision Making`-analyse opgesteld, waarmee ik op een eenvoudige manier kan bijhouden hoe de te onderzoeken frameworks aan deze requirements voldoen.

> * [Requirements](../AnalyseAdvies/Requirements.md)
> * [Checklist voor CMS criteria](../AnalyseAdvies/ChecklistVoorCMSCriteria)

Aan de hand van de requirements en een duidelijk beeld van wat er van een doorsnee AllesOnline-website verwacht wordt, ben ik aan de slag gegaan met het realiseren van een prototype van een website. In eerste instantie was het de bedoeling om een stamboom applicatie te ontwikkelen op basis van de familie Bonaparte. Toen echter de incestueuze geschiedenis van de Europese koningshuizen naar voren kwam – en de complexiteit van het verwerken hiervan in een systeem duidelijk werd – heb ik ervoor gekozen om in plaats daarvan een eenvoudige blog-website te realiseren, geschreven vanuit het perspectief van Napoleon Bonaparte.

> _Meer over waarom ik van plan ben met prototypen te werken en hoe deze eruit gaan zien lees je in het onderstaande document._ 
> * [Opzet van de prototypes](../DesignRealisatie/OpzetVanDePrototypes.md)


> _Repositories van de frontend en backend met het AllesOnline CMS zijn te vinden via onderstaande links._
> * [Repository: Frontend Prototype](https://github.com/Quitzchell/graduation-frontend)
> * [Repository: Backend AO CMS](https://github.com/Quitzchell/graduation-ao-cms/)

Om ervoor te zorgen dat mijn collega's de prototypes op hun eigen systeem kunnen draaien, heb voor alle prototypes Docker-containers voorbereid. Voor de backends maak ik gebruik van de voorgedefinieerde AllesOnline-container, met een uitbreiding die het mogelijk maakt om van SQLite gebruik te maken. Dit maakt het eenvoudiger om zowel tijdens het programmeren als binnen een pipeline feature tests uit te voeren, zonder de reguliere database te beïnvloeden.

### Conclusies onderzoek AllesOnline CMS en realisatie eerste Prototype

Tijdens dit onderzoek en de ontwikkeling van het prototype kan ik vanuit verschillende invalshoeken al antwoorden kunnen formuleren op enkele deelvragen.
 
* __Wat zijn de belangrijkste technische en functionele beperkingen van het huidige CMS?__
	* **Beperkte documentatie:** De documentatie is verouderd, waardoor ontwikkelaars onnodig lang bezig zijn met het begrijpen van bepaalde functionaliteiten en parameters.
	* **Complexe codestructuur:** Veel modules hebben te veel verantwoordelijkheden, wat onderhoud en uitbreiding bemoeilijkt.
	* **Sterke afhankelijkheid:** Modules en functionaliteiten zijn te afhankelijk van elkaar, waardoor wijzigingen in één module andere modules kunnen beïnvloeden.
	* **Niet-naleven van SOLID-principes:** Het systeem voldoet niet aan de SOLID-principes, wat leidt tot hogere complexiteit en lagere testbaarheid en onderhoudbaarheid.

* __Aan welke criteria moet een gemoderniseerd CMS voldoen?__
	* **Uitgebreide documentatie:** Het CMS beschikt over actuele documentatie die modules en functionaliteiten helder beschrijft.
	* **Modulariteit:** Het systeem moet opgesplitst zijn in kleinere, onafhankelijke modules met specifieke verantwoordelijkheden zodat deze gemakkelijk uitgebreid kan worden indien nodig.

* __Wat zijn de prestatieverschillen en kosten tussen het huidige CMS en een nieuw systeem?__
	* **Prestatie:** Het gebrek aan modulariteit en documentatie resulteert in hogere onderhoudskosten en meer bugs, wat de prestaties en stabiliteit beïnvloedt.
	* **Kosten:** De investeringen in het opschonen en onderhouden van een eigen systeem moet goed worden afgewogen tegen de voordelen van een al bestaande oplossing, die doorgaans lagere onderhoudskosten met zich meebrengt.

# Frontend tests met Cypress 

Rond het afronden van het prototype met het AllesOnline CMS ben ik aan de slag gegaan met het realiseren van een testsuite met Cypress. Deze is bedoeld om de communicatie tussen de backend en de frontend te valideren. Aan de hand van deze test kan ik controleren of de website vanuit de verschillende CMS-oplossingen de correcte informatie naar de frontend doorgeeft.

> * [Cypress test-suite in frontend repository](https://github.com/Quitzchell/graduation-frontend/tree/main/src/cypress)

Omdat Cypress niet in een Linux Alpine-container kan draaien, heb ik het mogelijk gemaakt dat Cypress vanuit zijn eigen container draait en kan communiceren met de frontend-applicatie. Het voordeel hiervan is dat de container voor de frontend niet onnodig groot wordt door de toevoeging en extra benodigdheden voor Cypress. Het nadeel is echter dat er een timeout tussen de requests nodig is, omdat de frontend een exception teruggeeft doordat het te snel opvolgende verzoeken van hetzelfde adres ontvangt.

> _Hieronder een kleine afwijking van het chronologische verhaal, met een aantal runs van de Cypress-testsuite waarin de verschillende CMS-oplossingen aan de frontend zijn gekoppeld._
> * [Video: Cypress tests met AllesOnline CMS](../Bijlagen/CypressTestsAOCms.md)
> * [Video: Cypress tests met Filament CMS](../Bijlagen/CypressTestsFilamentCms.md)

# Onderzoek naar CMS met Filament

Het tweede prototype dat ik ben gaan realiseren, was een CMS met Filament. Filament is niet per se een CMS-pakket, maar eerder een library met _"a collection of beautiful full-stack components"_, althans, als we hun eigen website moeten geloven. Met de componenten die het biedt, is het relatief eenvoudig om een CRUD-applicatie op te zetten en met een beetje creativiteit ook een CMS.

> * [Onderzoek naar Filament](../AnalyseAdvies/OnderzoekNaarFilament.md)
> * [SWOT: Filament](../AnalyseAdvies/SwotFilamentCms.md)

Na het onderzoek ben ik aan de slag gegaan met het ontwerpen en realiseren van een CMS gebaseerd op Filament. Voor het ontwerp van de CMS-functionaliteiten heb ik een iteratieve aanpak gehanteerd. Deze werkwijze stelde me in staat om tijdens elke ontwikkelstap te reflecteren op wat ik had gerealiseerd en wat de volgende logische stap zou zijn. Daarnaast ben ik begonnen met het documenteren van de werking van de verschillende systemen.

> * [Design voor CMS functionaliteit in Filament](../Bijlagen/UmlEntiteitenDiagramContentManagementFilament.md)
> * [Repository: Backend Filament CMS](https://github.com/Quitzchell/graduation-filament-cms)
> * [Technische documentatie van CMS Prototypes](../DesignRealisatie/TechnischeDocumentatieCmsPrototypes.md)

### Conclusies onderzoek Filament en realisatie Filament CMS

* **Welke commerciële en open-source CMS-oplossingen voldoen aan de vereisten voor modernisering en kunnen een haalbare vervanging bieden voor het huidige systeem?**
	- Omdat Filament flexibel en modulair is te gebruiken, is het zeer geschikt als basis voor het realiseren van een CMS die gebruikt kan worden voor AllesOnline websites.

* **Wat zijn de prestatieverschillen en kosten tussen het huidige CMS en een nieuw systeem?**
	* Met Filament is het eenvoudiger om een CRM of andere SaaS-oplossing voor klanten te realiseren. Hierdoor wordt het mogelijk om naast een CMS ook aanvullende oplossingen aan te bieden aan klanten die hierom vragen. Dit is aanzienlijk eenvoudiger dan het AllesOnline CMS in bochten te wringen om te voldoen aan vereisten waarvoor het praktisch gezien niet is ontworpen.
	* Filament is open-source en wordt aangeboden onder de MIT-licentie. Dit betekent dat er geen directe licentiekosten zijn verbonden aan het gebruik ervan.
	* Filament heeft relatief uitgebreide documentatie en een snel groeiende community. Dit maakt het eenvoudiger voor ontwikkelaars om ondersteuning en voorbeelden te vinden. Aangezien Filament nog relatief nieuw is, zijn er toch nog wat beperkingen. Hierdoor kan de documentatie op sommige vlakken beperkt zijn, wat ertoe kan leiden dat developers meer tijd kwijt zijn aan het onderzoeken van specifieke functionaliteiten in edgecases.
	* Filament is gebaseerd op Laravel, een framework waarmee het huidige development-team van AllesOnline al goed bekend is.

# Onderzoek migratie van AllesOnline naar Filament CMS

Nadat ik zowel een AllesOnline-prototype als een Filament CMS-prototype had gerealiseerd, kon ik aan de slag met een van de belangrijke wensen van AllesOnline: onderzoeken of het mogelijk is om bestaande projecten met het AllesOnline CMS op een geautomatiseerde manier te migreren naar een het Filament CMS.

Om tijd te besparen, heb ik besloten niet het volledige proces uit te werken, maar een Proof of Concept (PoC) te realiseren. Het doel van de PoC is om aan te tonen dat de kern van het AllesOnline CMS, namelijk de XML-schema's  omgezet kunnen worden naar de schema's die in Filament worden gebruikt. Voor het realiseren van deze tool heb ik gekozen om gebruik te maken van Symfony CLI-componenten.

> _Verantwoording voor de keuze om Symfony voor de tool gebruiken lees je hier_:
> * [Onderzoek naar Tool(s) voor Migratie](../AnalyseAdvies/OnderzoekNaarToolsVoorMigratie.md)

Met de tool is het in eerste opzet mogelijk om een XML-block om te zetten naar een PHP-class die gebruikt kan worden door het Filament CMS. 

> * [Repository PoC: Transcription (migratietool)](https://github.com/Quitzchell/poc-transcription)

### Conclusies onderzoek migratie AllesOnline CMS naar Filament CMS

* **Hoe verloopt de migratie van de bestaande webapplicaties naar een nieuw CMS, en welke technische uitdagingen komen hierbij kijken?**
	* Voor een migratie van een bestaande website die gebruik maakt van het AllesOnline CMS is goede planning en vooronderzoek nodig. Mocht er buiten de standaard AllesOnline CMS werking ook maatwerk voor de website zijn ingebouwd, dan moet eerst onderzocht worden hoe deze gemigreerd kan worden. 
	* De migratie van bestaande website naar een nieuw CMS kan verlopen via een op maat gebouwde CLI-tool, bijvoorbeeld met Symfony. Dit maakt het mogelijk om bestanden te lezen, aan te passen en over te zetten naar een nieuw CMS. 
	* Technische uitdagingen omvatten de verschillen in gegevensstructuur tussen de nieuwe en oude systemen. In het voorbeeld van het AllesOnline CMS en het Filament CMS vindt de validatie van input door gebruikers in andere bestanden plaats. Dit betekent dat het overzetten van schema's voor objecten relatief intensiever is dan het omzetten van contentschema's. Dit komt doordat contentschema's in het AllesOnline CMS geen mogelijkheid tot input validatie bieden.

# Onderzoek naar Statamic CMS

Na het afronden van de Proof of Concept (PoC) voor een migratietool om het AllesOnline CMS om te zetten naar een Filament CMS, ben ik begonnen met het ontwikkelen van een prototype voor het CMS met Statamic. In tegenstelling tot Filament is Statamic specifiek ontworpen om als CMS te fungeren. Bovendien is het een commercieel product waarvoor een licentie moet worden aangeschaft bij commercieel gebruik.

Hoewel de standaardconfiguratie van Statamic zeer snel kan worden opgezet, is dit waarschijnlijk niet de meest geschikte configuratie voor een AllesOnline-website. Voor deze toepassing is een combinatie van flat-file, de Eloquent-driver en Runway waarschijnlijk de beste keuze.

> * [Onderzoek naar Statamic](../AnalyseAdvies/OnderzoekNaarStatamicCMS.md)
> * [SWOT: Statamic](../AnalyseAdvies/SwotStatamicCms.md)