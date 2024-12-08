# Onderzoek naar het AllesOnline CMS

# Inleiding

Dit document beschrijft de analyse van het AllesOnline CMS. Deze is uitgevoerd als onderdeel van een *Available Product Analysis*, maar is ook gericht op het identificeren van knelpunten binnen het huidige AllesOnline CMS. 

> _Andere onderdelen van het Available Product Analysis:_
> * [Onderzoek naar Filament](../AnalyseAdvies/OnderzoekNaarFilament.md)

> *De belangrijkste bevindingen over het AllesOnline CMS zijn samengebracht in een SWOT-analyse.* 
> * [SWOT: AllesOnline CMS ](./SwotAOCms.md)

## Systeemoverzicht: Beheren van content 

Het AllesOnline CMS is gebouwd op **Laravel** en maakt gebruik van de Eloquent ORM van dit framework. In het CMS zijn verschillende modules beschikbaar, die samen met de bijbehorende frontend componenten het opzetten van een CMS voor een website mogelijk maken. Hieronder staan een aantal van de belangrijkste onderdelen van het systeem die het content beheer beschrijven.

### ContentManagerController

De `ContentManagerController` beheert de content binnen het CMS en biedt functionaliteiten voor het toevoegen, bewerken, verwijderen en kopiëren van pagina's. Daarnaast biedt het de functionaliteit om de rechten van gebruikers te verifiëren voor het beheren van specifieke content en pagina's binnen het CMS.

### ContentManagerModule

De `ContentManagerModule` en bijbehorende view zijn verantwoordelijk voor het tonen en beheren van de pagina's binnen het CMS. Deze module biedt mogelijkheden om pagina's aan te maken, bewerken, verwijderen en ordenen. De weergave van de module wordt verzorgd door een **blade**-template, waarin de pagina's en de pagina-structuur overzichtelijk worden weergegeven in een drag-en-drop-interface. Afhankelijk van de gebruikersrechten worden de bewerkings- en verwijder opties getoond, naast de opties om pagina's te bekijken of te kopiëren.

### Page (model)

De structuur van een website die gebruikmaakt van het AllesOnline CMS begint bij het `Page`-model. Dit model vertegenwoordigt individuele pagina's en slaat basisinformatie op, zoals de naam van de pagina en het toegepaste `Template`. Aan de hand van deze `Templates` wordt bepaald welke content aan een pagina kan worden toegevoegd. De content wordt op zijn beurt gepersisteerd via een polymorfe relatie tussen objecten die content aanbieden en het `CmsContent`-model. 

> _Een ERD met betrekking tot het `Page`-model en CMS content_:
> * [ERD AllesOnline CMS page model](../Bijlagen/ErdAoCmsPageModel.md).

### Templates

Binnen de repository van de websites die gebruikmaken van het AllesOnline CMS, kunnen de eerder genoemde `templates` dienen als een schema dat voorschrijft welke informatie op een pagina beschikbaar is. Deze templates kunnen worden gedefinieerd aan de hand van XML-bestanden. Binnen deze templates kunnen velden worden gedefinieerd die op hun beurt verwijzen naar `formfield`-modules.

Binnen templates is het ook mogelijk om te verwijzen naar andere schema's waarin een verzameling van `formfield`-modules beschikbaar is. Deze schema's worden binnen het CMS `blocks` genoemd. `Blocks` vormen de basis voor grotere, samengestelde elementen die regelmatig binnen een website worden hergebruikt.

### FormFields

De `FormField`-modules en bijbehorende views bieden een interface waarmee eindgebruikers content kunnen toevoegen of bewerken binnen het CMS. Denk hierbij aan invoervelden voor bijvoorbeeld tekst of bestanden. De weergave van de `FormFields` wordt gedefinieerd via `blade`-templates.

## Beheren van objecten

Naast het beheren van content op pagina's biedt het CMS ook de mogelijkheid om objecten te beheren die binnen een website gebruikt kunnen worden. Denk hierbij aan producten in een webshop of workshops op een website die opleidingen aanbiedt. Hieronder staan een aantal van de belangrijkste onderdelen van het systeem die het contentbeheer beschrijven.

### ObjectManagerController

De `ObjectManagerController` is verantwoordelijk voor het beheren van objecten. Deze controller breidt de standaard Laravel-controller uit en biedt functionaliteit voor acties zoals het bewerken en verwijderen van objecten, het beheren van relaties tussen modellen en het afhandelen van permissies en autorisatie. Daarnaast bevat de controller ook logica voor acties zoals het sorteren, zoeken en ordenen van objecten binnen het CMS.

### Eloquent models

Voor de representatie van objecten in het CMS wordt gebruikgemaakt van `Eloquent`-modellen. `Eloquent` is een ORM dat onderdeel is van Laravel en het mogelijk maakt om op een eenvoudige, objectgeoriënteerde manier met gegevens van objecten te werken. Denk hierbij aan het ophalen, bijwerken en verwijderen van gegevens, en het definiëren van relaties tussen verschillende objecten.

### CMS schema's

In de repositories van de websites die gebruikmaken van het AllesOnline CMS kunnen op basis van XML-bestanden ook CMS-schema's voor objecten worden aangemaakt. Deze schema's dienen als blauwdruk voor de weergave en het beheer van objecten in het CMS. Voor het definiëren van de invoervelden die door eindgebruikers kunnen worden gebruikt om objecten te beheren, worden, net als bij de templates voor content, `FormFields` gebruikt.

In deze schema's kan ook worden gedefinieerd hoe relaties tussen objecten kunnen worden beheerd. Denk hierbij aan mogelijkheden om gerelateerde objecten toe te voegen, te bewerken of los te koppelen vanuit een object dat een eindgebruiker aan het aanmaken of bewerken is.

### FormFields

De `FormField`-modules en bijbehorende views bieden - net zoals bij contentbeheer - een interface waarmee eindgebruikers informatie van objecten kunnen ingeven of bewerken. 

## Evaluatie van het AllesOnline CMS

Hoewel het door AllesOnline ontwikkelde CMS functioneel is en een goede basis biedt voor webontwikkeling, kent het systeem aanzienlijke beperkingen op het gebied van het bieden van een goede development experience, onderhoudbaarheid en uitbreidbaarheid. 

Hieronder een evaluatie die zich onderandere richt op de architectuur, functionaliteiten, documentatie en hoe het voldoet aan de best practices binnen softwareontwikkeling.

### Gebrekkige Documentatie

Een van de grootste gebreken in het AllesOnline CMS is de beperkte en verouderde documentatie. Hoewel documentatie vaak wel beschikbaar is, ontbreekt gedetailleerde informatie over hoe bepaalde functionaliteiten moeten worden toegepast. In sommige gevallen ontbreekt zelfs volledig informatie over het bestaan van bepaalde functionaliteiten. Dit zorgt voor onduidelijkheid bij ontwikkelaars, die hierdoor niet goed kunnen achterhalen welke functionaliteiten beschikbaar zijn voor specifieke `FormField`-modules.

> _Een voorbeeld van documentatie vind je in deze bijlage:_
>  * [voorbeeld documentatie AllesOnline CMS](../Bijlagen/VoorbeeldAllesOnlineCmsSchema.md).

**Aanbeveling**:
- Een verbetering van de documentatie, inclusief een up-to-date beschrijving van alle beschikbare modules, attributen en parameters, met voorbeelden van hoe functionaliteiten in verschillende scenario’s kunnen worden gebruikt. Dit voorkomt onduidelijkheid en bespaart veel tijd die anders besteed zou worden aan het onderzoeken van implementaties tijdens het ontwikkelen van websites.

### Complexe Codestructuur

De huidige codestructuur van modules is het volgende probleem waar een developer na onduidelijkheid in de documentatie tegenaan kan lopen. De modules bevatten vaak te veel verantwoordelijkheden, wat niet alleen het begrijpen van de werking bemoeilijkt, maar ook het onderhoud en de uitbreiding ervan ingewikkelder maakt. Een goed voorbeeld hiervan is de `FormTagsModule`, die zowel verantwoordelijk is voor dataverwerking, database-interactie en UI-configuratie. Dit is in strijd is met het **Single Responsibility Principle**.

**Gevolgen van deze complexiteit:**
* De kans op bugs neemt toe, aangezien wijzigingen in een deel van de module vaak onbedoeld andere onderdelen kan beïnvloeden.
* Het ontbreken van duidelijke interfaces tussen modules beperkt de mogelijkheid om de applicatie uit te breiden of om componenten opnieuw te gebruiken.
* De onderhoudbaarheid en testbaarheid van de module worden belemmerd, wat leidt tot hogere ontwikkel-en beheerkosten.

**Aanbeveling:**
* Een grondige refactoring van de modules, waarbij verantwoordelijkheden worden opgesplitst in afzonderlijke classes of componenten, bijvoorbeeld:
	* Een service class voor dataverwerking.
	* Een repository voor database-interactie.
	* Een aparte module voor UI-gerelateerde configuratie.
* Dit vereist het definiëren van duidelijke interfaces tussen deze onderdelen, wat zorgt voor betere herbruikbaarheid, testbaarheid en uitbreidbaarheid van de applicatie.

### SOLID principes

Waar in het vorige probleem al gehint werd naar SOLID principes, zien we op een grote schaal door het AllesOnline CMS dat de **SOLID-principes**, wat de onderhoudbaarheid, uitbreidbaarheid en testbaarheid niet toegepast is.

#### Kernprincipes

1. **Single Responsibility Principle (SRP)**: Dit principe stelt dat een class slechts één reden tot verandering moet hebben. In het AllesOnline CMS zijn veel classes verantwoordelijk voor meerdere functionaliteiten, wat leidt tot complexiteit en bemoeilijkt het isoleren en hergebruiken van functionaliteiten.
    
2. **Open/Closed Principle (OCP)**: Classes moeten open zijn voor uitbreiding, maar gesloten voor aanpassing. In de huidige architectuur vereisen nieuwe functionaliteiten vaak wijzigingen in bestaande code, wat de kans op bugs vergroot en de onderhoudbaarheid bemoeilijkt.
    
3. **Dependency Inversion Principle (DIP)**: Modules op een hoger niveau moeten afhankelijk zijn van abstracties in plaats van concrete implementaties. In het AllesOnline CMS worden veel functionaliteiten direct binnen classes aangeroepen, dit belemmert onafhankelijkheid.
     
#### Gevolgen van niet naleven SOLID principes

- **Verlies van herbruikbaarheid**: Functionaliteiten zijn gedefineerd in modules, hierdoor is het niet mogelijk om bepaalde logica in andere delen van het CMS te hergebruiken. Dit leidt in eerste instantie tot duplicatie van code, wat een risico is voor het ontstaan van inconsistenties in functionaliteit.
    
- **Slechte testbaarheid**: Omdat logica vaak in één grotere functie in een module is ondergebracht, is het moeilijk om individuele functionaliteiten te testen zonder de gehele module te laden. Dit maakt het schrijven van unit tests lastig.
    
- **Moeilijk onderhoudbaar**: Het onderhoud van een grote module met veel functionaliteiten is uitdagend. Wijzigingen kunnen andere delen van de module beïnvloeden, wat de stabiliteit van het systeem in gevaar brengt.
    
- **Moeilijk uitbreidbaar**: Nieuwe functionaliteiten kunnen niet eenvoudig worden toegevoegd zonder bestaande code aan te passen.
    
- **Tight coupling**: De huidige architectuur creëert sterke afhankelijkheden tussen verschillende delen van de applicatie, wat de kans op regressies vergroot bij wijzigingen.
    
#### Aanbevelingen voor verbetering

1. **Herziening van de architectuur**: Een grondige herziening om meer SOLID-principes toe te passen. Ontleed bestaande modules in kleinere, zelfstandige classes met duidelijke verantwoordelijkheden die testbaar zijn.
    
2. **Implementatie van interface-gebaseerde ontwikkeling**: Verminder de afhankelijkheid van concrete implementaties door interfaces te gebruiken. Dit verhoogt de flexibiliteit van het systeem en maakt uitbreiding eenvoudiger. Bijvoorbeeld: laat de `FormTagsModule` via zijn constructor een class opvragen die de `TagLoader`-interface implementeert. Dit garandeert dat tags ingeladen kunnen worden, maar de manier waarop dit gebeurt bepaald wordt door de geïnjecteerde class.
    
3. **Unit Testing en Continuous Integration**: Ontwikkel een uitgebreide suite van unit- en integratietests en implementeer een Continuous Integration (CI)-ontwikkelstraat om regressies te minimaliseren.
    
4. **Documentatie en training**: Zorg voor documentatie van de nieuwe architectuur en richtlijnen, en organiseer trainingen voor ontwikkelaars over de SOLID-principes.
    
5. **Iteratieve verbeteringen**: Voer veranderingen iteratief door en evalueer regelmatig de impact op de onderhoudbaarheid, uitbreidbaarheid en testbaarheid van het systeem.

## Conclusie

Het is duidelijk dat het AllesOnline CMS ruimte heeft voor verbetering en refactoring. Ondanks de huidige tekortkomingen in de architectuur en de documentatie is het zeker mogelijk om de functionaliteiten van het systeem te optimaliseren. Het is echter belangrijk om te begrijpen dat het verbeteren van de codestructuur en het implementeren van de SOLID-principes wel een aanzienlijke investering in tijd en middelen vereist.

Mocht er gekozen worden om het AllesOnline CMS onder handen te nemen. Dan is het aan te raden om deze wijzigingen gefaseerd door te voeren. Dit is noodzakelijk omdat er niet alleen geïnvesteerd moet worden in de ontwikkeling van nieuwe modules, maar ook in het opstellen van uitgebreide documentatie, het doorvoeren van geautomatiseerde tests en het trainen van ontwikkelaars in het toepassen en begrijpen van de SOLID-principes.

bij het refactoren van het AllesOnline CMS is het belangrijk om de balans te vinden tussen de investering die AllesOnline bereid is te maken en de voordelen van het moderniseren van het CMS. Daarnaast is het essentieel om te realiseren dat het proces van modernisering niet eindigt na de refactor, maar dat het CMS goed onderhouden moet blijven om het op niveau te houden.