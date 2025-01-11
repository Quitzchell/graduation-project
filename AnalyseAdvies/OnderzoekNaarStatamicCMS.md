# **Onderzoek naar het Statamic CMS**

# Inleiding

Dit document beschrijft de analyse van Statamic voor het opzetten van een CMS voor de websites die AllesOnline realiseert. Statamic is een commercieel CMS dat out-of-the-box functionaliteiten biedt waar developers relatief snel een CMS kunnen opzetten. Deze analyse is uitgevoerd als onderdeel van een *Available Product Analysis*.

> _Andere onderdelen van het Available Product Analysis:_
> * [Onderzoek naar AllesOnline CMS](../AnalyseAdvies/OnderzoekNaarHetAOCms.md)
> * [Onderzoek naar Filament](../AnalyseAdvies/OnderzoekNaarFilament.md)

> _De belangrijkste bevindingen over het Statamic zijn samengebracht in een SWOT-analyse._
>  * [SWOT: CMS met Statamic](./SwotStatamicCms.md)

# Over Statamic

Omdat **Statamic** developers de mogelijkheid biedt om eenvoudig te schakelen tussen verschillende configuraties, kan het CMS op diverse manieren worden opgezet. Hoewel Statamic geïntegreerd is met **Laravel**, maakt niet elke configuratie op dezelfde manier gebruik van de **Eloquent ORM**.

In Statamic zijn `Collections` containers waarin gerelateerde `Entries` worden gegroepeerd. Denk bijvoorbeeld aan pagina's, blogposts of films. Je zou `Collections` kunnen vergelijken met een repository voor vergelijkbare entiteiten binnen een systeem, omdat ze een manier bieden om gestructureerde gegevens te organiseren. 

De basisinstallatie, die gebruikmaakt van de `flat-file` configuratie, biedt direct een volledig CMS met een breed scala aan componenten voor het beheren van content en objecten. In alle configuraties die in dit onderzoek worden besproken, zijn de volgende modules beschikbaar:
* **Assets**: Beheer van mediabestanden zoals afbeeldingen, video's en documenten.
* **Blueprints**: Schema's die bepalen welke `Fields` beschikbaar zijn bij het aanmaken of bewerken van inhoud in het CMS.
* **Collections**: Groepen van gerelateerde inhoudsitems (`Entries`) die samen een bepaald type content vormen.
* **Entries**:  Individuele inhoudsitems.
* **Fields**: Diverse soorten invoervelden.
* **Globals**: Herbruikbare informatie die globaal kan worden opgehaald.
* **Search**: Standaardondersteuning voor zoekfunctionaliteit, zowel out-of-the-box als via integratie met Algolia.
* **Navigation**: Beheer en organisatie van navigatiemenu's via een gebruiksvriendelijke drag & drop-interface.
* **Taxonomies**: Functionaliteit voor het categoriseren en taggen van content.
* **Users**: Beheer van gebruikersgegevens en -rechten.

> _Voor uitgebreide informatie over het ontwikkelen met Statamic kan de documentatie geraadpleegd worden._
> * [Statamic Documentatie](https://statamic.dev/)

> _Er is ook een videocursus beschikbaar van de maker Jack McDade via Laracasts_
> * [Laracast cursus](https://laracasts.com/series/learn-statamic-with-jack)


# Verschillende configuraties

Statamic biedt redelijk wat flexibiliteit bij het persisteren en beheren van content en objecten. Standaard maakt het systeem gebruik van een `flat-file` configuratie, maar het is mogelijk om Statamic te configureren zodat het geïntegreerd kan worden met databases zoals **MySQL** of **MongoDB**.

## Flat-file
De flat-file configuratie biedt diverse voordelen. Ten eerste wordt er geen gebruik gemaakt van een externe database, wat resulteert in een vereenvoudigde infrastructuur. In plaats daarvan worden alle gegevens en configuraties opgeslagen als bestanden in de codebase en meegenomen in de Git-repository. Omdat er geen verzoeken naar een externe database nodig zijn, zorgt dit voor snellere laadtijden. 

Deze configuratie kent ook nadelen. Ten eerste is de schaalbaarheid beperkt: bij grotere projecten met veel gegevens kunnen de prestaties achterblijven ten opzichte van een configuratie met een database. Daarnaast kunnen er conflicten optreden wanneer meerdere gebruikers tegelijkertijd aan bestanden werken. Tot slot biedt deze configuratie beperkte mogelijkheden voor complexe relaties. Zo worden many-to-many-relaties bijvoorbeeld niet automatisch bi-directioneel verwerkt.

Voor grotere projecten die hogere schaalbaarheid vereisen, complexere relaties bevatten, of waarbij meerdere gebruikers tegelijk bewerkingen uitvoeren, is het dus verstandig om gebruik te maken van één van de configuraties die gebruik maakt van een database. 

## Eloquent-driver
Met de eloquent-driver packages kan Statamic worden geconfigureerd om gegevens via Laravel's **Eloquent ORM** in een database op te slaan. In deze opzet is het echter niet mogelijk om direct de entiteiten in het systeem als Eloquent-modellen te gebruiken. In plaats daarvan worden de Entries, die voorheen in Markdown-bestanden werden opgeslagen, nu als JSON-objecten in de database gepersisteerd.

Het omzetten van de flat-file configuratie naar de eloquent-driver configuratie is relatief eenvoudig. De eloquent-driver package biedt namelijk functionaliteiten die het omzetten van flat-files naar database-records en vice versa eenvoudig maken. Hoe de eloquent-driver binnen het systeem precies wordt ingezet, kan worden afgestemd in het configuratiebestand van de driver. 

> * [Eloquent-driver documenatie](https://github.com/statamic/eloquent-driver)
> * [Eloquent-driver configuratiebestand](../Bijlagen/eloquent-driver-config.md)

# Beheren van content en objecten
## Collection

In Statamic zijn `Collections` containers waarin gerelateerde `Entries` worden gegroepeerd. Denk bijvoorbeeld aan pagina's, blogposts of films. Je zou `Collections` kunnen vergelijken met een repository voor vergelijkbare entiteiten binnen een systeem, omdat ze een manier bieden om gestructureerde gegevens te organiseren. 

`Collections` kunnen via het Control Panel van Statamic of aan de hand van YAML-bestanden in de codebase gedefinieerd worden. In de eloquent-driver configuratie kan het configuratiebestand van een `Collection` in de database gepersisteerd worden.

> _Voorbeeld van een Collections YAML_
> * [Collections yaml voor Pages collection](../Bijlagen/VoorbeeldStatamicCollectionsFile.md)

## Blueprint

Binnen de `Collections` worden `Blueprints` gebruikt om de schema's voor `Entries` te definiëren. De structuur hiervan kan zowel via het Control Panel als in de codebase worden gedefinieerd. De schema's van Blueprints worden opgemaakt met de door Statamic aangeboden `FieldType` componenten (invoervelden).

Het gebruik van een **template** voor de verschillende pagina's werkt in Statamic anders dan in het **AllesOnline CMS** en **Filament**. In tegenstelling tot aparte schema's kun je binnen Statamic meerdere `Blueprints` onder de `Page`-collection definiëren. Deze fungeren dan als templates voor de pagina's.

Binnen een `Blueprint` is het mogelijk om te verwijzen naar andere schema's waarin een samenstelling van `FieldTypes` beschikbaar zijn. Deze schema's worden in Statamic gedefinieerd als `FieldSets` en kunnen ook in dit CMS worden geordend.

Met de Eloquent-driver configuratie kan het configuratiebestand voor een `Blueprint` in de database worden opgeslagen.

## FieldSet

`Fieldsets` zijn voorgedefinieerde, gegroepeerde `FieldTypes` en bieden de mogelijkheid om op een consistente manier herbruikbare contentblokken te creëren die eindgebruikers eenvoudig kunnen toevoegen en ordenen.

## Entry

De `Entry`-class is het model dat wordt gebruikt om alle objecten binnen een collectie te representeren. Deze class biedt mogelijkheden om de gegevens van een `Entry` te persisteren. 

In het geval dat de flat-file configuratie wordt gebruikt, worden de bestanden als markdown in de codebase opgeslagen in de directory van de bijbehorende `Collection`. Wanneer de eloquent-driver configuratie toegepast is, wordt de informatie van een Entry als JSON in de database gepersisteert. 

> _Voorbeeld van Entry binnen Page collection voor de homepage_
> * [Markdown voor homepage](../Bijlagen/VoorbeeldStatamicFlatFile.md)

# Runway

Indien het nodig is om voor entiteiten binnen een systeem specifieke Eloquent-modellen en databasetabellen te gebruiken, kan een configuratie met de `Runway`-addon worden toegepast. Deze addon is ontwikkeld door **The Rad Pack**, een groep developers die nauw samenwerken met het Statamic-team. Met `Runway` kun je Eloquent-models in Statamic weergeven en beheren.

## Runway

Het omzetten van de flat-file of eloquent-driver configuratie naar Runway vergt meer werk. Allereerst moeten alle entiteiten die als Eloquent-model beheerd moeten worden, worden aangemaakt. Deze moeten vervolgens binnen het configuratiebestand worden meegegeven. Daarna is het belangrijk om databasemigraties op te zetten, waarbij rekening gehouden moet worden met de door Runway geaccepteerde datatypes voor de `FieldTypes` die in de `Blueprint` zijn gedefinieerd. In het geval dat er gebruik gemaakt is van relaties tussen objecten, moet de `FieldTypes` voor het beheren hiervan in de `Blueprint` worden vervangen door specifieke `Runway FieldTypes`.

Daarnaast zal er een workaround geimplementeren moeten worden om de functionaliteit voor het beheren van pagina te behouden. Bijvoorbeeld door de blueprints niet om te zetten naar Runway-recources, maar door de blueprints om te zetten naar FieldSets die doormiddel van een `replicator` geselecteerd kan worden.

> * [Runway addon documentatie](https://runway.duncanmcclean.com/)
> * [Runway configuratie](../Bijlagen/RunwayConfigFile.md)

#### Beperkingen Runway

* **Beperkte ondersteuning voor bepaalde veldtypen**: niet alle veldtypen die beschikbaar zijn in Statamic, wat inhoudt dat er naast de documenatie van Statamic ook goed naar de documentatie van Runway gekeken moet worden. 
* **Geen ondersteuning voor pivot-gegevens in many-to-many-relaties**: Bij het werken met many-to-many-relaties biedt Runway geen mogelijkheid om pivot-gegevens mee te geven.
* **Ontbreken van ondersteuning voor polymorfe relaties**: Geen ondersteuning voor polymorfe relaties, wat inhoudt dat het niet mogelijk is om relaties te definiëren waarbij een model meerdere andere models kan associëren. 
* **Out-of-the-box navigation functionaliteit vervalt**: De navigation die Statamic aanbied kan enkel gebruik maken van entries in Collections. Dit betekend dat alle gegevens die via Runway worden gepersisteerd niet meegenomen kunnen worden in de standaard navigation funcionaliteit van Statamic.
* **Afhankelijkheid van externe ontwikkelaars**: Runway is ontwikkeld door een groep externe ontwikkelaars. Hoewel een groot deel van deze ontwikkelaars ook deel uitmaakt van het Statamic-team, geniet dit project toch minder prioriteit. Dit betekent dat men bij problemen of de behoefte aan nieuwe functies afhankelijk is van de beschikbaarheid en bereidheid van The Rad Pack om updates of ondersteuning te bieden.

## Evaluatie van Statamic

### Documenatie en ondersteuning

Hoewel Statamic veel van zijn functionaliteiten beschrijft in de documentatie, is deze vaak oppervlakkig en soms zelfs incompleet. Dit wordt vooral duidelijk wanneer je via code schema's wilt opzetten voor het CMS. In de documentatie worden geen velden gedefinieerd die gebruikt kunnen worden, wat de indruk wekt dat Statamic verwacht dat je hun UI gebruikt voor het beheren van schema's. Echter, bepaalde opties ontbreken, waardoor het alsnog nodig is om via code variabelen voor je schema's mee te geven. Tijdens de ontwikkeling van het prototype heb ik daarom veel informatie over de functionaliteiten uit GitHub-issues moeten halen of heb ik de Discord-server van Statamic geraadpleegd.

**aanbevelingen**: Het is nuttig om actief deel te nemen aan de community van Statamic. Dit kan bijdragen aan het verbreden van kennis over Statamic, maar ook het initieren van wijzigingen voor het platform.

### Gebruik van Vue 2

Statamic maakt gebruik van Vue 2 voor de views in het control panel. Dit is relatief opmerkelijk, aangezien Vue 2 op 31 december 2023 het einde van zijn levenscyclus (EOL) bereikte. Volgens de roadmap van Statamic is het de bedoeling om in de lente van 2025 over te stappen naar Vue 3. Voor ontwikkelaars die custom componenten in Statamic hebben gebouwd, betekent dit dat zij hun componenten ook moeten migreren naar Vue 3.

Daarnaast heeft momenteel slechts één van de ontwikkelaars bij AllesOnline ervaring met het werken met Vue.

**aanbevelingen**: Bied ontwikkelaars de gelegenheid om ervaring en kennis op te doen met Vue.

### Groot verschil tussen werking Runway en flat-file/eloquent-driver

Het verschil tussen de flat-file en eloquent-driver configuratie en Runway is vrij groot. Je zou zelfs kunnen stellen dat dit twee verschillende CMS'en zijn, die bepaalde functionaliteiten en een frontend met elkaar delen. Dit heeft zowel voordelen als nadelen. Want ondanks dat het systeem hierdoor erg flexibel is, heeft elke configuratie een afwijkende structuur, zowel in de manier waarop het CMS achter de schermen werkt als in de manier waarop gegevens voor de frontend opgehaald moeten worden. Het migreren van gegevens tussen de configuraties kan hierdoor erg ingewikkeld zijn, zelfs als je binnen het Statamic-ecosysteem wilt migreren.

## Uitbreidbaarheid van Statamic

Net zoals bij Filament heeft Statamic een marketplace waar oplossingen aangeboden worden die niet standaard in Statamic zijn geïmplementeerd. Deze in Statamic genoemde `Addons`, die zowel gratis als tegen betaling beschickbaar zijn, zijn ook hier ontwikkeld door het Statamic-team of door externe partijen. Ook binnen Statamic is het mogelijk om eigen componenten via de marketplace aan te bieden. Voor de ontwikkeling van deze Addons is het aan te raden om gebruik te maken van Vue, Laravel, Tailwind en Vite. 

> _Documentatie voor het realiseren van Addons voor Statamic_
> * [Addons documentatie](https://statamic.dev/extending/addons)

> _Laracast video over het realiseren van een Statamic Addon_
> * [Laracast video](https://laracasts.com/series/learn-statamic-with-jack/episodes/15)

### Kosten van Statamic CMS
Om gebruik te maken van Statamic voor de websites die AllesOnline ontwikkelt, moet een licentie aangeschaft worden. Statamic biedt verschillende pakketten aan die gekozen kunnen worden. Hieronder een overzicht:

* **Statamic Core**: Deze versie is volledig gratis en geschikt voor persoonlijke projecten.
* **Statamic Pro**: Voor professionele toepassingen is Statamic Pro beschikbaar voor $259 per jaar per site.
* **Master License**: Voor organisaties die meerdere websites beheren, biedt Statamic de Master License aan voor $1250. Dit pakket omvat vijf Statamic Pro-licenties met een korting van 10% en 25% korting op alle extra licenties gedurende één jaar.
* **Platform Subscription**: Voor een groter aantal websites is er een abonnementsmodel beschikbaar, waarbij de kosten per site per maand variëren op basis van het aantal sites. De eerste 25 sites kosten $175 per maand (vast tarief), de volgende 75 sites kosten $7 per maand per site, de volgende 400 sites kosten $6 per maand per site, enzovoorts.

Voor AllesOnline is minimaal de **Statamic Pro** licentie nodig, aangezien de websites voor commerciële doeleinden worden gerealiseerd.

In alle waarschijnlijkheid zullen er, wanneer er voor Statamic gekozen wordt, meerdere websites gebruik van maken. Het is daarom interessant om te starten met de **Master License** en Statamic te implementeren voor de eerste tien websites. Na tien website is het voordeliger om over te schakelen naar de **Platform Subscription**. In dit geval betaal je $2100 per jaar voor de eerste 25 websites, en voor elke extra website daarna een lager tarief. Meer details over deze prijsstaffel zijn te vinden op de website van Statamic. Wanneer er 25 websites in gebruik zijn, komen de kosten per website neer op $84 per jaar.

> * [Informatie over kosten van Platform Subscription](https://statamic.com/pricing/platform)

### Conclusie 

Statamic is in eerste instantie een veelzijdig en gebruiksvriendelijk CMS dat goed integreert met Laravel en nuttige out-of-the-box functionaliteiten biedt. Hoewel de flat-file configuratie interessant kan zijn voor kleinere websites, kunnen we voor AllesOnline ervan uitgaan dat er voornamelijk gebruik zal worden gemaakt van een setup met de Runway-addon. Dit betekent dat de `Navigation` functionaliteit van Statamic vervalt, tenzij deze alsnog als flat-files of entries via de eloquent-driver worden gepersisteerd.

Omdat veel van de werking van Statamic niet goed gedocumenteerd is, is de leercurve voor het opzetten van of migreren naar Statamic stijl. Daarnaast moet er rekening mee worden gehouden dat het team nog onbekend is met het door Statamic aanbevolen **Vue**, waarvan momenteel zelfs nog de EOL-versie Vue 2 wordt gebruikt. Wanneer Statamic  in het voorjaar migreert naar Vue 3, kan dit extra werk opleveren.

De kostenstructuur van Statamic is redelijk, met verschillende licentie-opties die ervoor ervoor zorgen dat de kosten per website daalt naarmate er meer websites gebruik van Statamic gaan maken. 

Statamic biedt dus een flexibele oplossing voor de websites van AllesOnline, maar vereist wel enige voorbereiding met betrekking tot de technische werking. Dit betreft voornamelijk het gebied van persistentie, relaties en de migratie van gegevens. Het is daarom belangrijk om de beperkingen en kosten in overweging te nemen bij het implementeren van dit systeem in alle AllesOnline-projecten.