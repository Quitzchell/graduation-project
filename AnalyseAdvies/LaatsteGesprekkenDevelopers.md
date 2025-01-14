# **Laatste gesprekken met Developers voor opstellen advies**

# Wilco Kuijpers

In de laatste week van het project heb ik met Wilco gesproken over de resultaten van het onderzoek en de realisatie van de CMS'en. Tijdens dit gesprek werd gesproken over de voordelen en nadelen van Filament en Statamic die ik tijdens mijn onderzoek heb geconstateerd.

Wilco benoemde tijdens het gesprek meerdere punten, waar hij zowel positieve als kritische kanttekeningen bij plaatste.

**Positief over Filament:**
- De flexibiliteit van Filament, met name dat de schema's direct aan te passen zijn omdat deze in PHP gedefinieerd worden. 
- Ondersteuning voor autocompletion in de schema's, omdat deze in PHP worden gedefinieerd.
- De tools die Filament beschikbaar stelt om de levenscyclus van de gegevensopslag te wijzigen.
- De mogelijkheid om Filament ook in te zetten voor klanten die niet perse een CMS nodig hebben, maar een adminpanel
- Relatief duidelijke documentatie in tegenstelling tot die van Statamic

**Negatief over Filament:**
- De best practice om custom componenten in Filament met Livewire en Alpine.js te realiseren.

**Positief Statamic:**
- Lijkt vrij gemakkelijk om op te zetten voor klanten die een eenvoudige CMS nodig hebben voor een simpele marketingwebsite.
- Mogelijkheid om, wanneer monolitisch gewerkt wordt, gemakkelijk live-previews voor klanten mogelijk te maken.

**Negatief Statamic:** 
- Dat Statamic nog achterloopt met het gebruik van Vue2.
- Beperkte mogelijkheden wanneer een relatie complexer is, bijvoorbeeld wanneer er pivot-data toegevoegd moet worden.
- Het gebruik van YAML voor het definiëren van schema's. 
- Dat er door het gebruik van YAML er geen autocompletion voor het maken van de schema's is. 
- Ontoegankelijke documentatie waar niet alles gemakkelijk te vinden blijkt te zijn.

Wilco gaf ook aan dat hij het niet als noodzakelijk ziet om een definitieve keuze te maken tussen **Filament** en **Statamic** voor toekomstige projecten. Voor eenvoudige marketingwebsites zonder complexe relaties of logica is **Statamic** een geschikte optie. Voor webapplicaties met complexere vereisten biedt **Filament** meer mogelijkheden dankzij de flexibele opzet. Bij grotere projecten waarbij een klant zowel een marketingwebsite als een webapplicatie vraagt, kan een combinatie van **Filament** en **Statamic** zelfs een overweging zijn.

# Yoran van Driel

Tijdens het gesprek met Wilco sloot ook Yoran aan. Op dat moment hadden we het niet per se meer over de voor- en nadelen van beide oplossingen, maar hij kon zich wel vinden in de conclusie die Wilco trok: dat we niet per se een definitieve keuze hoeven te maken tussen **Filament** en **Statamic**. Hij voegde daaraan toe dat de mogelijkheid van beide systemen om custom packages te maken een positieve bijkomstigheid is. Dit stelt ons niet alleen in staat om op maat gemaakte oplossingen eenvoudig over verschillende projecten te hergebruiken, maar maakt het ook mogelijk om het basis-CMS zo kaal mogelijk te houden.