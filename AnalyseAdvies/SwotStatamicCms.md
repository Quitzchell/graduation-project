# **SWOT: Statamic**

>_Voor meer informatie over het Statamic:_
> * [Onderzoeksdocument over Statamic](./OnderzoekNaarStatamicCMS.md)

## Strengths (Sterktes)

* **Flexibiliteit in datamanagement**: Ondersteuning voor verschillende configuraties voor gegevenspersistentie, zowel via flat-files als via databaseconfiguraties (zoals de Eloquent-driver en de Runway-addon).
* **Integratie met Laravel**: Statamic is geïntegreerd met Laravel, een framework waarmee alle developers bij AllesOnline bekend zijn.
* **Out-of-the-box CMS**: Statamic biedt veel out-of-the-box CMS-functionaliteiten en componenten.
* **Marketplace voor extensies**: Beschikbaarheid van gratis en betaalde addons, met mogelijkheden om zelf addons te ontwikkelen.
* **Schaalbaarheid**: De mogelijkheid om te schalen van kleinere projecten naar complexere systemen door te wisselen tussen configuraties.

## Weaknesses (Zwaktes)

* **Incomplete documentatie**: Er is weinig documentatie beschikbaar wanneer je buiten de gebaande paden wilt treden of wijzigingen wilt aanbrengen die niet via de Statamic UI mogelijk zijn. Hoewel er informatie te vinden is in de GitHub-issues of de Discord-community, is dit omslachtig.
* **Gebruik van Vue 2**: De views van Statamic zijn momenteel gebouwd met Vue 2, dat in 2023 al EOL was. Begin 2025 migreert Statamic naar Vue 3.
* **Complexiteit bij verschillende configuraties**: De verschillen tussen de flat-file/eloquent-driver configuratie en de Runway-addon maken migraties tussen deze configuraties complexer.
* **Beperkingen in databaseconfiguraties**: Het verschil in databasestructuur tussen het AllesOnline CMS en Statamic is groot, waardoor migratie naar Statamic complexer wordt. Daarnaast ontbreken er bepaalde features voor relatiebeheer, zoals het meegeven van pivot-gegevens en polymorfe relaties.
* **Afhankelijkheid van externe ontwikkelaars**: Functionaliteit zoals de Runway-addon is ontwikkeld door derden.

## Opportunities (Kansen)

* **Snellere implementatie voor simpele websites**: Dankzij de vele out-of-the-box CMS-functionaliteiten en de gebruiksvriendelijke interface kan het CMS de implementatietijd aanzienlijk verkorten voor websites die niet veel complexiteit vereisen.
* **Gestandaardiseerde Componenten voor Whitelabel Websites**: Het is mogelijk om een boilerplate voor Statamic te ontwikkelen met gestandaardiseerde componenten. Dit zou de vormgevers van AllesOnline in staat stellen om zelfstandig whitelabel-websites te realiseren.
* **Kostenefficiëntie op de Lange Termijn**: Hoewel de initiële kosten van de Statamic Pro licentie hoger zijn, biedt het Platform Subscription-model een aantrekkelijke oplossing voor grotere aantallen websites. De kosten per website dalen aanzienlijk wanneer meer websites worden toegevoegd, waardoor het op lange termijn kostenefficiënt is voor AllesOnline wanneer er veel Statamic-websites in productie gaan.

## Threats (Bedreigingen)

* **Onbekende technieken voor AllesOnline developers**: Een risico voor AllesOnline is dat Statamic voor de views binnen het CMS gebruik maakt van Vue. Dit framework is voor vrijwel alle developers binnen AllesOnline onbekend.
* **Beperkte scope**: Statamic is oorspronkelijk ontworpen als CMS. Hoewel het mogelijk is om het systeem aan te passen voor gebruik als CRM, biedt Statamic niet dezelfde mate van flexibiliteit als bijvoorbeeld Filament.
