
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
     

# Onderzoek naar huidige CMS en opzet Prototype applicatie

In de eerste weken van dit project heb ik mij gericht op het analyseren van het AllesOnline CMS. Om een goed beeld te krijgen en hier gericht advies over uit te kunnen brengen, heb ik gesprekken gevoerd met mijn collega-developers van AllesOnline over hun ervaringen en de uitdagingen die zij tegenkomen bij het werken met het CMS. Daarnaast heb ik mijn eigen ervaringen met het CMS meegenomen in deze analyse en ben ik de code van het CMS ingedoken om op een dieper niveau te begrijpen hoe het systeem is opgebouwd.

Uit de gesprekken en de analyse bleek dat de meeste developers tevreden zijn met het CMS. Hoewel niet alles volgens Best Practices verloopt en er onduidelijkheden bestaan over de werking van het CMS, slagen de meeste developers erin om op een pragmatische manier tot oplossingen te komen. Toch valt op dat de developers binnen AllesOnline graag een verbeterde developer experience zouden zien. Hierbij kan gedacht worden aan meer abstractie van functionaliteiten, waardoor complexiteit beter kan worden verborgen en herbruikbare oplossingen eenvoudiger te implementeren zijn. Dit kan bijdragen aan het sneller en eenvoudiger realiseren van specifieke klantwensen.

> * [Gesprekken met Developers](GesprekkenEnErvaringenMetDevelopers.md)
> * [Onderzoek naar het AllesOnline CMS](../AnalyseAdvies/OnderzoekNaarHetAOCms.md)
> * [SWOT: AllesOnline CMS](../AnalyseAdvies/SwotAOCms.md)

Parallel aan het onderzoek heb ik requirements opgesteld voor wat het CMS moet bieden om te voldoen aan de eisen van een doorsnee AllesOnline-website. Op basis van deze requirements heb ik een DOT-frameworkesque `Multi-Criteria Decision Making`-analyse opgesteld, waarmee ik op een eenvoudige manier kan bijhouden hoe de te onderzoeken frameworks aan deze requirements voldoen.

> * [Requirements](../AnalyseAdvies/Requirements.md)
> * [Checklist voor CMS criteria](../AnalyseAdvies/ChecklistVoorCMSCriteria)

Aan de hand van de requirements en een duidelijk beeld van wat er van een doorsnee AllesOnline-website verwacht wordt, ben ik aan de slag gegaan met het realiseren van een prototype van een website. In eerste instantie was het de bedoeling om een stamboom applicatie te ontwikkelen op basis van de familie Bonaparte. Toen echter de incestueuze geschiedenis van de Europese koningshuizen naar voren kwam – en de complexiteit van het verwerken hiervan in een systeem duidelijk werd – heb ik ervoor gekozen om in plaats daarvan een eenvoudige blog-website te realiseren, geschreven vanuit het perspectief van Napoleon Bonaparte.

> * [Opzet van het prototype](OpzetVanDePrototypes.md)

een prototype-applicatie ontwikkeld, waardoor ik het CMS op praktische knelpunten kon evalueren. Dit prototype bestaat uit drie lagen: een frontend, verwisselbare backends gebaseerd op verschillende CMS-pakketten (waaronder het AllesOnline CMS), en een database gevuld met dummydata die gegenereerd wordt door seeders. Deze opzet creëert een gestandaardiseerde testomgeving waarin iedere CMS-implementatie onafhankelijk kan worden beoordeeld op functionaliteit en prestaties via end-to-end tests.


> * [Repository: Frontend Prototype](https://github.com/Quitzchell/graduation-frontend)
> * [Repository: Backend AO CMS](https://github.com/Quitzchell/graduation-ao-cms/)

Tijdens dit onderzoek en de ontwikkeling van het prototype kan ik vanuit verschillende invalshoeken al antwoorden formuleren op enkele deelvragen.
 
* __Wat zijn de belangrijkste technische en functionele beperkingen van het huidige CMS?__
	* **Beperkte documentatie:** De documentatie is verouderd, waardoor ontwikkelaars onnodig lang bezig zijn met het begrijpen van bepaalde functionaliteiten en parameters.
	* **Complexe codestructuur:** Veel modules hebben te veel verantwoordelijkheden, wat onderhoud en uitbreiding bemoeilijkt.
	* **Sterke afhankelijkheid:** Modules en functionaliteiten zijn te afhankelijk van elkaar, waardoor wijzigingen in één module andere modules kunnen beïnvloeden.
	* **Niet-naleven van SOLID-principes:** Het systeem voldoet niet aan de SOLID-principes, wat leidt tot hogere complexiteit en lagere testbaarheid en onderhoudbaarheid.

* __Aan welke criteria moet een gemoderniseerd CMS voldoen?__
	* **Uitgebreide documentatie:** Het CMS beschikt over actuele documentatie die modules en parameters helder beschrijft.
	* **Modulariteit:** Het systeem moet opgesplitst zijn in kleinere, onafhankelijke modules met specifieke verantwoordelijkheden zodat deze gemakkelijk uitgebreid kan worden indien nodig.

* __Wat zijn de prestatieverschillen en kosten tussen het huidige CMS en een nieuw systeem?__
	* **Prestatie:** Het gebrek aan modulariteit en documentatie resulteert in hogere onderhoudskosten en meer bugs, wat de prestaties en stabiliteit beïnvloedt.
	* **Kosten:** De investeringen in het opschonen en onderhouden van een eigen systeem moet goed worden afgewogen tegen de voordelen van een al bestaande oplossing, die doorgaans lagere onderhoudskosten met zich meebrengt.

Om ervoor te zorgen dat mijn collega's de prototypes op hun eigen systeem kunnen draaien, heb voor alle prototypes Docker-containers voorbereid. Bij de backends maak ik gebruik van de voorgedefinieerde AllesOnline-container, met een uitbreiding die het mogelijk maakt om een SQLite-database te gebruiken. Deze uitbreiding het eenvoudiger om zowel tijdens het programmeren als binnen een pipeline featuretests uit te voeren, zonder de reguliere database te beïnvloeden.

# Opzetten van nulmeting en frontend tests met Cypress

Om de betrouwbaarheid en consistentie van verschillende backends ten opzichte van het AllesOnline CMS te verifiëren, heb ik een Cypress-testsuite opgezet voor de frontend van het prototype.

De suite controleert of de gegevensuitwisseling tussen de backend en frontend correct verloopt en evalueert of de frontend de gegevens accuraat kan ontvangen. Hiermee kan, zodra een CMS-implementatie gerealiseerd is, door middel van gestandaardiseerde tests gevalideerd worden.

> * [Cypress test-suite in frontend repository](https://github.com/Quitzchell/graduation-frontend/tree/main/src/cypress)
> * [Video: Cypress test-suite](../Bijlagen/CypressTestsAOCms.md)

Omdat Cypress niet in onze standaard Docker-container kan draaien vanwege bepaalde dependencies die ontbreken in een Alpine-omgeving, heb ik het zo ingericht dat een externe container, specifiek opgezet voor Cypress, kan communiceren met de frontend-container om de tests uit te voeren. Het voordeel hiervan is dat de container voor de frontend niet onnodig groter wordt door de toevoeging van Cypress. Het nadeel is echter dat er een pauze moet worden ingebouwd tussen de requests, omdat de frontend-container anders te snel opeenvolgende verzoeken van hetzelfde adres ontvangt.

# Onderzoek naar CMS met Filament

Na het realiseren van het prototype met het AllesOnline CMS en het voorbereiden van de Cypress Test-suite, ben ik aan de gang gegaan met het onderzoeken van Filament. Filament is zoals ze het zelf zeggen  *"A collection of beautiful full-stack  components"*. Het biedt een verzameling componenten die ingezet kunnen worden om eenvoudig CRUD-functionaliteit op te zetten om gegevens te beheren. 

> * [Onderzoek naar CMS met Filament](../AnalyseAdvies/OnderzoekNaarCMSMetFilament.md)
> * [SWOT: Filament](../AnalyseAdvies/SwotFilamentCms.md)

Tijdens het onderzoek ben ik ook begonnen met het ontwerpen en realiseren van een CMS met behulp van de Filament Library. Hierbij heb ik zowel de checklist voor CMS-criteria als de requirements in acht genomen. Daarnaast ben ik begonnen met het beschrijven van de vergelijking tussen hoe specifieke functionaliteiten gerealiseerd kunnen worden in verschillende frameworks en libraries.

> * [Design voor CMS functionaliteit in Filament](../Bijlagen/UmlEntiteitenDiagramContentManagementFilament.md)
> * [Concepten voor CMS Prototype Realisaties](../DesignRealisatie/CmsPrototypesRealisatie.md)
> * [Repository: Backend Filament CMS](https://github.com/Quitzchell/graduation-filament-cms)

* **Welke commerciële en open-source CMS-oplossingen voldoen aan de vereisten voor modernisering en kunnen een haalbare vervanging bieden voor het huidige systeem?**
	- **Filament**: Filament is dankzij zijn flexibiliteit en modulaire opbouw geschikt als basis voor een CMS voor organisaties die maatwerk nodig hebben bij het beheren van hun contentbehoeften. Daarnaast is de library populair binnen de Laravel-community ([State of Laravel, Administration Panel Results](https://stateoflaravel.com/results#question:administration+panel)). Vanwege de beperkte ondersteuning voor diepere hiërarchieën en relationele structuren is de library echter nog afhankelijk van externe of zelfontwikkelde oplossingen.


* **Wat zijn de prestatieverschillen en kosten tussen het huidige CMS en een nieuw systeem?**
	* **Ondersteuning en ontwikkelkosten**: Filament heeft een groeiende community maar kan bij complexe functies extra ontwikkeltijd vergen vanwege het gebrek aan out-of-the-box oplossingen voor hiërarchische content. Dit kan leiden tot hogere onderhouds- en ontwikkelkosten in vergelijking met meer volwassen systemen.
	* **Laravel**: Filament maakt gebruik van Laravel’s Eloquent ORM, wat het beheer van resources vergemakkelijkt. Daarnaast biedt het het huidige developer-team een voordeel omdat ze al bekenend met Laravel zijn.
	* **Documentatie en schaalbaarheid**: De relatief nieuwe status van Filament betekent dat de ondersteuning en documentatie nog beperkt zijn, wat kan resulteren in hogere kosten om specifieke oplossingen te ontwikkelen en te onderhouden.