# **Opzet van de prototypes**

# Inleiding

Om te valideren of de Content Management Systemen die in dit onderzoek worden onderzocht daadwerkelijk functioneel zijn, zal ik in dit project een praktisch onderzoek uitvoeren door een full-stack prototype te ontwikkelen.

Dit prototype zal een frontend, gebouwd met een modern framework, koppelen aan verschillende backends met een CMS. Hierbij wordt gebruik gemaakt van een *decoupled architectuur*, waarbij de frontend en backend gescheiden zijn en via API's communiceren. Dit maakt het gemakkelijk om te beoordelen of de functionaliteiten die voor een typische AllesOnline-website worden verwacht, aanwezig zijn in de te onderzoeken CMS'en. 

Bovendien biedt het opzetten van deze prototypes de mogelijkheid om te onderzoeken of er een tool ontwikkeld kan worden voor de migratie van het AllesOnline CMS naar een andere CMS. Aan de hand van een gevalideerd prototype dat een extern systeem gebruikt, kunnen we bepalen hoe bepaalde bestanden er in deze systemen uit moeten zien.

# Prototype Omschrijving

Omdat het prototype moet voldoen aan de vereisten voor een doorsnee AllesOnline-website, is het niet nodig om voor het prototype uiterst complex te zijn. Daarom zal het prototype een vrij simpele blog-website vanuit het oogpunt van Napoleon Bonaparte zijn. Dit concept biedt namelijk de mogelijkheid om alle requirements voor wat een doorsnee AllesOnline-website moet kunnen, te realiseren.

Het prototype zal uit verschillende systemen bestaan die met elkaar communiceren:

- **Frontend**: Een webinterface gerealiseerd met Next.js, die via API-requests kan communiceren met de te realiseren backends.
    
- **Backend(s)**: Een backend met CMS die gebruikers in staat stelt de content van een website te beheren en communiceert met zowel de frontend als de database. 
    
- **Database**: Een MySQL database voor het persisteren van gegevens voor de website. 
	
* **Cypress**: Om correcte communicatie tussen de backend en de frontend te valideren, zal er een testsuite met Cypress worden ontwikkeld die kan controleren of de website vanuit de verschillende CMS'en succesvol gerenderd kan worden.
	  
* **Docker**: Voor alle systemen zal een aparte Docker-container geconfigureerd worden waarin de applicaties kunnen draaien.
	  
# Functioneel

De prototypes zullen aan de volgende eisen moeten voldoen:

- **Contentbeheer**: De mogelijkheid om pagina's en de bijbehorende content via het CMS te beheren.
- **Objectenbeheer**: De mogelijkheid om objecten en de bijbehorende content via het CMS te beheren.
- **API-integraties**: Integratie van API’s voor het uitwisselen van gegevens tussen de backend en de frontend.

# Bijlagen

> _Volledige lijst met requirements voor een doorsnee AllesOnline-website:_
>  * [Requirements](../AnalyseAdvies/Requirements.md)

> _UML entiteiten diagram voor opzet prototype:_
> * [UML entiteiten diagram](../Bijlagen/UmlEntiteitenDiagramPrototype.md)