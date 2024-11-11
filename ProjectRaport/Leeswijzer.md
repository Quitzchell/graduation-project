# Inleiding

Dit verslag beschrijft het verloop van het project _'Revitalising Content Management'_, waarin wordt onderzocht hoe AllesOnline, een full-service bureau voor online en offline communicatie, haar huidige Content Management Systeem (CMS) kan moderniseren. Het bestaande CMS, dat sinds 2015 operationeel is, vormt de basis van de webapplicaties van het bedrijf. Door veroudering en beperkt onderhoud is het systeem echter moeilijk uitbreidbaar en minder efficiënt geworden, wat de schaalbaarheid en toekomstbestendigheid belemmert.

Binnen dit project wordt onderzocht of migratie naar een nieuw, modern CMS een duurzamer en kostenefficiënter alternatief biedt voor doorontwikkeling van de huidige systeem. Er wordt hierbij specifiek gekeken naar de beperkingen van het bestaande systeem, de vereisten voor een toekomstbestendig CMS, mogelijke alternatieven en de uitdagingen en voordelen van een migratieproces.
## Onderzoeksvraag
 Hoe kan AllesOnline een bestaand Content Management Systeem inzetten om de schaalbaarheid, onderhoudbaarheid en toekomstbestendigheid van haar webapplicaties te verbeteren?
### Deelvragen

1. **Wat zijn de belangrijkste technische en functionele beperkingen van het huidige CMS van AllesOnline?**
    
2. **Aan welke criteria moet een gemoderniseerd CMS voldoen om de huidige en toekomstige behoeften van AllesOnline te ondersteunen?**
    
3. **Welke commerciële en open-source CMS-oplossingen voldoen aan de vereisten voor modernisering en kunnen een haalbare vervanging bieden voor het huidige systeem?**
    
4. **Hoe verloopt de migratie van de bestaande webapplicaties naar een nieuw CMS, en welke technische uitdagingen komen hierbij kijken?**
    
5. **Welke prestatieverschillen en kosten zijn er tussen het huidige CMS en een nieuw systeem?**
     

# Onderzoek naar huidige CMS en opzet Prototype applicatie

In de eerste weken van het project heb ik mij gericht op het analyseren van het huidige CMS. Om een gedegen beeld te krijgen, heb ik gesprekken gevoerd met de developers van AllesOnline over hun ervaringen en de uitdagingen die zij tegenkomen bij het werken met het CMS. Daarnaast heb ik mijn eigen ervaringen met CMS-systemen meegenomen en een grondige analyse gedaan van de structuur en functionaliteiten binnen de codebase.

Uit de gesprekken en analyses kwamen verschillende knelpunten naar voren die aantonen dat het huidige CMS niet volledig aan de verwachtingen voldoet. Hoewel pragmatische oplossingen vaak mogelijk zijn, botsen deze oplossingen met ontwikkelingsstandaarden, zoals de SOLID-principes.

* [Gesprekken met Developers](AnalyseAdvies/GesprekkenMetDevelopers.md)
* [Onderzoek naar het AllesOnline CMS](AnalyseAdvies/OnderzoekNaarHetAOCms.md)
* [SWOT: AllesOnline CMS](AnalyseAdvies/SwotAOCms.md)

Parallel aan het onderzoek heb ik de requirements opgesteld en een prototype-applicatie ontwikkeld, waardoor ik het CMS op praktische knelpunten kon evalueren. Dit prototype bestaat uit drie lagen: een frontend, verwisselbare backends gebaseerd op verschillende CMS-pakketten (waaronder het AllesOnline CMS), en een database gevuld met dummydata die gegenereerd wordt door seeders. Deze opzet creëert een gestandaardiseerde testomgeving waarin iedere CMS-implementatie onafhankelijk kan worden beoordeeld op functionaliteit en prestaties via end-to-end tests.

* [Requirements](AnalyseAdvies/Requirements.md)
* [Checklist voor CMS criteria](AnalyseAdvies/ChecklistVoorCMSCriteria)
* [Opzet van het prototype](DesignRealisatie/OpzetVanHetPrototype.md)
* [Repository: Frontend Prototype](https://github.com/Quitzchell/graduation-frontend)
* [Repository: Backend AO CMS](https://github.com/Quitzchell/graduation-ao-cms/)

Tijdens dit onderzoek en de ontwikkeling van het prototype kan ik vanuit verschillende invalshoeken al antwoorden formuleren op enkele deelvragen.
 
__Wat zijn de belangrijkste technische en functionele beperkingen van het huidige CMS?__
* **Beperkte Documentatie:** De documentatie is verouderd, waardoor ontwikkelaars onnodig lang bezig zijn bij het begrijpen van bepaalde functionaliteiten en parameters.
* **Complexe Codestructuur:** Veel modules hebben te veel verantwoordelijkheden, wat onderhoud en uitbreiding bemoeilijkt.
* **Sterke Afhankelijkheid:** Modules zijn te afhankelijk van elkaar, waardoor wijzigingen in één module andere modules kunnen beïnvloeden.
* **Niet-Naleven van SOLID-principes:** Het systeem voldoet niet aan de SOLID-principes, wat leidt tot hogere complexiteit en lagere testbaarheid en onderhoudbaarheid.

__Aan welke criteria moet een gemoderniseerd CMS voldoen?__
* **Uitgebreide Documentatie:** Het CMS moet beschikken over actuele documentatie die modules en parameters helder beschrijft.
* **Modulariteit:** Het systeem moet opgesplitst zijn in kleinere, onafhankelijke modules met specifieke verantwoordelijkheden zodat deze gemakkelijk uitgebreid kan worden als nodig.

__Wat zijn de prestatieverschillen en kosten tussen het huidige CMS en een nieuw systeem?__
* **Prestatieverschillen:** Het gebrek aan modulariteit en documentatie resulteert in hogere onderhoudskosten en meer bugs, wat de prestaties en stabiliteit beïnvloedt.
* **Kosten:** De investeringen in het opschonen en onderhouden van een eigen systeem moet goed worden afgewogen tegen de voordelen van een al bestaande oplossing, die doorgaans lagere onderhoudskosten met zich meebrengt.

# Opzetten van nulmeting en frontend tests met Cypress

Om de betrouwbaarheid en consistentie van verschillende backends ten opzichte van het AllesOnline CMS te verifiëren, heb ik een Cypress-testsuite opgezet voor de frontend van het prototype. Deze tests controleren of de gegevensuitwisseling tussen backend en frontend juist verloopt, zodat de informatie correct wordt weergegeven. De Cypress-scripts evalueren specifiek of de frontend de gegevens accuraat ontvangt van de verschillende backend-implementaties. Hiermee kunnen, zodra een CMS-opzet met een van de onderzochte libraries is gerealiseerd, problemen met gegevensintegriteit vroegtijdig worden opgespoord. Dit zorgt voor een gestandaardiseerde basis om CMS-opties te vergelijken op functionaliteit en prestaties.

