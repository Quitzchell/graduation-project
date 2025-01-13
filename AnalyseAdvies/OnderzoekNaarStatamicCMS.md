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

> * [Eloquent-driver documentatie](https://github.com/statamic/eloquent-driver)
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

Daarnaast zal er een workaround geïmplementeerd moeten worden om de functionaliteit voor het beheren van templates te behouden. Bijvoorbeeld door de `Blueprints` van de templates om te zetten naar `FieldSets`. Deze kunnen dan doormiddel van een `replicator` geselecteerd worden.

> * [Runway addon documentatie](https://runway.duncanmcclean.com/)
> * [Runway configuratie](../Bijlagen/RunwayConfigFile.md)

## Beperkingen Runway

* **Geen ondersteuning voor pivot-gegevens in many-to-many-relaties**: Bij het werken met many-to-many-relaties biedt Runway geen mogelijkheid om pivot-gegevens mee te geven.
* **Ontbreken van ondersteuning voor polymorfe relaties**: Geen ondersteuning voor polymorfe relaties, waardoor er geen mogelijkheid is om een model aan meerdere andere modellen te associëren.
* **Out-of-the-box navigation functionaliteit vervalt**: De standaard Statamic-navigatie kan niet rechtstreeks omgaan met Runway-modellen. Het is dus niet mogelijk om Runway-gegevens direct op te nemen in de standaard navigatiefunctionaliteit zonder zelf aangepaste code toe te voegen.
* **Afhankelijkheid van externe ontwikkelaars**: Runway is ontwikkeld door een groep externe ontwikkelaars. Hoewel een groot deel van deze ontwikkelaars ook deel uitmaakt van het Statamic-team, geniet dit project toch minder prioriteit. Dit betekent dat men bij problemen of de behoefte aan nieuwe functies afhankelijk is van de beschikbaarheid en bereidheid van **The Rad Pack** om updates en ondersteuning te bieden.

# Evaluatie van Statamic

## Documenatie en ondersteuning

Hoewel Statamic veel van zijn functionaliteiten beschrijft in de documentatie, is deze vaak oppervlakkig en soms zelfs incompleet. Dit wordt vooral duidelijk wanneer je schema's via de codebase voor het CMS wilt definiëren. In de documentatie worden de velden die gebruikt kunnen worden niet gedefinieerd, wat de indruk wekt dat Statamic verwacht dat je hun UI gebruikt voor het beheren van schema's. Echter, ontbreken bepaalde opties in de UI, waardoor het alsnog nodig is om variabelen via de codebase aan je schema's mee te geven. 

**Aanbeveling**: Moedig developers aan om actief deel te nemen aan de community van Statamic. Deze community is actief en er zijn vaak gedetailleerde voorbeelden te vinden via GitHub-issues of de Discord.

## Gebruik van Vue 2

Statamic maakt gebruik van Vue 2 voor de weergave in het control panel. Dit is opvallend, aangezien Vue 2 op 31 december 2023 het einde van zijn levenscyclus (EOL) bereikte. Volgens de roadmap van Statamic is het de bedoeling om in de lente van 2025 over te stappen naar Vue 3.

Daarnaast heeft momenteel slechts één van de developers bij AllesOnline ervaring met Vue.

**Aanbeveling**: Faciliteer training en kennisdeling om ontwikkelaars in staat te stellen ervaring op te doen met Vue.

## Verschillen tussen werking Runway en andere configuraties

Het verschil tussen de flat-file en eloquent-driver configuraties in Runway is aanzienlijk. Je zou zelfs kunnen zeggen dat het om twee verschillende CMS'en gaat. Aan de ene kant zorgt dit voor flexibiliteit in de manier hoe je Statamic wilt opzetten. Aan de andere kant leidt het tot meer complexiteit omdat de verschillende configuraties specifieke werkwijzen hebben in de manier waarop gegevens uit de backend verwerkt kunnen te worden.

## Afwijkende datastructuren tussen Statamic en AllesOnline

In alle verschillende configuraties wijkt de datastructuur van Statamic af van de datastructuur van het AllesOnline CMS. Deze variaties maken niet alleen het migreren van AllesOnline naar Statamic aanzienlijk complexer dan een migratie naar **Filament**.

**Aanbeveling**: Voor migraties van projecten met het AllesOnline CMS is het verstandiger om te migreren naar een CMS met Filament.

## Uitbreidbaarheid van Statamic

`Statamic` heeft een marketplace voor `Addons`, die extra functionaliteiten bieden en zijn ontwikkeld door het `Statamic`-team of externe partijen. Deze `Addons` zijn zowel gratis als tegen betaling beschikbaar. Het is voor AllesOnline ook mogelijk om zelf ontwikkelde `Addons` op de marketplace aan te bieden. Voor de ontwikkeling van `Addons` wordt het aanbevolen om gebruik te maken van `Vue`, `Laravel`, `Tailwind` en `Vite`.

> _Documentatie voor het realiseren van Addons voor Statamic_
> * [Addons documentatie](https://statamic.dev/extending/addons)

> _Laracast video over het realiseren van een Statamic Addon_
> * [Laracast video](https://laracasts.com/series/learn-statamic-with-jack/episodes/15)

## Kosten van Statamic

Om gebruik te maken van Statamic, moet een licentie aangeschaft worden. Statamic biedt verschillende pakketten aan die gekozen kunnen worden. Hieronder een overzicht:

* **Statamic Core**: Deze versie is volledig gratis en geschikt voor persoonlijke projecten.
* **Statamic Pro**: Voor professionele toepassingen is `Statamic Pro` beschikbaar voor $259 per jaar per website.
* **Master License**: Voor organisaties die meerdere websites beheren, biedt `Statamic` de `Master License` aan voor $1250. Dit pakket omvat vijf `Statamic Pro`-licenties met een korting van 10% en 25% korting op alle extra licenties gedurende één jaar.
* **Platform Subscription**: Voor een groter aantal websites is er een abonnementsmodel beschikbaar, waarbij de kosten per website per maand variëren op basis van het aantal websites. De eerste 25 websites kosten $175 per maand (vast tarief), de volgende 75 websites kosten $7 per maand per website, de volgende 400 sites kosten $6 per maand per website, enzovoorts.

Voor AllesOnline is minimaal de **Statamic Pro** licentie nodig, aangezien de websites voor commerciële doeleinden worden gerealiseerd.

Wanneer er meerdere websites met `Statamic` in productie gebracht worden, is het interessant om gebruik te maken van de **Master License**. Als het lukt om binnen een jaar tien websites in productie te brengen, ben je met dit pakket het goedkoopst uit. Na tien websites, of nadat het jaar waarin je met korting extra licenties kunt kopen voorbij is, is het voordeliger om over te schakelen naar de `Platform Subscription`. Omdat er tot 25 websites een flat fee van $175 per maand geldt, kost dit voor de eerste 25 websites $2100 per jaar. Wanneer er dus 25 websites in gebruik zijn, komen de kosten per website neer op $84 per jaar. Voor elke extra website die hierop volgt geldt een lager tarief. Meer details over deze prijsstaffel zijn te vinden op de website van Statamic.

> * [Informatie over kosten van Platform Subscription](https://statamic.com/pricing/platform)

### Conclusie 

Statamic is een veelzijdig en gebruiksvriendelijk CMS dat goed integreert met Laravel en nuttige out-of-the-box functionaliteiten biedt. Hoewel de flat-file en eloquent-driver configuratie interessant kan zijn voor kleinere websites, kunnen we ervan uitgaan dat het voor AllesOnline niet interessant is om grotere projecten met Statamic te realiseren. Daarnaast is het vanwege de verschillen in datastructuur ook niet verstandig om bestaande projecten met het AllesOnline CMs te migreren naar Statamic.

Veel van de werking van Statamic is bovendien moeilijk terug te vinden in de documentatie, waardoor de leercurve voor het werken met Statamic steiler kan zijn dan noodzakelijk.

Waar Statamic wel erg geschikt zou kunnen zijn, is voor simpele marketing- en whitelabel-websites. Omdat Statamic in de flat-file en eloquent-driver out-of-the-box met de Statamic API gegevens beschikbaar kan stellen, is het mogelijk om met weinig werk een simpel CMS klaar te zetten waarin content beheerd kan worden. 

Omdat de licentiekosten voor een Statamic-website per in productie genomen website lager worden, is het verstandig om een tijdlijn op te stellen waarmee inzichtelijk wordt gemaakt wanneer de kosten per website voor AllesOnline rendabel zijn.