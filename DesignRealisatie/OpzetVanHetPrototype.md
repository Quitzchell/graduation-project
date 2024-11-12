# Opzet van het prototype

# Inleiding

Om verschillende Content Management Systemen (CMS) te vergelijken in gecontroleerde situaties, zal ik in dit project een praktisch experiment uitvoeren. Namelijk, het ontwikkelen van een full-stack prototype met verschillende CMS-pakketten. Dit maakt het eenvoudig om te beoordelen of verschillende functionaliteiten, die in het AllesOnline CMS beschikbaar zijn (of verwacht worden), ook beschikbaar zijn in CMS'en ontwikkeld door derden. Daarnaast maakt dit het mogelijk om te onderzoeken of een migratie van het AllesOnline CMS naar een nieuw pakket (deels) geautomatiseerd kan worden.

Uiteindelijk kunnen we met de inzichten uit deze prototypes bepalen wat de haalbaarheid en efficiëntie zijn van het overstappen naar een ander CMS.

# Prototype Omschrijving

Omdat het prototype dat ontwikkeld wordt niet per se complexe logica hoeft te bevatten, maar vooral de belangrijkste functionaliteiten van het huidige AllesOnline CMS moet omvatten, heb ik gekozen om een fictieve blogwebsite rondom Napoleon te realiseren. Dit concept biedt de mogelijkheid om vrijwel alle essentiële functionaliteiten van een CMS te implementeren, zoals contentbeheer, objectbeheer en gebruikersbeheer. Bovendien maakt dit ontwerp het mogelijk om op een heldere en gestructureerde manier gebruik te maken van dummydata, waardoor geautomatiseerde tests eenvoudiger kunnen worden uitgevoerd. Deze aanpak zorgt voor een consistente testomgeving en biedt waardevolle inzichten in de prestaties, schaalbaarheid en gebruiksvriendelijkheid van de verschillende CMS-pakketten.

# Prototype Specificaties

Een prototype bestaat uit drie verschillende systemen die met elkaar communiceren:

- **Front-end**: Een eenvoudige webinterface die via API-requests kan communiceren met verschillende CMS-backends.
    
- **Back-end**: Er worden meerdere back-end implementaties ontwikkeld, elk gebaseerd op een ander CMS-pakket, waaronder het huidige AllesOnline CMS.
    
- **Database**: Voor het prototype wordt gewerkt met dummydata, gegenereerd door Seeders. 
	
### Functionele eisen
Globaal gezien zal een prototype aan de volgende eisen moeten voldoen. 

- **Contentbeheer**: De mogelijkheid om webpagina's, inclusief content, aan te maken, te bewerken en te verwijderen.
- **Objectenbeheer:** De mogelijkheid om objecten aan te maken, te bewerken en te verwijderen.
- **Gebruikersbeheer**: Functies voor het toevoegen, beheren en authenticeren van gebruikers.
- **API-integraties**: Integratie van API’s voor data-uitwisseling tussen de backend en frontend.

Een lijst met **requirements**, waarin de functionaliteiten van het huidige CMS staan die in het prototype moeten worden opgenomen, is te vinden in de [requirements](Requirements.md). Hierin zijn ook een aantal non-functionele requirements opgenomen.

# Beschrijving van Objecten en Contentblokken

 In het prototype zijn verschillende objecten en contentblokken beschikbaar. Hieronder volgt een lijst met de objecten, blokken en de bijbehorende FormFields die worden gebruikt om de inhoud van deze objecten en content te beheren.

## Objecten

### Pagina (Page)

De **Pagina**-objecten vertegenwoordigen de webpagina's die worden weergegeven op de website. Ze zijn gebaseerd op `templates` en worden dynamisch opgemaakt. Dit maakt het mogelijk voor gebruikers om pagina’s te creëren en aan te passen. Deze templates vormen de basisstructuur voor het presenteren van verschillende soorten content.
### Actor
Het **Actor**-object is bedoeld om informatie over acteurs vast te leggen, zoals naam, geboortedatum en de films waarin ze voorkomen. Dit object kan nuttig zijn om te koppelen aan bijvoorbeeld filmgerelateerde blogposts of recensies.

| Attribuut   | FormField type |
| ----------- | -------------- |
| name        | text           |
| middle_name | text           |
| surname     | text           |
| dob         | date           |
| movies      | tags           |
| movie_names | dynamic        |
| full_name   | dynamic        |

### Author
Het **Author**-object bevat gegevens over auteurs, vergelijkbaar met het Actor-object. Dit object kan worden gebruikt voor het beheren van informatie over auteurs van boeken die worden besproken in de blog of recensies.

|Attribuut|FormField type|
|---|---|
|name|text|
|middle_name|text|
|surname|text|
|dob|date|
|tags|books|
|book_names|dynamic|
|full_name|dynamic|

### BlogPost
Het **BlogPost**-object is een essentieel onderdeel van de website en dient als opslagstructuur voor blogartikelen. Elk artikel kan categorieën, afbeeldingen en verschillende contentblokken bevatten.

| Attribuut    | FormField type |
| ------------ | -------------- |
| title        | text           |
| excerpt      | textarea       |
| image        | media-item     |
| category_id  | relation       |
| published_at | date           |
| published    | select         |
| blocks       | blocks         |

### Book
Het **Book**-object bevat informatie over boeken die relevant kunnen zijn voor de blog of recensies. Dit object biedt specifieke velden voor auteurs, publicatiejaar en beschrijving.

|Attribuut|FormField type|
|---|---|
|title|text|
|published_year|date|
|description|textarea|
|author_id|relation|
|author_named|dynamic|
|published_year_formatted|dynamic|

### Category

Het **Category**-object biedt een manier om blogposts te categoriseren, waardoor gebruikers eenvoudiger kunnen filteren en navigeren binnen verschillende soorten content.

|Attribuut|FormField type|
|---|---|
|name|text|

### Director
Het **Director**-object bevat gegevens over filmregisseurs. Dit object kan worden gekoppeld aan films in de blog of recensies.

|Attribuut|FormField type|
|---|---|
|name|text|
|middle_name|text|
|surname|text|
|date_of_birth|date|
|movies|tags|
|movie_names|dynamic|
|full_name|dynamic|

### FrontendUser

Het **FrontendUser**-object representeert geregistreerde gebruikers van de website die reacties op blogposts of recensies kunnen achterlaten.

|Attribuut|FormField type|
|---|---|
|Username|text|
|Password|password|
|Email|email|

### Movie
Het **Movie**-object biedt de structuur om informatie over films op te slaan, inclusief de regisseur en acteurs.

|Attribuut|FormField type|
|---|---|
|title|text|
|release_year|date|
|description|textarea|
|trailer_url|video|
|director_id|relation|
|actors|tags|
|director_name|dynamic|
|actor_names|dynamic|
|release_year_formatted|dynamic|

### Review
Het **Review**-object is bedoeld voor recensies van boeken en films. Gebruikers kunnen beoordelingen en scores achterlaten.

|Attribuut|FormField type|
|---|---|
|title|text|
|excerpt|textarea|
|score|number|
|image|media-item|
|books|edit_relation|
|movies|edit_relation|
|subject|dynamic|
|subject_type|dynamic|
|blocks|blocks|

### User

Het **User**-object representeert beheerders die toegang hebben tot het CMS om de website te beheren.

|Attribuut|FormField type|
|---|---|
|Username|text|
|Password|password|
|Email|email|
|Inloggen|submit|

## Templates

Templates zijn vooraf gedefinieerde structuren die de lay-out en organisatie van verschillende soorten content op de website bepalen.

### Blog
De **Blog**-template wordt gebruikt voor blogpagina’s en bevat de lay-out voor een header-afbeelding, een titel, en een verzameling van contentblokken.

|Attribuut|FormField type|
|---|---|
|header_image|media-item|
|header_title|text|
|blocks| blocks|

### Home
De **Home**-template vormt de opmaak van de homepage. Deze template bevat velden voor een afbeelding, een titel, een over-sectie, en algemene contentblokken.

|Attribuut|FormField type|
|---|---|
|header_image|media-item|
|header_title|text|
|about|blocks|
|blocks|blocks|

### Review
De **Review**-template biedt de lay-outstructuur voor recensiepagina's.

|Attribuut|FormField type|
|---|---|
|header_image|media-item|
|header_title|text|
|blocks|blocks|

## Blocks
Blocks zijn herbruikbare elementen voor content in de templates. Ze zorgen voor modulariteit en flexibiliteit bij het opbouwen van pagina’s.
### Call to Action
Een **Call to Action**-block is bedoeld om gebruikers aan te moedigen tot een actie, bijvoorbeeld een link volgen of een formulier invullen.

|Attribuut|FormField type|
|---|---|
|title|text|
|text|html|
|button_url|url|
|button_text|text|

### Image
Het **Image**-block biedt de mogelijkheid om een afbeelding weer te geven in de content.

|Attribuut|FormField type|
|---|---|
|image|media-item|

### Paragraph
Het **Paragraph**-block biedt een sectie met tekst en een optionele titel voor algemene tekstinhoud.

|Attribuut|FormField type|
|---|---|
|title|text|
|html|text|

### About
Het **About**-block wordt vaak gebruikt om algemene informatie weer te geven, zoals op een "Over ons"-pagina.

|Attribuut|FormField type|
|---|---|
|title|text|
|text|html|
