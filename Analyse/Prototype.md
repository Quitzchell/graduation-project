# Prototype

## Inleiding
Om het onderzoek naar verschillende Content Management Systemen (CMS) in een praktische context uit te voeren, wordt er een prototype applicatie ontwikkeld. Deze prototype applicatie dient als blauwdruk voor de standaard functionaliteiten van een typische AllesOnline-webapplicatie. Het doel van dit prototype is om zowel een subjectieve als objectieve vergelijking te maken tussen het huidige AllesOnline CMS en alternatieve CMS-pakketten, door dezelfde functionaliteiten in verschillende systemen te implementeren en te evalueren.

Het prototype stelt ons in staat om de technische mogelijkheden van elk CMS in een gecontroleerde omgeving te testen. Hierbij worden belangrijke aspecten zoals schaalbaarheid, onderhoudbaarheid, gebruiksvriendelijkheid en prestaties onderzocht. Daarnaast biedt het prototype de mogelijkheid om realistische migratieproeven uit te voeren, waarbij de gegevens en functionaliteiten van het huidige CMS worden overgezet naar nieuwe systemen. Op deze manier kunnen potentiële obstakels, zoals data-integriteit en functionaliteitsverlies, vroegtijdig worden geïdentificeerd en aangepakt.

## Doelstellingen van het prototype
Het prototype heeft de volgende doelstellingen:

1. **Vergelijking van CMS-pakketten**: Objectieve evaluatie van de technische en functionele mogelijkheden van het huidige AllesOnline CMS ten opzichte van de alternatieve CMS-pakketten.
   
2. **Migratieanalyse**: Onderzoeken van het migratieproces door het overzetten van data en functionaliteiten van het huidige CMS naar de alternatieve systemen. Hierbij worden de prestaties, compatibiliteit en eventuele technische uitdagingen in kaart gebracht.

3. **Functionele validatie**: Valideren dat de basale functionaliteiten van de AllesOnline-webapplicaties, zoals contentbeheer, gebruikersbeheer en API-integraties, in elk CMS correct functioneren.

4. **Gebruikerservaring en onderhoudbaarheid**: Het testen van de gebruiksvriendelijkheid van het beheer in de verschillende CMS-oplossingen, alsmede de onderhoudbaarheid en het aanpassingsgemak voor ontwikkelaars.

## Prototype Specificaties
### Functionele eisen
Het prototype is ontworpen om de kernfunctionaliteiten van een standaard AllesOnline-webapplicatie te simuleren. Dit omvat onder andere:

- **Contentbeheer**: De mogelijkheid om webpagina's aan te maken, te bewerken en te verwijderen.
- **Gebruikersbeheer**: Functies voor het toevoegen, beheren en authenticeren van gebruikers.
- **API-integraties**: Integratie van API’s voor data-uitwisseling tussen de backend en frontend.
- **Data validatie**: Controleren of ingevoerde gegevens correct worden verwerkt en weergegeven in de frontend.

### Technische specificaties
Het prototype wordt gebouwd met de volgende technische kenmerken:

- **Front-end**: Een eenvoudige webinterface die communiceert met de verschillende CMS-backends via API-aanvragen. Deze front-end wordt hergebruikt voor alle tests, zodat verschillen in prestaties of functionaliteit aan de CMS-systemen kunnen worden toegeschreven.
  
- **Back-end**: Er worden meerdere back-end implementaties ontwikkeld, elk gebaseerd op een ander CMS-pakket, waaronder het huidige AllesOnline CMS. Deze back-ends worden getest op aspecten zoals API-performance, integratie van modules en databasebeheer.
  
- **Database**: Voor het prototype wordt in eerste instantie gewerkt met dummydata die is gegenereerd via Seeders en Factories. Dit maakt het mogelijk om verschillende scenario’s te testen zonder afhankelijk te zijn van productiegegevens.

## Prototype Omschrijving
Het prototype zal worden ontwikkeld als een stamboomapplicatie, waarin gebruikers een familiestamboom kunnen beheren en visualiseren. Dit concept biedt de mogelijkheid om vrijwel alle essentiële functionaliteiten van een CMS te implementeren, zoals contentbeheer, gebruikersbeheer, en gegevensvisualisatie. Bovendien maakt dit ontwerp het mogelijk om op een heldere en gestructureerde manier gebruik te maken van dummy-data, waardoor geautomatiseerde tests eenvoudig kunnen worden uitgevoerd. Deze aanpak zorgt voor een consistente testomgeving en biedt waardevolle inzichten in de prestaties, schaalbaarheid en gebruiksvriendelijkheid van de verschillende CMS-pakketten.