# SWOT: Filament

>_Voor meer informatie over het Filament:_
> * [Onderzoeksdocument over Filament](./OnderzoekNaarFilament.md)

## Strengths (Sterktes)

* **Intuïtieve gebruikersinterface**: Filament biedt met zijn standaard library aan componenten breed scala aan een gebruiksvriendelijke interface voor het beheren van content en objecten. Dit maakt het mogelijk om een eenvoudig te gebruiken CMS te faciliteren waarmee gebruikers hun websites kunnen beheren.
* **Integratie met Laravel**: Filament is gebouwd op Laravel, een framework waar alle developers binnen AllesOnline bekend mee zijn. Dit zou betekend dat een groot deel van de logica binnen Filament voor het AllesOnline team vertrouwd zou moeten aanvoelen.
* **Modulariteit van templates**: De mogelijkheid om templates op te bouwen met herbruikbare content-secties biedt flexibiliteit bij het ontwerpen van templates voor paginas en maakt het gemakkelijk om consistentie te behouden door de site heen.
* **Modulariteit van FormField**: De FormFields die worden gebruikt om schema's voor het beheren van objecten en content te definiëren, zijn flexibel doordat ze in PHP worden gedefinieerd. Dit maakt het eenvoudig om bijvoorbeeld de opties voor een selectieveld uit een relatie op te halen. De query om de opties op te halen kan zelfs bewerkt worden via een query of de Eloquent-builder.

## Weaknesses (Zwaktes)

* **Andere werkwijze nodig voor het beheren van menu's en de websitestructuur**: Het beheren van menu's en de websitestructuur zoals dit in het AllesOnline CMS is gerealiseerd, is niet direct mogelijk in Filament. Dit betekent echter niet dat er geen oplossing voor gevonden kan worden, maar dat er tijd gestoken moet worden in het onderzoeken hoe dit in een CMS met Filament beheert kan worden.
* **Beperkte community-ondersteuning en documentatie**: Omdat Filament relatief nieuw is, is de ondersteuning vanuit de community relatief gezien nog in ontwikkeling. Daarnaast kan het zijn dat de documentatie mogelijk nog niet voldoende op niveau is om oplossingen voor complexere problemen te duiden.

## Opportunities (Kansen)

* **Uitbreiding van community-ondersteuning**: Naarmate de community rondom Filament groeit, zal waarschijnlijk ook de library en de documentatie verbeteren. Uit een recente enquête, de **State of Laravel**, blijkt dat Filament erg populair is onder de 4568 ondervraagden. _De resultaten van de enquete lees je hier: [State of Laravel, Administration Panel Results](https://stateoflaravel.com/results#question:administration+panel)_
* **Vergroting van de flexibiliteit met plugins**: Filament heeft een speciale marktplaats beschikbaar gesteld waar plugins van derden beschikbaar zijn en eventueel oplossingen door AllesOnline - eventueel tegen betaling - aangeboden kunnen worden. 

## Threats (Bedreigingen)

* **Onbekende technieken voor AllesOnline developers**: Een risico voor AllesOnline is dat Filament voor de werking van zijn componenten gebruikt maakt van Livewire en Alpine.js. Deze twee technieken zijn voor de developers van AllesOnline nog relatief onbekend.
* **Kosten van maatwerkontwikkeling**: Filament is goed voor eenvoudige tot middelgrote projecten, maar voor organisaties die uitgebreide, complexe structuren nodig hebben, kunnen de kosten voor maatwerk en de tijdsinvestering in aanpassingen snel oplopen.