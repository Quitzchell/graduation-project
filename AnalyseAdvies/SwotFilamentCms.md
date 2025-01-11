# **SWOT: Filament**

>_Voor meer informatie over het Filament:_
> * [Onderzoeksdocument over Filament](./OnderzoekNaarFilament.md)

# Strengths (Sterktes)

* **Intuïtieve gebruikersinterface**: Filament biedt een breed scala aan flexibele componenten die zorgen voor een gebruiksvriendelijke interface waarin objecten beheerd kunnen worden. Hierdoor is het mogelijk een adminpanel of CMS te realiseren waarmee gebruikers hun websites en/of applicaties eenvoudig kunnen beheren.
* **Integratie met Laravel**: Filament is goed geïntegreerd met Laravel, het framework waarmee alle developers binnen AllesOnline vertrouwd zijn. Hierdoor kan het team van AllesOnline snel vertrouwd raken met het werken met Filament.
* **Modulariteit**: De mogelijkheid om schema's te extraheren en in aparte PHP-classes te definiëren, maakt het mogelijk om schema's te hergebruiken. Daarnaast is het mogelijk om deze geëxtraheerde classes te voorzien van interfaces om bepaalde functionaliteiten, zoals het beschikbaar stellen van het schema, op een uniforme manier te integreren.
* **Uitbreidbaarheid**: Omdat de componenten van Filament in PHP gedefinieerd worden, is het mogelijk om voor `Fields`, `Columns` en `Entries` inline onder andere de queries voor relatievelden uitbreiden, validaties toevoegen of velden reactief maken. 
* **Autocompletion**: Omdat de componenten van Filament in PHP gedefinieerd worden, profiteren developers in moderne IDE's van autocompletions. 

# Weaknesses (Zwaktes)

* **Beheren van hiërarchische gegevens**: Het beheren van hiërarchische gegevens, zoals gerealiseerd in het AllesOnline CMS, is niet standaard inbegrepen in Filament. Hiervoor moet een oplossing worden gerealiseerd.
* **Beperkte community-ondersteuning en documentatie**: Omdat Filament nog relatief nieuw is, kan het zijn dat een oplossing voor een edgecase niet direct beschikbaar is vanuit de documentatie.

# Opportunities (Kansen)

* **Vergroting van de flexibiliteit met plugins**: Filament heeft een marketplace met gratis en betaalde `Plugins`, ontwikkeld door het Filament-team en externe partijen. AllesOnline kan eigen componenten aanbieden via de marketplace.
* **Actieve open source community:** De actieve open source community van Filament biedt niet alleen ondersteuning maar ook mogelijkheden voor samenwerking, zoals het bijdragen aan nieuwe features, verbeteren van documentatie, of oplossen van bugs. (FilamentPHP, 2025).

# Threats (Bedreigingen)

* **Onbekende technieken voor AllesOnline developers**: Een risico voor AllesOnline is dat Filament voor de werking van zijn componenten gebruikt maakt van Livewire en Alpine.js. Deze twee technieken zijn voor de developers van AllesOnline nog relatief onbekend.
* **Kosten van maatwerkontwikkeling**: Filament is goed voor eenvoudige tot middelgrote projecten, maar voor organisaties die uitgebreide, complexe structuren nodig hebben, kunnen de kosten voor maatwerk en de tijdsinvestering in aanpassingen oplopen.

# Literatuurlijst
> FilamentPHP. (2025). Filament [Software repository]. GitHub. [https://github.com/filamentphp/filament](https://github.com/filamentphp/filament)