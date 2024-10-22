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

In de eerste weken van het project heb ik me voornamelijk gericht op het onderzoeken van het huidige CMS, waarbij ik verschillende invalshoeken heb toegepast. Allereerst heb ik informele gesprekken gevoerd met de developers binnen AllesOnline om inzicht te krijgen in hun ervaringen en de uitdagingen die zij tegenkomen bij het werken met het CMS. Daarbij heb ik ook mijn eigen ervaringen met CMS meegenomen. Vervolgens heb ik een grondige analyse uitgevoerd van de code, de structuur en de functionaliteiten van het systeem. Dit proces liep parallel aan het opstellen van de requirements en het realiseren van een prototype-applicatie, waardoor ik dieper in de werking van het CMS ben gedoken en zelf de belangrijkste knelpunten in de praktijk heb ervaren.

Uit de gesprekken en bevindingen van de analyse zijn verschillende punten naar voren gekomen waar het CMS niet aan bepaalde verwachtingen voldoet. Ondanks deze beperkingen lukt het vaak wel om pragmatische oplossingen te vinden, maar dit staat wel haaks op enkele ontwikkelingsstandaarden, zoals de SOLID-principes. Tijdens het onderzoek en het realiseren van het prototype heb ik vanuit een bepaalde invalshoek al antwoorden kunnen formuleren op enkele deelvragen uit mijn projectplan.

* [Onderzoek naar het AllesOnline CMS](analyse/OnderzoekNaarHetAOCms.md)
* [Requirements](Analyse/Requirements.md)
* [Prototype](Analyse/prototype.md)

Aan de hand van deze activiteiten kan ik al voor een deel antwoord geven op de volgende deelvragen: 
 
__Wat zijn de belangrijkste technische en functionele beperkingen van het huidige CMS?__
* **Beperkte Documentatie:** De documentatie is verouderd, waardoor ontwikkelaars onnodig lang bezig zijn bij het begrijpen van bepaalde functionaliteiten en parameters.
- **Complexe Codestructuur:** Veel modules hebben te veel verantwoordelijkheden, wat onderhoud en uitbreiding bemoeilijkt.
- **Sterke Afhankelijkheid:** Modules zijn te afhankelijk van elkaar, waardoor wijzigingen in één module andere modules kunnen beïnvloeden.
- **Niet-Naleven van SOLID-principes:** Het systeem voldoet niet aan de SOLID-principes, wat leidt tot hogere complexiteit en lagere testbaarheid en onderhoudbaarheid.

__Aan welke criteria moet een gemoderniseerd CMS voldoen?__
* **Uitgebreide Documentatie:** Het CMS moet beschikken over actuele documentatie die modules en parameters helder beschrijft.
- **Modulariteit:** Het systeem moet opgesplitst zijn in kleinere, onafhankelijke modules met specifieke verantwoordelijkheden zodat deze gemakkelijk uitgebreid kan worden als nodig.

__Wat zijn de prestatieverschillen en kosten tussen het huidige CMS en een nieuw systeem?__
* **Prestatieverschillen:** Het gebrek aan modulariteit en documentatie resulteert in hogere onderhoudskosten en meer bugs, wat de prestaties en stabiliteit beïnvloedt.
* **Kosten:** De investeringen in het opschonen en onderhouden van een eigen systeem moet goed worden afgewogen tegen de voordelen van een al bestaande oplossing, die doorgaans lagere onderhoudskosten met zich meebrengt.
