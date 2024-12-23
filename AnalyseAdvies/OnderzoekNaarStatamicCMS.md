# Onderzoek naar het Statamic CMS

# Inleiding

Dit document beschrijft de analyse over het gebruik van Statamic voor het opzetten van een CMS voor de websites die AllesOnline ontwikkelt. Statamic is een commercieel CMS dat veel out-of-the-box functionaliteiten biedt waarmee ontwikkelaars en gebruikers eenvoudig een website kunnen bouwen en beheren.


# Over Statamic

Net zoals het AllesOnline CMS en Filament maakt Statamic gebruik van **Laravel**, waardoor het mogelijk is om de functionaliteiten van Laravel te benutten bij het ontwikkelen van een systeem.

De basisinstallatie van Statamic komt met een volledig CMS waarin een overvloed aan functionaliteiten beschikbaar is voor het beheren van content. De kernonderdelen zijn:

* Assets: voor het beheren van afbeeldingen en video's.
* Configuration: aparte configuratie voor het instellen van Statamic.
* Collections: voor het opzetten van structuur en het organiseren van content en objecten.
* Fields: verschillende soorten invoervelden voor content.
* Globals: herbruikbare informatie die globaal opgehaald kan worden.
* Search: out-of-the-box ondersteuning voor zoekfunctionaliteit, inclusief Algolia.
* Navigation: beheren van navigatiemenu's via een drag & drop interface.
* Taxonomies: functionaliteit voor het categoriseren en taggen van content voor globale organisatie van content.
* Users: Voor het beheren van gebruikersgegevens.

De standaard confugiratie voor Statamic maakt gebruik van **flat-file**. Dit houdt in dat, inplaats van dat gegevens opgeslagen worden in een databse, gegevens gepersisteert worden in de codebase binnen markdown bestanden. 

> _voorbeeld van een flat-file markdown bestand_
> [Flat-file voor homepage](../Bijlagen/VoorbeeldStatamicFlatFile.md)

Net zoals bij Filament is het mogelijk om 