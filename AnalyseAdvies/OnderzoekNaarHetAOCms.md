# **Onderzoek naar AllesOnline CMS**

# Inleiding

Dit document beschrijft de analyse van het AllesOnline CMS en is gericht op het identificeren van knelpunten binnen het huidige systeem, evenals het doen van aanbevelingen. Deze analyse is uitgevoerd als onderdeel van een *Available Product Analysis*.

>*Andere onderdelen van het Available Product Analysis*:
>* [Onderzoek naar Filament](../AnalyseAdvies/OnderzoekNaarFilament.md) 
>* [Onderzoek naar Statamic](../AnalyseAdvies/OnderzoekNaarStatamicCMS.md)
>
>*De belangrijkste bevindingen over het AllesOnline CMS zijn samengebracht in een SWOT-analyse:* 
>* [SWOT: AllesOnline CMS](./SwotAOCms.md)

# Beheren van content 

Het AllesOnline CMS is geïntegreerd met **Laravel** en maakt gebruik van het **Eloquent ORM** binnen dit framework. Het CMS bevat verschillende modules, die samen met de bijbehorende frontendcomponenten het opzetten van een CMS mogelijk maken

## ContentManagerController

De `ContentManagerController` beheert de pagina's binnen het CMS en biedt functionaliteiten voor CRUD-operaties van pagina's. Daarnaast biedt het de functionaliteit om de rechten van gebruikers te verifiëren.

**Naast deze basisfunctionaliteiten biedt de controller ook de volgende functionaliteiten:**
* **Content hiërarchie en volgorde:** Het beheren van de hiërarchie en volgorde van pagina-items in de database via methoden zoals `updateManagedContent`.
* **Autorisatie:** Door middel van methoden zoals `can` en `methodPermission` wordt gecontroleerd of gebruikers de juiste rechten hebben om specifieke acties uit te voeren. Dit wordt ondersteund door middleware en specifieke permissies die in een configuratiebestand gedefinieerd kunnen worden.
* **Pagina-vergrendeling:** De controller maakt het mogelijk om pagina's te vergrendelen en te ontgrendelen, zodat beheerders kunnen voorkomen dat ongewenste wijzigingen worden aangebracht.
* **Replicatie van pagina's:** Met de `getCopy`-methode kunnen pagina's en hun gekoppelde content eenvoudig worden gedupliceerd.
* **Templatebeheer:** De controller beheert beschikbare `pagina`-templates, inclusief filtering en sortering, en biedt integratie met specifieke resolvers.
* **Menu-integratie:** Pagina's kunnen worden gekoppeld aan menu's, waarbij ook de structuur van de menu's beheerd kan worden.

## ContentManagerModule

De `ContentManagerModule` en de bijbehorende view, voorzien het CMS van de gebruikersinterface voor het beheren van de pagina's binnen het CMS. Binnen deze interface is het mogelijk om CRUD-operaties voor pagina's uit te voeren.
De weergave van de module is gedefinieerd in een **blade**-template, waarin de pagina's en de paginastructuur beheerd kunnen worden aan de hand van een drag-and-drop-interface. Afhankelijk van de gebruikersrechten worden de bewerkings- en verwijderopties getoond, naast de opties om pagina's te bekijken of te kopiëren.

## Page (Eloquent model)

Het `Page`-model kan gezien worden als het belangrijkste model binnen de websites die gebruik maken van het AllesOnline CMS. Dit model is verantwoordelijk voor het persisteren van de pagina's binnen een website en fungeert als de basisstructuur waarop de inhoud van de website wordt opgebouwd. Binnen het model worden belangrijke gegevens zoals de naam van de pagina, de URL en het toegepaste `Template` opgeslagen. Aan de hand van een `Template` wordt bepaald welke content aan een pagina kan worden toegevoegd. Deze content wordt op zijn beurt gepersisteerd via een polymorfe relatie tussen objecten die content aanbieden en het `CmsContent`-model. 

> _Een ERD met betrekking tot het `Page`-model en CMS content_:
> * [ERD AllesOnline CMS page model](../Bijlagen/ErdAoCmsPageModel.md)

## Templates

Binnen een project dient een eerder genoemde `Template` als het schema dat voorschrijft welke gegevens via het CMS aan een pagina meegegeven kunnen worden. Een `Template` wordt gedefinieerd aan de hand van een XML-bestand. Binnen een `Template` worden velden gedefinieerd die op hun beurt verwijzen naar `FormField`-modules.

Binnen een `Template` is het ook mogelijk om te verwijzen naar andere schema's waarin een samenvoeging van `FormField`-modules beschikbaar wordt gesteld. Deze schema's worden binnen het CMS `Blocks` genoemd. Aan de hand van een `Block`,kunnen gegevens voor complexere elementen binnen een website op een gemakkelijke manier herhaaldelijk beschikbaar worden gesteld. Daarnaast is het ook mogelijk om de volgorde van de `Blocks` en daarmee de elementen op de website te ordenen.

## FormFields

De `FormField`-modules en bijbehorende views voorzien het CMS van interfaces waarmee eindgebruikers gegevens kunnen invoeren of bewerken. Denk hierbij aan invoervelden voor tekst, selectievelden voor selecteren van opties of een uploadveld voor bestanden. Ook de weergave van de `FormFields` wordt gedefinieerd aan de hand van **blade**-templates.

# Beheren van objecten

Naast het beheren van pagina's met content, biedt het AllesOnline CMS ook de mogelijkheid om, naast `Page`-objecten, andere objecten te beheren die binnen een website worden gebruikt. Denk hierbij aan producten in een webshop of workshops op een website die opleidingen aanbiedt.

## ObjectManagerController

De `ObjectManagerController` is verantwoordelijk voor het beheren van objecten. Deze controller breidt de standaard Laravel-controller uit en biedt functionaliteit voor acties zoals het bewerken en verwijderen van objecten, het beheren van relaties tussen modellen en het afhandelen van permissies en autorisatie. Daarnaast bevat de controller ook logica voor acties zoals het sorteren, zoeken en ordenen van objecten binnen het CMS.

De `ObjectManagerController` is een uitgebreide controller die de basis vormt voor objectbeheer in het CMS. Deze breidt de basis Laravel-controller uit met functionaliteit voor CRUD-operaties, beheer van relaties, permissies, zoek- en sorteerfuncties, Excel-exports en het renderen van `Templates`. De controller biedt een flexibel fundament voor zowel synchrone als asynchrone (AJAX) interacties met objecten in het systeem.

## Eloquent modellen

Voor de representatie van objecten in het CMS wordt gebruikgemaakt van `Eloquent`-modellen. `Eloquent` is een ORM dat onderdeel is van Laravel en het mogelijk maakt om op een eenvoudige, objectgeoriënteerde manier met gegevens van objecten te werken.

## Schema's

Binnen de repository van een project worden aan de hand van XML-bestanden schema's voor objecten gedefinieerd. Deze schema's dienen als blauwdruk voor de weergave en het beheer van objecten in het CMS. Binnen deze schema's worden velden gedefinieerd die, net zoals bij een `Template`, verwijzen naar `FormField`-modules. Net zoals in een `Template` kunnen objecten ook `Blocks` toevoegen. Deze worden met een polymorfe relatie gepersisteerd onder het `CmsContent`-model.

In een objectschema kunnen ook relaties tussen objecten gedefinieerd worden, die op een detailweergave van een object informatie van gerelateerde objecten in een tabel weergeven.

# Evaluatie van het AllesOnline CMS

Hoewel het AllesOnline CMS een goed fundament biedt voor het ontwikkelen van websites en webapplicaties, heeft het systeem ook zijn beperkingen op het gebied van een goede developer experience, onderhoudbaarheid en uitbreidbaarheid.

## Gebrekkige documentatie

Een van de grootste gebreken van het AllesOnline CMS is de beperkte en verouderde documentatie. Hoewel er documentatie beschikbaar is, ontbreekt het vaak aan concrete informatie over de werking van bepaalde modules en functionaliteiten. In sommige gevallen is er zelfs helemaal geen informatie beschikbaar over specifieke functionaliteiten.

> _Een voorbeeld van documentatie vind je in deze bijlage:_
>  * [voorbeeld documentatie AllesOnline CMS](../Bijlagen/VoorbeeldAllesOnlineCmsSchema.md)

**Aanbeveling**:
* Zorg voor correcte documentatie, inclusief een up-to-date beschrijving van alle beschikbare modules, met voorbeelden van hoe functionaliteiten in verschillende scenario’s gebruikt kunnen worden. Dit voorkomt onduidelijkheid en zorgt ervoor dat developers zich kunnen concentreren op het realiseren van de requirements van klanten.

## Complexe codestructuur

De huidige codestructuur van de modules is vaak onnodig complex. Dit komt onder andere door inconsistentie in naming conventions, relatief lange methoden met te veel verantwoordelijkheden, hardcoded strings die worden gebruikt als flag en te veel objecten die worden geïnstantieerd in plaats van geïnjecteerd.

**Gevolgen van deze complexiteit:**
* De kans op bugs neemt toe, omdat wijzigingen in een deel van de module vaak onbedoeld andere onderdelen kunnen beïnvloeden.
* Het ontbreken van duidelijke interfaces tussen modules beperkt de mogelijkheid om de applicatie uit te breiden of componenten opnieuw te gebruiken.
* De onderhoudbaarheid en testbaarheid van de modules worden belemmerd, wat leidt tot hogere ontwikkelings- en beheerkosten.

**Aanbeveling:**
* Een grondige refactoring van de modules, waarbij verantwoordelijkheden worden opgesplitst in afzonderlijke classes of componenten, bijvoorbeeld:
	* Een service class voor dataverwerking.
	* Een repository voor database-interactie.
	* Een aparte module voor UI-gerelateerde configuratie.
* Dit vereist het definiëren van duidelijke interfaces tussen deze onderdelen, wat zorgt voor betere herbruikbaarheid, testbaarheid en uitbreidbaarheid van de applicatie.

## SOLID-principes

Het vorige punt stipte dit onderwerp al aan. Als we naar de  **SOLID-principes** kijken, blijkt uit de codebase dat deze principes niet zijn toegepast. Ondanks het niet verplicht is om strict aan deze principes vast te houden, zijn het sterke concepten om een codebase onderhoudbaar, uitbreidbaar en testbaar te maken (DigitalOcean, z.d.). 

* **Single Responsibility Principle (SRP)**: Dit principe stelt dat een class slechts één reden tot verandering mag hebben. In het AllesOnline CMS zijn er veel classes waarvan de methoden verantwoordelijk zijn voor meerdere functionaliteiten. Dit verhoogt de complexiteit en maakt het moeilijker om methoden te isoleren, te testen en opnieuw te gebruiken.
    
* **Open/Closed Principle (OCP)**: Dit principe stelt dat classes open moeten zijn voor uitbreiding, maar gesloten voor aanpassing. Dit betekent dat de functionaliteit van een class uitgebreid kan worden zonder de bestaande code te wijzigen. In de huidige codebase vereisen nieuwe functionaliteiten echter vaak wijzigingen in de bestaande code, wat de kans op bugs vergroot en de onderhoudbaarheid vermindert.

* **Liskov Substitution Principle (LSP):** Dit principe stelt dat objecten van een afgeleide class zonder problemen kunnen worden vervangen door objecten van de parent class.

* **Interface Segregation Principle (ISP):** Dit principe stelt dat een class niet gedwongen moet worden om afhankelijk te zijn van interfaces die het niet gebruikt.
    
* **Dependency Inversion Principle (DIP)**: Dit principe stelt dat modules op een hoger niveau afhankelijk moeten zijn van abstracties zoals bijvoorbeeld interfaces, in plaats van van concrete implementaties. In het AllesOnline CMS worden veel functionaliteiten direct afhankelijk gemaakt van concrete implementaties. Dit belemmert de flexibiliteit en maakt het moeilijker om componenten te vervangen of aan te passen zonder invloed op andere delen van het systeem te hebben. Door gebruik te maken van abstracties kunnen modules op een hoger niveau onafhankelijk werken van lagere, concrete modules, wat de onderhoudbaarheid en uitbreidbaarheid van het systeem verbetert.
    
**Aanbevelingen** 
* **Refactoring van de codebase**: Refactor de codebase stapsgewijs om de SOLID-principes te introduceren. Begin op een hoog niveau en werk stapsgewijs naar lagere niveaus.
* **Introduceer abstracties**: Verminder de afhankelijkheid van concrete implementaties door interfaces te gebruiken. Dit verhoogt de flexibiliteit van het systeem en maakt uitbreiding eenvoudiger. 
* **Testing en Continuous Integration**: Realiseer parallel aan de refactoring een suite van unit- en integratietests en implementeer een Continuous Integration (CI)-ontwikkelstraat om regressies te minimaliseren.
* **Iteratieve verbeteringen**: Voer veranderingen iteratief door en evalueer regelmatig de impact op de onderhoudbaarheid, uitbreidbaarheid en testbaarheid van het systeem.
* **Documentatie en training**: Zorg voor documentatie van de nieuwe architectuur en richtlijnen, en moedig developers aan zich te verdiepen in de SOLID-principes.

# Conclusie

Het is duidelijk dat het AllesOnline CMS voldoende punten heeft die aangepakt moeten worden om het systeem te verbeteren. Het is echter belangrijk te begrijpen dat het verbeteren van de codebase en het introduceren van de SOLID-principes een langlopend project is, dat een aanzienlijke investering in tijd en middelen vereist.

Mocht er gekozen worden om het AllesOnline CMS te verbeteren, dan is het aan te raden deze wijzigingen gefaseerd door te voeren. Dit is noodzakelijk omdat er niet alleen geïnvesteerd moet worden in de ontwikkeling van nieuwe modules, maar ook in het opstellen van uitgebreide documentatie, het implementeren van geautomatiseerde tests en het trainen van het developmentteam in het toepassen en begrijpen van de SOLID-principes.

Bij de keuze voor het refactoren van het AllesOnline CMS is het belangrijk de balans te vinden tussen de investering die AllesOnline bereid is te doen en de voordelen van het uitwijken naar een systeem van een externe partij. Hierbij moet ook gerealiseerd worden dat het proces niet eindigt na het refactoren van het AllesOnline CMS, maar dat deze ook goed onderhouden worden.

### Literatuurlijst
>DigitalOcean. (z.d.). SOLID: The First 5 Principles of Object Oriented Design. [https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design](https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)

