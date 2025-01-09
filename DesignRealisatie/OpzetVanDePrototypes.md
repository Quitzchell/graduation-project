# Opzet van de prototypes

# Inleiding

Om verschillende opties voor een Content Management Systeem (CMS) op een gecontroleerde manier met elkaar te vergelijken, zal ik in dit project een praktisch experiment uitvoeren: het ontwikkelen van een full-stack prototypewebsite met een frontend die door verschillende CMS-oplossingen aangestuurd kan worden. Op deze manier wordt het makkelijker om te beoordelen of de verschillende functionaliteiten die voor een doorsnee AllesOnline-website worden verwacht, aanwezig zijn in de te onderzoeken CMS-oplossingen. Los daarvan maakt dit het ook mogelijk om te onderzoeken of er een tool voor de migratie van het AllesOnline CMS naar een andere CMS-oplossing gerealiseerd kan worden. Aan de hand van een prototype met een ander CMS weten we namelijk hoe bepaalde bestanden eruit moeten zien.

Uiteindelijk zal dit alles een nuttig inzicht geven in andere CMS oplossingen en of het verstandig is om eventueel naar een ander CMS over te stappen.

# Prototype Omschrijving

Omdat het prototype moet voldoen aan de vereisten voor een doorsnee AllesOnline-website, is het niet nodig om voor het prototype uiterst complex te zijn. Daarom zal het prototype een vrij simpele blog-website vanuit het oogpunt van Napoleon Bonaparte zijn. Dit concept biedt namelijk de mogelijkheid om alle requirements voor wat een doorsnee AllesOnline-website moet kunnen, te realiseren.  

Het prototype zal uit verschillende systemen bestaan die met elkaar communiceren:

- **Frontend**: Een webinterface gerealiseerd met Next.js, die via API-requests kan communiceren met de te realiseren backends.
    
- **Backend(s)**: Een backend met CMS die gebruikers in staat stelt de content van een website te beheren en communiceert met zowel de frontend als de database. 
    
- **Database**: Een MySQL database voor het persisteren van gegevens voor de website. 
	
* **Cypress**: Om de communicatie tussen de backend en de frontend te valideren, zal er een testsuite met Cypress worden ontwikkeld die kan controleren of de website vanuit de verschillende CMS-oplossingen de correcte informatie naar de frontend doorgeeft.
	  
* **Docker**: Voor alle systemen zal een aparte Dockercontainer geconfigureerd worden waarin de applicaties kunnen draaien. 
	  
## Functioneel

De prototypes zullen aan de volgende eisen moeten voldoen:

- **Contentbeheer**: De mogelijkheid om webpagina's en de bijbehorende content via het CMS te beheren.
- **Objectenbeheer**: De mogelijkheid om objecten en de bijbehorende inhoud via het CMS te beheren.
- **API-integraties**: Integratie van API’s voor het uitwisselen van gegevens tussen de backend en de frontend.

# Bijlagen

> _Volledige lijst met requirements voor een doorsnee AllesOnline-website:_
>  * [Requirements](../AnalyseAdvies/Requirements.md)


> _UML entiteiten diagram voor opzet prototype:_
> * [UML entiteiten diagram](../Bijlagen/UmlEntiteitenDiagramPrototype.md)