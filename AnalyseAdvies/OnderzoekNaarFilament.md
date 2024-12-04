# Onderzoek naar Filament

## Inleiding

Dit document beschrijft de analyse voor een CMS met **Filament**, een Laravel-package voor het opzetten van beheertools en content management systemen. Deze evaluatie is uitgevoerd als onderdeel van een breder onderzoek naar beschikbare productopties voor content management en focust op de sterke en zwakke punten, evenals de uitbreidbaarheid en gebruiksvriendelijkheid van het Filament CMS.

*De belangrijkste sterke en zwakke punten van het Filament CMS, evenals mogelijke kansen en bedreigingen, zijn in kaart gebracht in de SWOT-analyse die je [hier kunt lezen](SwotFilamentCms).*

## Systeemoverzicht: Beheren van content

Het Filament CMS is gebaseerd op **Laravel** en maakt gebruik van de Laravel Eloquent ORM. Filament biedt een intuïtieve interface en diverse componenten om content-en objectbeheer eenvoudig en schaalbaar te maken. Het systeem biedt standaardcomponenten zoals resource- en table-views, waarmee content kan worden geconfigureerd en beheerd zonder veel custom code.

### Resources

Met Filament's **Resource** kunnen beheerders content en objecten beheren en structureren. Het stelt hen in staat om content-items en objecten toe te voegen, te wijzigen, te verwijderen en te organiseren binnen een heldere en gebruiksvriendelijke interface. Tot de belangrijkste functies behoren:

1. **CRUD-operaties**: Gebruikt standaard Laravel-resource-controllers en Eloquent om objecten aan te maken, te lezen, bij te werken en te verwijderen.
2. **Views**: De Resource biedt standaard view-opties (index, create, edit, view) die eenvoudig kunnen worden aangepast aan de specifieke eisen van het CMS.
3. **FormFields**: Standaardonderdelen in Filament bieden de mogelijkheid om snel invoervelden toe te voegen en te configureren.
4. **Permissions en Middleware**: Er is ondersteuning voor toegangscontrole en het toekennen van specifieke rollen en rechten binnen het CMS.

### Contentbeheer

Filament biedt binnen de tabelweergave de mogelijkheid om content te ordenen met **drag-and-drop** functionaliteit, waardoor items eenvoudig herschikt kunnen worden. Echter, Filament biedt geen out-of-the-box ondersteuning om hiërarchische informatie mee te nemen. Hoewel het niet onmogelijk is om hiërarchie voor contentpagina's toe te passen, is voor het nabootsen van de hiërarchische drag-and-drop in het AllesOnline CMS relatief veel maatwerk nodig. 

Mocht er voor Filament gekozen worden, dan wordt aanbevolen om te onderzoeken of de bestaande functionaliteiten van Filament benut kunnen worden om het beheer en de ordening van hiërarchische content op een andere manier te ondersteunen.

### Page (Model)

Net zoals in het AllesOnline CMS kunnen we in een CMS met Filament gebruik maken van een **Page**-model. Ook hier vertegenwoordigt het afzonderlijke pagina's en dient als een interface naar CMS content. Dit model wordt beheerd met behulp van **Laravel Eloquent model** en biedt ondersteuning voor dynamische URL-generatie en meertalige content. Dankzij de Eloquent-relaties kunnen pagina’s hiërarchisch worden gekoppeld, wat bijdraagt aan de opbouw van een unieke URI-structuur en navigatie voor elke pagina.

### Templates en FormFields

**Templates** in Filament worden opgebouwd met behulp van resource-controllers waarin de lay-out en inhoudsvelden voor een specifieke pagina kunnen worden gedefinieerd. **FormFields** (zoals tekstvakken, datumvelden, keuzelijsten) worden gebruikt om de invoer en bewerking van content intuïtief te maken. Templates ondersteunen modulaire opbouw door middel van **blocks** en herbruikbare content secties, wat vooral handig is voor componenten die in verschillende delen van de website terugkomen.

## Systeemoverzicht: Beheren van objecten

Het Filament CMS biedt met de Resources ook de mogelijkheden voor het beheer van objecten zoals producten, transacties of andere entiteiten.
### Resources voor Objectbeheer

Objectbeheer in Filament wordt uitgevoerd via resource-controllers die zijn uitgebreid voor specifieke soorten entiteiten. Hoewel een "ObjectManagerResource" niet formeel bestaat, kunnen resources worden ingesteld om objecten zoals producten of klantgegevens te beheren. Filament ondersteunt hierbij:

1. **Dynamische formulieren en velden**: De objectvelden kunnen eenvoudig worden aangepast om de unieke attributen van elk type object vast te leggen.
2. **Relaties**: Eloquent-relaties kunnen worden gedefinieerd voor interactie tussen objecten. Dit maakt het mogelijk om bijvoorbeeld gerelateerde producten of gekoppelde klantgegevens te beheren.
3. **Zoek- en filteropties**: Met de filter- en sorteeropties in Filament kunnen objecten worden beheerd, doorzocht, en gefilterd op basis van verschillende criteria.

### Eloquent Models en CMS Schema's

Filament maakt gebruik van **Eloquent-modellen** voor de representatie van objecten. Elk model komt overeen met een database-tabel en kan relaties tussen objecten definiëren. Het ophalen en verwerken van gegevens kan eenvoudig worden aangepast met dynamische query’s en filters, wat ontwikkelaars helpt om objecten efficiënt te beheren en te manipuleren.
### FormFields voor Objecten

De **FormFields**-modules voor objecten in Filament maken het mogelijk om verschillende gegevensvelden (zoals tekst, datum of relatievelden) te gebruiken voor het invoeren en beheren van gegevens. Elke formfield biedt de mogelijkheid om gegevens invoeropties aan te passen aan de vereisten van een specifiek object.

## Evaluatie van Filament

Hoewel het Filament een solide basis biedt voor het opzetten van contentbeheer, heeft het systeem beperkingen in termen van uitbreidbaarheid en schaalbaarheid. De onderstaande analyse richt zich op de codestructuur, gebruiksvriendelijkheid en mogelijkheden voor aanpassing binnen het Filament CMS.

### Gebrekkige Documentatie en Ondersteuning

Filament is relatief nieuw en de community-ondersteuning is nog groeiend. Hoewel de documentatie uitgebreid is, kunnen ontwikkelaars soms problemen ondervinden bij complexere configuraties.

**Aanbeveling**:
- Verhoog de betrokkenheid van de gemeenschap en voeg meer gedetailleerde voorbeelden toe voor complexe configuraties.

### Beperkingen in Complexe Structuren en Code-organisatie

De structuur van resources binnen Filament maakt het instellen van zeer complexe hiërarchieën of relationele structuren uitdagend. Dit kan de uitbreidbaarheid van het systeem beperken.

**Gevolgen van deze beperking**:
- De kans op bugs neemt toe naarmate meer custom code wordt toegevoegd.
- Mogelijkheden voor hergebruik en uitbreidbaarheid worden beperkt.

**Aanbeveling**:
- Overweeg om de code te modulariseren en aanvullende interfaces of abstracties te ontwikkelen voor complexere structuren.

## SOLID-Principes in de Filament Resources

Bij het analyseren van de Filament resources blijkt dat ze op sommige vlakken niet volledig voldoen aan de SOLID-principes, met name aan het **Single Responsibility Principle** (SRP) en het **Open/Closed Principle** (OCP).

* **Single Responsibility Principle (SRP)**:
    
    - De Filament resources zijn zodanig ontworpen dat ze vaak meerdere verantwoordelijkheden dragen, wat kan leiden tot verhoogde complexiteit en moeilijkheden bij onderhoud en debugging.
    - Wanneer één resource meerdere functies vervult, kan het doorvoeren van wijzigingen in één aspect van de resource onbedoeld effect hebben op andere delen van de code.
    
* **Open/Closed Principle (OCP)**:
    
    - Naarmate resources uitgebreider worden en specifieke functionaliteiten moeten ondersteunen, kunnen er met regelmatig wijzigingen in bestaande code nodig zijn.

Toch kan beargumenteerd worden dat deze overtredingen van de SOLID-principes in bepaalde situaties juist de developer-experience verbeteren. Hoewel er verschillende weergaves (views) in één resource worden gespecificeerd, zorgt dit ervoor dat alle views voor een bepaald objecttype op één plek geconcentreerd zijn, wat de overzichtelijkheid verhoogt.

Een bijkomend voordeel van de Filament-bibliotheek is dat veel van de functionaliteiten al zijn geënncapsuleert in specifieke functies die door de library zelf worden getest. Wanneer maatwerkfunctionaliteiten worden toegevoegd, kunnen deze onafhankelijk worden getest doormiddel van unit-en componenttests.

## Conclusie

Filament biedt een goed uitgangspunt voor het opzetten van een eenvoudig CMS, maar kent enkele beperkingen als het gaat om schaalbaarheid en uitbreidbaarheid. Het gebruik van Eloquent als ORM en de Laravel-resource-architectuur zorgt voor een solide basis, maar de mogelijkheden voor aanpassing zijn beperkter in vergelijking met meer volwassen CMS-oplossingen.

Hoewel Filament aantrekkelijk is vanwege de integratie met Laravel, zullen organisaties die een complexe contentstructuur of uitgebreide functionaliteiten nodig hebben, mogelijk tegen grenzen aanlopen van Filament. Daarom moet een balans worden gevonden tussen het gebruiksgemak van Filament en de potentieel hoge kosten voor het aanpassen van het systeem.