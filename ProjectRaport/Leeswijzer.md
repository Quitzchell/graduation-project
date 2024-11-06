# Inleiding

Dit verslag beschrijft het project _'Revitalising Content Management'_, waarin wordt onderzocht hoe AllesOnline, een full-service bureau voor online en offline communicatie, haar huidige Content Management Systeem (CMS) kan moderniseren. Het huidige CMS, dat sinds 2015 operationeel is, vormt de basis van de webapplicaties van het bedrijf, maar is door veroudering en gebrek aan onderhoud moeilijk uitbreidbaar en minder efficiënt geworden. Dit belemmert de schaalbaarheid en toekomstbestendigheid van de systemen.

In dit project wordt de vraag onderzocht of migratie naar een nieuw, modern CMS een duurzamer en kostenefficiënter alternatief biedt voor de doorontwikkeling van de bestaande code. Er wordt hierbij specifiek gekeken naar de beperkingen van het huidige systeem, de eisen aan een toekomstbestendig CMS, mogelijke alternatieven en de uitdagingen en voordelen van een migratieproces.
## Onderzoeksvraag
 Hoe kan AllesOnline een bestaand Content Management Systeem inzetten om de schaalbaarheid, onderhoudbaarheid en toekomstbestendigheid van haar webapplicaties te verbeteren?
### Deelvragen

1. **Wat zijn de belangrijkste technische en functionele beperkingen van het huidige CMS van AllesOnline?**
    
2. **Aan welke criteria moet een gemoderniseerd CMS voldoen om de huidige en toekomstige behoeften van AllesOnline te ondersteunen?**
    
3. **Welke commerciële en open-source CMS-oplossingen voldoen aan de vereisten voor modernisering en kunnen een haalbare vervanging bieden voor het huidige systeem?**
    
4. **Hoe verloopt de migratie van de bestaande webapplicaties naar een nieuw CMS, en welke technische uitdagingen komen hierbij kijken?**
    
5. **Welke prestatieverschillen en kosten zijn er tussen het huidige CMS en een nieuw systeem?**
     

# Onderzoek naar huidige CMS en opzet Prototype applicatie

In de eerste weken van het project heb ik me voornamelijk gericht op het onderzoeken van het huidige CMS, waarbij ik verschillende invalshoeken heb toegepast. Allereerst heb ik informele gesprekken gevoerd met de developers binnen AllesOnline om inzicht te krijgen in hun ervaringen en de uitdagingen die zij tegenkomen bij het werken met het CMS. Daarbij heb ik ook mijn eigen ervaringen met CMS meegenomen. Vervolgens heb ik een analyse gedaan van de structuur en functionaliteiten in de codebase van het CMS.

Uit de gesprekken en bevindingen van de analyse zijn verschillende punten naar voren gekomen waar het CMS niet aan bepaalde verwachtingen voldoet. Ondanks deze beperkingen lukt het vaak wel om pragmatische oplossingen te vinden, maar dit staat wel haaks op enkele ontwikkelingsstandaarden, zoals de SOLID-principes. 

* [Onderzoek naar het AllesOnline CMS](OnderzoekNaarHetAOCms.md)

Dit proces liep parallel aan het opstellen van de requirements en het ontwikkelen van een prototype-applicatie, waardoor ik ook dieper in de werking van het CMS moest duiken en bepaalde knelpunten in de praktijk ervaarde. Het prototype dat ik heb gerealiseerd is opgebouwd uit drie lagen: een frontend, verwisselbare backends gebaseerd op diverse CMS-pakketten (waaronder het AllesOnline CMS) inclusief database gevuld met dummydata die te genereren is doormiddel van seeders. Deze opzet zorgt voor een gestandaardiseerde situatie waarin elke CMS-implementatie aan de hand van e2e-tests onafhankelijk kan worden beoordeeld op functionaliteit en prestaties.

* [Requirements](Requirements.md)
* [Opzet van het prototype](OpzetVanHetPrototype.md)

Tijdens het onderzoek en het realiseren van het prototype heb ik vanuit een bepaalde invalshoek al antwoorden kunnen formuleren op enkele deelvragen uit mijn projectplan.
 
__Wat zijn de belangrijkste technische en functionele beperkingen van het huidige CMS?__
1. **Beperkte Documentatie:** De documentatie is verouderd, waardoor ontwikkelaars onnodig lang bezig zijn bij het begrijpen van bepaalde functionaliteiten en parameters.
2. **Complexe Codestructuur:** Veel modules hebben te veel verantwoordelijkheden, wat onderhoud en uitbreiding bemoeilijkt.
3. **Sterke Afhankelijkheid:** Modules zijn te afhankelijk van elkaar, waardoor wijzigingen in één module andere modules kunnen beïnvloeden.
4. **Niet-Naleven van SOLID-principes:** Het systeem voldoet niet aan de SOLID-principes, wat leidt tot hogere complexiteit en lagere testbaarheid en onderhoudbaarheid.

__Aan welke criteria moet een gemoderniseerd CMS voldoen?__
1. **Uitgebreide Documentatie:** Het CMS moet beschikken over actuele documentatie die modules en parameters helder beschrijft.
2. **Modulariteit:** Het systeem moet opgesplitst zijn in kleinere, onafhankelijke modules met specifieke verantwoordelijkheden zodat deze gemakkelijk uitgebreid kan worden als nodig.

__Wat zijn de prestatieverschillen en kosten tussen het huidige CMS en een nieuw systeem?__
1. **Prestatieverschillen:** Het gebrek aan modulariteit en documentatie resulteert in hogere onderhoudskosten en meer bugs, wat de prestaties en stabiliteit beïnvloedt.
2. **Kosten:** De investeringen in het opschonen en onderhouden van een eigen systeem moet goed worden afgewogen tegen de voordelen van een al bestaande oplossing, die doorgaans lagere onderhoudskosten met zich meebrengt.

# Opzetten van nulmeting en frontend tests met Cypress

Om de betrouwbaarheid en consistentie van diverse backends te verifiëren ten opzichte van het AllesOnline CMS, heb ik voor de frontend van het prototype een Cypress-testsuite opgezet. Deze tests controleren of de gegevensuitwisseling tussen backend en front-end correct verloopt, zodat de informatie op de juiste wijze wordt weergegeven. De Cypress-scripts evalueren specifiek of de front-end gegevens, afkomstig van een van de verschillende backendimplementaties, correct ontvangt en presenteert. Hierdoor kunnen problemen met gegevensintegriteit vroegtijdig worden opgespoord en ontstaat een gestandaardiseerde basis om CMS-opties te vergelijken op functionaliteit en prestaties.

