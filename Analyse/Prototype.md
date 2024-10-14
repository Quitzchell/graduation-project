# Prototype

## Inleiding
In dit onderzoek vergelijken we verschillende Content Management Systemen (CMS) door een praktisch experiment uit te voeren. We ontwikkelen meerdere prototypes om te zien hoe eenvoudig het is om van het AllesOnline CMS naar andere systemen te migreren.

Het eerste prototype, gebaseerd op het AllesOnline CMS, fungeert als referentiepunt. Dit betekent dat we de meest gebruikte functies van een typische AllesOnline-webapplicatie, zoals object-en contentbeheer, hierin opnemen. Dit referentiepunt dient als basis om later het migratieproces naar andere CMS-pakketten te kunnen beoordelen.

Het doel van dit project is om te onderzoeken hoe gemakkelijk het is om het AllesOnline CMS naar verschillende alternatieve CMS-pakketten te migreren. Door hetzelfde prototype in andere systemen te bouwen, kunnen we de complexiteit van de migratie evalueren. We richten ons op de volgende aspecten:

- **Compatibiliteit van functies**: Hoe makkelijk kunnen de kernfunctionaliteiten van AllesOnline worden toegepast in andere systemen? Zijn er aanpassingen nodig om dezelfde functionaliteiten te behouden?
- **Migratie-inspanningen**: Hoeveel tijd en middelen kost de migratie? Welke obstakels komen we tegen?
- **Gebruiksvriendelijkheid en schaalbaarheid**: Bieden de alternatieve CMS-pakketten voldoende flexibiliteit en schaalbaarheid? Hoe gebruiksvriendelijk zijn ze in vergelijking met AllesOnline?
- **Integratiemogelijkheden**: Hoe eenvoudig is het om bestaande backend-systemen en API-koppelingen van AllesOnline naar andere CMS-pakketten te migreren?

Het uiteindelijke doel van dit onderzoek is inzicht te krijgen in de haalbaarheid en efficiëntie van het overstappen naar andere CMS-systemen. Hierbij kijken we niet alleen naar de technische mogelijkheden, maar ook naar de lange termijn voordelen, zoals lagere kosten, eenvoudiger onderhoud en toekomstbestendigheid.

Deze vergelijking stelt ons in staat om een goed onderbouwd advies te geven over welk CMS het beste aansluit bij de behoeften van onze klanten en developers.
# Prototype Specificaties
### Functionele eisen
Het prototype is ontworpen om de kernfunctionaliteiten van een standaard AllesOnline-webapplicatie te simuleren. Dit omvat onder andere:

- **Contentbeheer**: De mogelijkheid om webpagina's inclusief content aan te maken, te bewerken en te verwijderen.
- **Gebruikersbeheer**: Functies voor het toevoegen, beheren en authenticeren van gebruikers.
- **API-integraties**: Integratie van API’s voor data-uitwisseling tussen de backend en frontend.
- **Data validatie**: Controleren of ingevoerde gegevens correct worden verwerkt en weergegeven in de frontend.

Een lijst met **requirements**, waarin de functionaliteiten van het huidige CMS staan die in het prototype moeten worden opgenomen, is te vinden in de [requirements](/Analyse/Requirements.md). Hierin zijn ook een aantal Non-Functional requirements opgenomen.

### Technische specificaties
Het prototype wordt gebouwd met de volgende technische kenmerken:

- **Front-end**: Een eenvoudige webinterface die communiceert met de verschillende CMS-backends via API-aanvragen. Deze front-end wordt hergebruikt voor alle tests, zodat verschillen in prestaties of functionaliteit aan de CMS-systemen kunnen worden toegeschreven.
  
- **Back-end**: Er worden meerdere back-end implementaties ontwikkeld, elk gebaseerd op een ander CMS-pakket, waaronder het huidige AllesOnline CMS. Deze back-ends worden getest op aspecten zoals API-performance, integratie van modules en databasebeheer.
  
- **Database**: Voor het prototype wordt in eerste instantie gewerkt met dummydata die is gegenereerd via Seeders en Factories. Dit maakt het mogelijk om verschillende scenario’s te testen zonder afhankelijk te zijn van productiegegevens.

# Prototype Omschrijving
Het prototype dat ontwikkeld wordt is een bescheiden blogwebsite van de enige echte Napoleon Bonaparte. Dit concept biedt de mogelijkheid om vrijwel alle essentiële functionaliteiten van een CMS te implementeren, zoals contentbeheer, objectbeheer, gebruikersbeheer, en gegevensvisualisatie. Bovendien maakt dit ontwerp het mogelijk om op een heldere en gestructureerde manier gebruik te maken van dummy-data, waardoor geautomatiseerde tests eenvoudig kunnen worden uitgevoerd. Deze aanpak zorgt voor een consistente testomgeving en biedt waardevolle inzichten in de prestaties, schaalbaarheid en gebruiksvriendelijkheid van de verschillende CMS-pakketten.

## Objecten
### Pagina (Page)
De pagina’s die beschikbaar zijn op de website.

### Gebruiker (User)
Gebruikers zijn ook wel de beheerders (administrators) van de website. Zij hebben toegang tot het CMS en kunnen pagina’s, objecten en content beheren.

| Attribute | FormField type |
| --------- | -------------- |
| Username  | text           |
| Password  | password       |
| Email     | email          |
| Inloggen  | submit         |

### BlogPost
Blogposts met verschillende attributen en content.

| Attribute    | FormField type |
| ------------ | -------------- |
| Title        | text           |
| Excerpt      | textarea       |
| Image        | media-item     |
| Category     | relation       |
| Published at | date           |
| Published    | select         |
| Content      | blocks         |

### Categorie (Category)
De categorieën waarin blogposts kunnen worden onderverdeeld. In de objectmanager van een categorie wordt een relatie-module toegevoegd, waarmee kan worden ingezien welke blogposts de geselecteerde categorie gebruiken.

| Attribute | FormField type |
| --------- | -------------- |
| name      | text           |

### FrontendGebruiker (FrontendUser)
Gebruikers die zich kunnen aanmelden op de website om reacties op blogposts en reviews achter te laten.

| Attribute | FormField type |
| --------- | -------------- |
| Username  | text           |
| Password  | password       |
| Email     | email          |

## Content
### 