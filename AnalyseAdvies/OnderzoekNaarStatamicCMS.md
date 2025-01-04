# Onderzoek naar het Statamic CMS

## Inleiding

Dit document beschrijft de analyse over het gebruik van Statamic voor het opzetten van een CMS voor de websites die AllesOnline ontwikkelt. Statamic is een commercieel CMS dat veel out-of-the-box functionaliteiten biedt waarmee ontwikkelaars en gebruikers eenvoudig een website kunnen bouwen en beheren.

## Over Statamic

Net zoals het AllesOnline CMS en Filament maakt Statamic gebruik van **Laravel**, waardoor het mogelijk is om de functionaliteiten van Laravel te benutten bij het ontwikkelen van een systeem.

De basisinstallatie van Statamic komt met een volledig CMS waarin een overvloed aan functionaliteiten beschikbaar is voor het beheren van content. De kernonderdelen zijn:

* Assets: voor het beheren van afbeeldingen en video's.
* Collections: voor het opzetten van structuur en het organiseren van content en objecten.
* Entries: records van objecten en/of content.
* Fields: verschillende soorten invoervelden voor content.
* Globals: herbruikbare informatie die globaal kan worden opgehaald.
* Search: out-of-the-box ondersteuning voor zoekfunctionaliteit, zowel on-the-fly als met behulp van Algolia.
* Navigation: beheren van navigatiemenu's via een drag & drop interface.
* Taxonomies: functionaliteit voor het categoriseren en taggen van content voor globale organisatie van content.
* Users: Voor het beheren van gebruikersgegevens.

> _Voor uitgebreide informatie over het ontwikkelen met Statamic kan de officiele documentatie geraadpleegd worden._
> * [Statamic Documentatie](https://statamic.dev/)

> _Er is ook een videocursus beschikbaar van de maker Jack McDade via Laracasts._
> * [Laracast cursus](https://laracasts.com/series/learn-statamic-with-jack)

## Uitbreidbaarheid van Statamic

Net zoals bij Filament heeft Statamic een marketplace waar oplossingen aangeboden worden die niet standaard in Statamic zijn geïmplementeerd. Deze in Statamic genoemde `Addons`, die zowel gratis als tegen betaling beschickbaar zijn, zijn ook hier ontwikkeld door het Statamic-team of door externe partijen. Ook binnen Statamic is het mogelijk om eigen componenten via de marketplace aan te bieden. Voor de ontwikkeling van deze Addons is het aan te raden om gebruik te maken van Vue, Laravel, Tailwind en Vite. 

> _Documentatie voor het realiseren van Addons voor Statamic_
> * [Addons documentatie](https://statamic.dev/extending/addons)

> _Laracast video over het realiseren van een Statamic Addon_
> * [Laracast video](https://laracasts.com/series/learn-statamic-with-jack/episodes/15)

## Beheren van content en objecten

Statamic biedt flexibiliteit rondom het persisteren en beheren van content en objecten. Standaard maakt het systeem gebruik van een `flat-file` architectuur, maar er kan gekozen worden om Statamic te configuereren om gebruik te maken van databases zoals MySQL of MongoDB. 

### Collections 

In Statamic worden `Collections` gebruikt om groepen gerelateerde content-items te organiseren. Elke collectie fungeert als een container voor `Entries`, zoals pagina's, blogposts of films. De entries binnen deze collections maken gebruik van voorgedefinieerde `Blueprints` die de datastructuur bepalen. 

In het geval dat er gebruik gemaakt wordt van de flat-file architectuur, worden alle entries van een collectie onder dezelfde directory in de repository opgeslagen. 

Collections kunnen via het Control Panel van Statamic of aan de hand van YAML-bestanden in de repository gedefinieerd worden.

> _Voorbeeld van een Collections YAML_
> [Collections yaml voor Pages collection](../Bijlagen/VoorbeeldStatamicCollectionsFile.md)

### Blueprints

Zoals bij collections al is benoemd, worden `Blueprints` gebruikt om te definiëren wat de datastructuur voor `Entries` binnen een collection is. Deze structuur wordt bepaald door velden aan de blueprints toe te voegen. Om eindgebruikers in staat te stellen op een dynamische manier gegroepeerde velden voor contentblokken te beheren, kan in Statamic gebruik worden gemaakt van het `Replicator`-veld. Dit stelt gebruikers in staat om onder andere voorgedefinieerde `Fieldsets` aan de content toe te voegen.

### FieldSets

`Fieldsets` zijn voorgedefineerde gegroepeerde velden en bieden de mogelijkheid om op een consistente manier herbruikbare contentblokken te creeren die eindgebruikers eenvoudig kunnen toevoegen en ordenen.

### Entries

In de standaard flat-file configuratie voor Statamic worden `Entries` gebruikt om content als Markdown-bestand te beheren en persisteren binnen de bijhorende collection.  

> _Voorbeeld van entry binnen Page collection voor de hoempage_
> * [Markdown voor homepage](../Bijlagen/VoorbeeldStatamicFlatFile.md)


### Meer over flat-file 

In de standaard flat-file configuratie van Statamic worden `Entries` opgeslagen als Markdown-bestanden binnen de bijbehorende collecties. Deze configuratie biedt enkele voordelen:

* **Geen database**: een vereenvoudigde infrastructuur zonder afhankelijkheid van een database.
* **Snelheid**: directe toegang tot bestanden zorgt voor snellere laadtijden, aangezien er geen database-queries nodig zijn.
* **Versiebeheer**: omdat de bestanden worden meegenomen in de Git-repository, zijn wijzigingen eenvoudig te traceren en terug te draaien.

Uiteraard kent de flat-file configuratie ook enkele nadelen die in overweging moeten worden genomen:

**Beperkte schaalbaarheid**: bij grotere projecten met veel data of complexe queries blijven de prestaties van de flat-file architectuur achter bij die van een geoptimaliseerde database.
* **Conflicten**: als meerdere gebruikers tegelijkertijd bestanden bewerken, kan dit leiden tot mergeconflicten in het versiebeheer.
**Beperktere mogelijkheden voor complexe relaties**: objecten met complexere relaties, zoals many-to-many-relaties, kunnen momenteel niet goed worden ondersteund in de flat-file architectuur van Statamic. Het is mogelijk om via een Observer wijzigingen in beide objecten bij te houden om een many-to-many-relatie te realiseren, maar dat vrij omslachtig.

Voor projecten die een hogere schaalbaarheid vereisen, complexere relaties hebben, of waarbij gelijktijdige bewerkingen door meerdere gebruikers vaker voorkomen, biedt Statamic de mogelijkheid om een configuratie te gebruiken waarin een database wordt ingezet.

### Met Eloquent-driver of Runway

Met de officiële `eloquent-driver` kan Statamic worden geconfigureerd om gegevens via Laravel's Eloquent ORM in een database op te slaan. In deze configuratie is het niet de bedoeling om voor de entiteiten Eloquent-modellen te gebruiken. In plaats daarvan worden de gegevens voor entries, die eerder in Markdown-bestanden werden opgeslagen, nu als JSON in de database gepersisteerd.

Mocht het nodig zijn om specifieke eloquent-modellen en databasetabellen te gebruiken, dan kan de **Runway** addon gebruikt worden. Deze addon is ontwikkeld door *The Rad Pack*, een groep ontwikkelaars die nauw samenwerken met het Statamic-team. Met Runway kun je Eloquent-modellen in Statamic weergeven en beheren.

### Eloquent-driver

Het omzetten van de reguliere Statamic configuratie naar de `eloquent-driver` is relatief eenvoudig. Het installeren van de pakketten en het volgen van de documentatie is voldoende om tot een werkbare oplossing te komen. De pakketten bevatten ook functionaliteit die het eenvoudig maakt om gegevens van flat-files om te zetten naar database-records (en eventueel zelfs weer terug naar flat-files). De manier waarop de eloquent-driver binnen het systeem gebruikt dient te worden, kan geconfigureerd worden in het configuratiebestand voor de eloquent-driver. Het grootste voordeel van de eloquent-driver configuratie is dat deze de schaalbaarheid aanzienlijk verbetert.

> [Eloquent-driver configuratie](https://github.com/Quitzchell/graduation-statamic-cms/blob/eloquent/config/statamic/eloquent-driver.php)

> [Eloquent-driver documenatie](https://github.com/statamic/eloquent-driver)

#### Beperkingen Eloquent-driver

* **Geen ondersteuning voor eager loading**: De Eloquent Driver ondersteunt (nog) geen eager loading van relaties. Er is vanuit het team ook geen plan om dit te realiseren. 
* **Complexiteit bij migraties**: Het beheren van database-migraties kan complexer worden, vooral bij wijzigingen in de structuur en/of naamgeving van content(types).
* **Bi-directionele many-to-many-relaties**: Many-to-many-relaties worden enkel gepersisteerd aan het object dat bewerkt wordt. Eventuele oplossingen hiervoor zijn externe addons of het realiseren van observers die beide objecten voorzien van gegevens over hun relatie.
* **Geen specifieke modellen**: De `Eloquent-driver` slaat alle gegevens op in een enkele data-kolom als JSON. Hoewel het mogelijk is om velden in afzonderlijke kolommen op te slaan, is er geen mogelijkheid om eigen tabellen te creëren die door middel van Eloquent-modellen verantwoordelijk zijn voor het persisteren van de gegevens.

> [Github-issue: eager loading](https://github.com/statamic/eloquent-driver/issues/119)

### Runway

Het omzetten van de reguliere Statamic-configuratie naar Runway vergt, in tegenstelling tot de eloquent-driver, meer wijzigingen. Naast het configuratiebestand dat opgezet moet worden, is het ook nodig om voor alle entiteiten die je als specifieke Eloquent-modellen wilt beheren, Eloquent-modellen aan te maken, Runway-resources te definiëren en databasemigraties op te zetten. Daarnaast is het nodig om een workaround te implementeren om een functionaliteit te realiseren waarmee je gebruik kunt maken van pagina-templates. Bijvoorbeeld door de blueprints niet om te zetten naar Runway-recources, maar door de blueprints om te zetten naar FieldSets die doormiddel van een `replicator` geselecteerd kan worden.

> [Runway configuratie](https://github.com/Quitzchell/graduation-statamic-cms/blob/runway/config/runway.php)

> [Runway addon documentatie](https://runway.duncanmcclean.com/)

#### Beperkingen Runway

* **Beperkte ondersteuning voor bepaalde veldtypen**: niet alle veldtypen die beschikbaar zijn in Statamic, wat inhoudt dat er naast de documenatie van Statamic ook goed naar de documentatie van Runway gekeken moet worden. 
* **Geen ondersteuning voor pivot-gegevens in many-to-many-relaties**: Bij het werken met many-to-many-relaties biedt Runway geen mogelijkheid om pivot-gegevens mee te geven.
* **Ontbreken van ondersteuning voor polymorfe relaties**: Geen ondersteuning voor polymorfe relaties, wat inhoudt dat het niet mogelijk is om relaties te definiëren waarbij een model meerdere andere modellen kan associëren. 
* **Out-of-the-box navigation functionaliteit vervalt**: De navigation die Statamic aanbied kan enkel gebruik maken van entries in Collections. Dit betekend dat alle gegevens die via Runway worden gepersisteerd niet meegenomen kunnen worden in de standaard navigation funcionaliteit van Statamic.
* **Afhankelijkheid van externe ontwikkelaars**: Runway is ontwikkeld door een groep externe ontwikkelaars. Hoewel een groot deel van deze ontwikkelaars ook deel uitmaakt van het Statamic-team, geniet dit project toch minder prioriteit. Dit betekent dat men bij problemen of de behoefte aan nieuwe functies afhankelijk is van de beschikbaarheid en bereidheid van The Rad Pack om updates of ondersteuning te bieden.

## Evaluatie van Statamic

### Documenatie en ondersteuning

Hoewel Statamic veel van zijn functionaliteiten beschrijft in de documentatie, is deze vaak oppervlakkig en soms zelfs incompleet. Dit wordt vooral duidelijk wanneer je via code schema's wilt opzetten voor het CMS. In de documentatie worden geen velden gedefinieerd die gebruikt kunnen worden, wat de indruk wekt dat Statamic verwacht dat je hun UI gebruikt voor het beheren van schema's. Echter, bepaalde opties ontbreken, waardoor het alsnog nodig is om via code variabelen voor je schema's mee te geven. Tijdens de ontwikkeling van het prototype heb ik daarom veel informatie over de functionaliteiten uit GitHub-issues moeten halen of heb ik de Discord-server van Statamic geraadpleegd.

**aanbevelingen**:
- Het is nuttig om actief deel te nemen aan de community van Statamic. Dit kan bijdragen aan het verbreden van kennis over Statamic, maar ook het initieren van wijzigingen voor het platform.

### Gebruik van Vue 2

Statamic maakt gebruik van Vue 2 voor de views in het control panel. Dit is relatief opmerkelijk, aangezien Vue 2 op 31 december 2023 het einde van zijn levenscyclus (EOL) bereikte. Volgens de roadmap van Statamic is het de bedoeling om in de lente van 2025 over te stappen naar Vue 3. Voor ontwikkelaars die custom componenten in Statamic hebben gebouwd, betekent dit dat zij hun componenten ook moeten migreren naar Vue 3.

Daarnaast heeft momenteel slechts één van de ontwikkelaars bij AllesOnline ervaring met het werken met Vue.

**aanbevelingen**:
- Bied ontwikkelaars de gelegenheid om ervaring en kennis op te doen met Vue.

### Groot verschil tussen werking Runway en flat-file/eloquent-driver

Het verschil tussen de standaard/eloquent-driverconfiguratie en Runway is vrij groot. Je zou zelfs kunnen stellen dat dit twee verschillende CMS'en zijn, die bepaalde functionaliteiten en een frontend met elkaar delen. Dit heeft zowel voordelen als nadelen. Want ondanks dat het systeem hierdoor erg flexibel is, heeft elke configuratie een afwijkende structuur, zowel in de manier waarop het CMS achter de schermen werkt als in de manier waarop gegevens voor de frontend opgehaald moeten worden. Het migreren van gegevens tussen de configuraties kan hierdoor erg ingewikkeld zijn, zelfs als je binnen het Statamic-ecosysteem wilt migreren.

### Kosten van Statamic CMS

Om gebruik te mogen van Statamic voor de websites die AllesOnline ontwikkeld, moet een licensie aangeschaft worden. Statamic biedt verschillende pakketen aan die afgenomen kunnen worden. Hieronder vallen:
* **Statamic Core**: Deze versie is volledig gratis en geschikt voor persoonlijke projecten, hobbyisten of kleine websites. 
* **Statamic Pro**: Voor professionele toepassingen is Statamic Pro beschikbaar voor $259 per jaar per site. 
* **Master License**: Voor organisaties die meerdere sites beheren, biedt Statamic de Master License aan voor $1250. Dit pakket omvat vijf Statamic Pro-licenties met een korting van 10% en een korting van 25% op alle extra licenties gedurende één jaar. 
* **Platform Subscription**: Voor grotere aantallen sites is er een abonnementsmodel beschikbaar, waarbij de kosten per site per maand variëren op basis van het aantal sites. De eerste 25 sites kosten $175 per maand (vast tarief), de volgende 75 sites kosten $7 per maand per site, de volgende 400 sites kosten $6 per maand per site, etc.

Voor AllesOnline is op zijn minst de **Statamic Pro** licensie nodig aangezien websites voor commenerciele doeleinden gearliseerd worden. Uiteraard zou **Statamic Pro** niet voldoen voor AllesOnline. Men kan ervan uit gaan dat er meer dan een website gebruik zou gaan maken van de Statamic backend. In eerste instantie zou het interessant zijn om te beginnen met de **Master License** en Statamic eerst te implementeren eerste vijf websites. Na vijf websites is het voordeliger om over te gaan op **Platform Subscription**, hier ben je tot 25 websites met Statamic $2100 per jaar kwijt en betaal je na de eerste 25 een kleine prijs per extra website. Meer over deze staffel is te lezen op de website van Statamic. Op het moment dat er 25 websites zijn die Statamic gebruiken, kost dit per jaar per website $84. 

### Kosten van Statamic CMS
Om gebruik te maken van Statamic voor de websites die AllesOnline ontwikkelt, moet een licentie aangeschaft worden. Statamic biedt verschillende pakketten aan die gekozen kunnen worden. Hieronder een overzicht:

* **Statamic Core**: Deze versie is volledig gratis en geschikt voor persoonlijke projecten, hobbyisten of kleine websites.
* **Statamic Pro**: Voor professionele toepassingen is Statamic Pro beschikbaar voor $259 per jaar per site.
* **Master License**: Voor organisaties die meerdere websites beheren, biedt Statamic de Master License aan voor $1250. Dit pakket omvat vijf Statamic Pro-licenties met een korting van 10% en 25% korting op alle extra licenties gedurende één jaar.
* **Platform Subscription**: Voor een groter aantal websites is er een abonnementsmodel beschikbaar, waarbij de kosten per site per maand variëren op basis van het aantal sites. De eerste 25 sites kosten $175 per maand (vast tarief), de volgende 75 sites kosten $7 per maand per site, de volgende 400 sites kosten $6 per maand per site, enzovoorts.

Voor AllesOnline is minimaal de **Statamic Pro** licentie nodig, aangezien de websites voor commerciële doeleinden worden gerealiseerd. Het is waarschijnlijk dat meerdere websites gebruik gaan maken van Statamic. Het is daarom interessant om te starten met de **Master License** en Statamic te implementeren voor de eerste tien websites. Na tien website is het voordeliger om over te schakelen naar de **Platform Subscription**. In dit geval betaal je $2100 per jaar voor de eerste 25 websites, en voor elke extra website daarna een lager tarief. Meer details over deze prijsstaffel zijn te vinden op de website van Statamic. Wanneer er 25 websites in gebruik zijn, komen de kosten per website neer op $84 per jaar.

> [Prijs informatie Platform Subscription](https://statamic.com/pricing/platform)

### Conclusie 

Statamic is in eerste instantie een veelzijdig en gebruiksvriendelijk CMS dat goed integreert met Laravel en nuttige out-of-the-box functionaliteiten biedt. Hoewel de flat-file configuratie interessant kan zijn voor kleinere websites, kunnen we voor AllesOnline ervan uitgaan dat er voornamelijk gebruik zal worden gemaakt van een setup met de Runway-addon. Dit betekent dat de `Navigation` functionaliteit van Statamic vervalt, tenzij deze alsnog als flat-files of entries via de eloquent-driver worden gepersisteerd.

Omdat veel van de werking van Statamic niet goed gedocumenteerd is, is de leercurve voor het opzetten van of migreren naar Statamic stijl. Daarnaast moet er rekening mee worden gehouden dat het team nog onbekend is met het door Statamic aanbevolen **Vue**, waarvan momenteel zelfs nog de EOL-versie Vue 2 wordt gebruikt. Wanneer Statamic  in het voorjaar migreert naar Vue 3, kan dit extra werk opleveren.

De kostenstructuur van Statamic is redelijk, met verschillende licentie-opties die ervoor ervoor zorgen dat de kosten per website daalt naarmate er meer websites gebruik van Statamic gaan maken. 

Statamic biedt dus een flexibele oplossing voor de websites van AllesOnline, maar vereist wel enige voorbereiding met betrekking tot de technische werking. Dit betreft voornamelijk het gebied van persistentie, relaties en de migratie van gegevens. Het is daarom belangrijk om de beperkingen en kosten in overweging te nemen bij het implementeren van dit systeem in alle AllesOnline-projecten.