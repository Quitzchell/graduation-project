# SWOT: Statamic

>_Voor meer informatie over het Statamic:_
> * [Onderzoeksdocument over Statamic](./OnderzoekNaarStatamicCMS.md)

## Strengths (Sterktes)

* **Flexibiliteit in datamanagement**: Ondersteuning voor verschillende vormen van gegevenspersistentie, zowel via flat files als via databaseconfiguraties (zoals de Eloquent-driver en de Runway-addon).
* **Integratie met Laravel**: Statamic is gebouwd op Laravel, een framework waarmee alle developers binnen AllesOnline bekend zijn.
* **Uitgebreide functionaliteiten**: Veel out-of-the-box ondersteuning voor onder andere zoekfunctionaliteit, navigatiebeheer en mediabeheer.
* **Marketplace voor extensies**: Beschikbaarheid van gratis en betaalde addons, met mogelijkheden om zelf addons te ontwikkelen.
* **Schaalbaarheid**: De mogelijkheid om te schalen van kleinere projecten naar complexere systemen met ondersteuning voor databases.
* **Kosten**: Redelijk geprijsd met voordelige, schaalbare licentieopties (Master License en Platform Subscription).

## Weaknesses (Zwaktes)

* **Incompleet documentatie**: Er is weinig informatie beschikbaar als je buiten de gebaande paden wilt treden of wijzigingen wilt aanbrengen buiten de Statamic UI. Voor veel informatie moet verder gezocht worden, bijvoorbeeld in GitHub-issues of de Discord-community.
* **Gebruik van Vue 2**: De views van Statamic zijn momenteel gebouwd met Vue 2, dat in 2023 al EOL was. Begin 2025 migreert Statamic naar Vue 3. Hoewel dit positief is, kan het betekenen dat bepaalde custom inputvelden die door gebruikers zelf zijn ontwikkeld, ook omgezet moeten worden om te blijven functioneren.
* **Complexiteit bij configuraties**: Grote verschillen tussen flat-file/eloquent-driver configuratie, en Runway configuratie maken migraties meer hands-on.
* **Beperkingen in databaseconfiguraties**:
    - Geen ondersteuning voor eager loading in Eloquent-driver.
    - Complexe migraties en beperkte relatieondersteuning (bijvoorbeeld pivot-gegevens).
* **Afhankelijkheid van externe ontwikkelaars**: Functionaliteit zoals de Runway-addon is ontwikkeld door derden en heeft lagere prioriteit bij updates.

## Opportunities (Kansen)

* **Vergroting van de flexibiliteit met addons**: Statamic heeft een speciale marktplaats beschikbaar gesteld waar plugins van derden beschikbaar zijn en eventueel oplossingen door AllesOnline - eventueel tegen betaling - aangeboden kunnen worden. 
## Threats (Bedreigingen)

* **Onbekende technieken voor AllesOnline developers**: Een risico voor AllesOnline is dat Statamic voor de views binnen het CMS gebruik maakt van Vue. Dit framework is voor vrijwel alle developers binnen AllesOnline onbekend.
- **Afgebakend**: Statamic is in de basis ontworpen als CMS. Hoewel het mogelijk is om het systeem aan te passen voor gebruik als CRM-systeem, is Statamic niet in dezelfde mate flexibel als bijvoorbeeld Filament.
* **Beperkingen bij complexe relaties**: De flat-file-architectuur en de eloquent-driver bieden onvoldoende ondersteuning voor complexe relaties, wat extra logica vereist, waardoor het systeem complexer kan worden dan noodzakelijk is.