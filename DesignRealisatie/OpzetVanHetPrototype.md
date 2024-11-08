# Opzet van het prototype

## Inleiding

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

## Beschrijving van Objecten en Contentblokken

 In het prototype zijn verschillende objecten en contentblokken beschikbaar. Hieronder volgt een lijst met de objecten, blokken en de bijbehorende FormFields die worden gebruikt om de inhoud van deze objecten en content te beheren.

### Pagina (Page)

De pagina’s die beschikbaar zijn via XML-templates en dynamisch worden opgemaakt. Voor een 

### Actor

|Attribuut|FormField type|
|---|---|
|name|text|
|middle_name|text|
|surname|text|
|dob|date|
|movies|tags|
|movie_names|dynamic|
|full_name|dynamic|

### Author

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

|Attribuut|FormField type|
|---|---|
|title|text|
|excerpt|textarea|
|image|media-item|
|category_id|relation|
|published_at|date|
|published|select|
|blocks|blocks|

### Book

|Attribuut|FormField type|
|---|---|
|title|text|
|published_year|date|
|description|textarea|
|author_id|relation|
|author_named|dynamic|
|published_year_formatted|dynamic|

### Category

De categorieën waarin blogposts kunnen worden onderverdeeld. In de objectmanager van een categorie wordt een relatie-module toegevoegd, waarmee kan worden ingezien welke blogposts de geselecteerde categorie gebruiken.

|Attribuut|FormField type|
|---|---|
|name|text|

### Director

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

Gebruikers die zich kunnen registreren op de website om reacties op blogposts en reviews achter te laten.

|Attribuut|FormField type|
|---|---|
|Username|text|
|Password|password|
|Email|email|

### Movie

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

Gebruikers, ook wel beheerders (administrators) van de website, hebben toegang tot het CMS en kunnen pagina’s, objecten en content beheren.

|Attribuut|FormField type|
|---|---|
|Username|text|
|Password|password|
|Email|email|
|Inloggen|submit|

## Templates
### Blog

|Attribuut|FormField type|
|---|---|
|header_image|media-item|
|header_title|text|
|blocks| blocks|

### Home

|Attribuut|FormField type|
|---|---|
|header_image|media-item|
|header_title|text|
|about|blocks|
|blocks|blocks|

### Review
|Attribuut|FormField type|
|---|---|
|header_image|media-item|
|header_title|text|
|blocks|blocks|
## Blocks

### Call to Action
|Attribuut|FormField type|
|---|---|
|title|text|
|text|html|
|button_url|url|
|button_text|text|

### Image
|Attribuut|FormField type|
|---|---|
|image|media-item|

### Paragraph
|Attribuut|FormField type|
|---|---|
|title|text|
|html|text|

### About
|Attribuut|FormField type|
|---|---|
|title|text|
|text|html|
