# **Gesprekken met developers**

# Stefan Grevelink 

## Initiële gesprek aan start van project

Stefan is de eigenaar en directeur van AllesOnline, een bedrijf dat gespecialiseerd is in weboplossingen. Ongeveer tien jaar geleden heeft hij zelf een Content Management Systeem (CMS) ontwikkeld, wat tot op de dag van vandaag een belangrijk onderdeel van zijn bedrijf vormt. In gesprekken met Stefan komt naar voren dat zijn beslissing om een eigen CMS te bouwen destijds niet uit de lucht kwam vallen. Op het moment dat hij dit project startte, begon het Laravel-framework net populariteit te winnen onder PHP-developers. Dit nieuwe framework werd geprezen om zijn eenvoud en flexibiliteit, en leek een goede basis voor toekomstgerichte webontwikkelingen.

Volgens Stefan was er op dat moment naar zijn idee geen kwalitatief, breed beschikbaar CMS van derde partijen dat goed aansloot bij Laravel. Deze situatie leidde ertoe dat hij besloot een eigen CMS te ontwikkelen, specifiek gericht op een optimale samenwerking met Laravel. Dankzij deze beslissing kon Stefan zijn klanten een op maat gemaakte oplossing bieden die zowel gebruiksvriendelijk als goed te onderhouden was, en bovendien kon meegroeien met de veranderingen binnen het Laravel-ecosysteem.

Vandaag de dag blijft het CMS een belangrijk onderscheidend kenmerk van AllesOnline, waarmee het bedrijf zich weet te positioneren in een concurrerende markt. Stefan kijkt tevreden terug op zijn keuze, die zijn bedrijf niet alleen een uniek product en daarmee een voorsprong opleverde, maar ook de mogelijkheid bood om klanten op een flexibele en efficiënte manier te bedienen. 

Echter, is Stefan zich er ook van bewust dat het onderhouden van een eigen CMS een kostbare en intensieve bezigheid is. Dit besef heeft ertoe geleid dat hij wilt laten onderzoeken wat de mogelijkheden zijn om over te stappen naar een CMS van derde partijen. Door gebruik te maken van een extern CMS zou hij niet alleen de onderhoudskosten kunnen verlagen, maar mogelijk ook profiteren van de doorontwikkeling en ondersteuning die deze systemen genieten. Dit zou AllesOnline in staat stellen om zich nog sterker te richten op haar kernactiviteiten en de klantbehoeften, zonder de operationele lasten van het onderhouden van een eigen systeem.

Een belangrijke wens van Stefan is om het mogelijk te maken voor klanten om niet een volledig nieuw developmenttraject te hoeven doorlopen bij de overstap van het huidige CMS naar een nieuw CMS. Voor zover het haalbaar is, zou hij in het onderzoek graag willen zien of het mogelijk is om een migratie van het AllesOnline CMS naar een nieuw CMS te realiseren.

## Gesprek rond mid-term review

Tijdens de midterm reviews werd duidelijk dat ik met mijn examenproject niet goed op schema lag. Dit inzicht maakte me bewust van de noodzaak om mijn verantwoordelijkheden binnen AllesOnline anders aan te pakken. In plaats van voortdurend brandjes te blussen die door anderen worden veroorzaakt, besef ik dat ik de developers die verantwoordelijk zijn voor de opkomende issues hierover moet informeren en hen zelf de problemen moet laten oplossen.

Dit onderwerp kwam ook aan bod in een gesprek met Stefan naar aanleiding van de mid-term review. Hij bevestigde dat er niet van mij wordt verwacht dat ik problemen oplos waarvoor ik oorspronkelijk niet verantwoordelijk ben. Een betere aanpak zou zijn om bijvoorbeeld hem op de hoogte te brengen van de gevonden issues, of de verantwoordelijke developer direct aan te spreken op de bottlenecks die zij veroorzaken.

Doordat ik dit eerder niet deed, heb ik binnen een aantal projecten van AllesOnline flink wat overuren gemaakt. Hierdoor heb ik de tijd die ik voor mijn examenproject had, niet optimaal benut. In het gesprek met Stefan heb ik dan ook aangegeven dat ik bewuster met mijn tijd zal omgaan, het aantal overuren zal beperken en de beschikbare tijd voor mijn examenproject effectief zal inzetten. Dit zodat ik de verloren tijd in kan halen en kan voorkomen dat ik mezelf tijdens een werkweek overbelast.

## Gesprek rond afronding van CMS met Filament

Rond de afronding van het prototype CMS met Filament benadrukte Stefan opnieuw het belang van het onderzoeken of er een gemakkelijke manier is om van het AllesOnline CMS naar het Filament CMS te migreren. Naar aanleiding van deze opmerking besloot ik, voordat ik mijn onderzoek naar Statamic begin, eerst een spike te doen om de mogelijkheden tot migratie te verkennen.

# Wilco Kuijpers

## Algemene gesprekken
 
Wilco werkt al ongeveer acht jaar bij AllesOnline en is onze senior software developer. In de gesprekken die ik tijdens werktijd met hem voer, komt het CMS regelmatig ter sprake. Deze gesprekken vinden vooral plaats wanneer we samen in de wirwar van spaghetticode op zoek zijn naar een bug of een specifieke casus proberen mogelijk te maken binnen het CMS. Hij erkent dat het CMS, hoewel functioneel, erg ongestructureerd is. Volgens hem beperkt dit niet alleen onze flexibiliteit bij het realiseren van maatwerkoplossingen, maar ook de mogelijkheid om een grondige test-suite voor het systeem op te zetten.

Een voorbeeld van deze gesprekken vond plaats tijdens een onderzoek naar hoe we de waarden die voor een gebruiker zichtbaar zijn in een selectieveld binnen het CMS kunnen filteren. Tijdens dit onderzoek ontdekten we dat er voor verschillende soorten selectievelden niet alleen verschillende syntaxis beschikbaar is, maar ook dat de mogelijkheden voor het filteren bij de ene beperkter zijn dan bij de andere. Wilco legde mij uit dat dit probleem in andere websites was opgelost door een nieuw model aan te maken met een global scope die voldoet aan de eisen van het filter dat we proberen op te zetten en de relatie voor het veld hierop te baseren. Naar mijn idee een omslachtige oplossing die indruist tegen best practices, maar wel een typische pragmatische oplossing die vaak door developers bij AllesOnline wordt gedaan om bepaalde cases te realiseren.

## Gesprek tijdens realisatie van Filament CMS

In een gesprek met Wilco, waarin ik kort uitlegde hoe pagina’s en blokken met CMS-content toegepast kunnen worden in een CMS met Filament, bespraken we een aantal onderwerpen. Wilco gaf aan dat hij het prettig vond dat de codebase van Filament grotendeels in PHP is geschreven, wat autocompletion mogelijk maakt — iets wat met XML beperkter en minder intuïtief is. Daarnaast vond hij het belangrijk dat de overhead voor developers wordt verminderd. In Filament zou dit bereikt kunnen worden door, in plaats van telkens een FormField te definiëren en dezelfde functies eraan te koppelen, een veelgebruikt FormField met deze specifieke functies in een factory te abstraheren. Zo kan een developer eenvoudiger dit veelgebruikte FormField definiëren.

Toen ik hem liet zien hoe, naar mijn idee, de content voor het CMS gepersisteerd kon worden — namelijk in een JSON, waarbij de content als key-value pairs in het Page-model werd opgeslagen — gaf hij aan dat dit mogelijk niet schaalbaar genoeg zou zijn wanneer er veel content op een pagina voorkomt. Helemaal op het moment dat de content in meerdere talen beschikbaar moet zijn. Daarnaast was het op deze manier ook niet mogelijk om content te koppelen aan objecten die geen Page waren, tenzij deze objecten allemaal een 'content'-attribute in de database toegewezen kregen.

Naar aanleiding van deze feedback ben ik gaan onderzoeken of het mogelijk is de content onder een eigen model te persisteren. Dit heb ik gerealiseerd door een kleine aanpassing te maken in de manier waarop ik de key-value pairs op dat moment persisteerde. In plaats van de content als JSON in het Page-model op te slaan, heb ik nu een polymorfe relatie gecreëerd tussen het object dat content kan bevat en een Content-object.

## Gesprek rond afronding project

In de laatste week van het project heb ik met Wilco gesproken over de resultaten van het onderzoek en de realisatie van de CMS'en. Tijdens dit gesprek bespraken we de voordelen en nadelen van Filament en Statamic die ik tijdens mijn onderzoek heb geconstateerd.

Wilco benoemde tijdens het gesprek meerdere punten, waar hij zowel positieve als kritische kanttekeningen bij plaatste.

**Positief over Filament:**
- De flexibiliteit van Filament, met name dat de CMS schema's op een laag niveau nog aan te passen zijn omdat deze in PHP gedefineerd worden. 
- Ondersteuning voor autocompletion in de CMS-schema's, omdat deze in PHP worden gedefinieerd.
- De tools die Filament beschikbaar stelt om lifecycle van de gegevensopslag te wijzigen.
- De mogelijkheid om Filament ook in te zetten voor klanten die niet perse een CMS nodig hebben, maar een adminpanel
- Relatief duidelijkere documentatie in tegenstelling to die van Statamic

**Negatief over Filament:**
- Dat het best practice is om custom compontenten in Filament met Livewire en Alpine.js te realiseren. 

**Positief Statamic:**
- Lijkt vrij gemakkelijk om op te zetten voor klanten die een eenvoudige CMS nodig hebben voor een simpele content-website.
- Mogelijkheid om, wanneer monolitisch gewerkt wordt, gemakkelijk live-previews voor klanten mogelijk te maken.


**Negatief Statamic:** 
- Dat Statamic nog achterloopt met het gebruik van Vue2.
- Beperkte mogelijkheden wanneer een relatie complexer is, bijvoorbeeld wanneer er pivot-data toegevoegd moet worden.
- Het gebruik van YAML voor het definiëren van schema's. 
- Dat er door het gebruik van YAML er geen autocompletion voor het maken van de schema's is. 
- Ontoegangkelijke documentatie waar niet alles gemakkelijk te vinden blijkt te zijn.

Wilco gaf ook aan dat hij het niet als noodzakelijk ziet om een definitieve keuze te maken tussen **Filament** en **Statamic** voor toekomstige projecten. Voor eenvoudige contentwebsites zonder complexe relaties of logica is **Statamic** een geschikte optie. Voor webapplicaties met complexere vereisten biedt **Filament** meer mogelijkheden dankzij de flexibele opzet. Bij grotere projecten waarbij een klant zowel een contentwebsite als een webapplicatie vraagt, kan een combinatie van **Filament** en **Statamic** zelfs een overweging zijn.

# Yoran van Driel

## Algemene mening over het CMS

Yoran - een van onze nieuwe medior developers met ervaring bij andere webbureaus - vindt het CMS verouderd. Hij waardeert echter de modulariteit van het systeem, zoals de mogelijkheid om bijvoorbeeld SEO-instellingen te configureren. Het inbouwen van maatwerk is naar zijn idee wel tijdrovend. Wanneer je alleen gebruikmaakt van de beschikbare functies in het CMS, werkt het systeem prettig, maar zodra specifieke aanpassingen nodig zijn, wordt het een uitdaging om dit goed te realiseren. Hij heeft zelf nog niet eerder aanpassingen in het CMS hoeven doorvoeren en werkt er in de meeste gevallen omheen door een alternatieve oplossing te bedenken.

Volgens Yoran is het CMS op zich een goed stuk maatwerk dat aangepast kan worden. Toch wordt er volgens hem besloten om deze aanpassingen niet door te voeren. Hij denkt dat dit komt doordat men ervan uitgaat dat bepaalde functionaliteiten niet voor alle klanten nodig zijn. Yoran is echter van mening dat deze functionaliteiten juist als verkoopargumenten kunnen dienen om klanten te overtuigen een website te laten ontwikkelen bij AllesOnline.

Wat betreft de developer experience zou hij het ook fijn vinden als er in het CMS minder tijdrovende taken waren om bijvoorbeeld een objectmanager, template of block beschikbaar te maken.

## Gesprek rond afronding project

Tijdens het gesprek met Wilco sloot ook Yoran aan. Op dat moment hadden we het niet per se meer over de voor- en nadelen van beide oplossingen, maar hij kon zich wel vinden in de conclusie die Wilco trok: dat we niet per se een definitieve keuze hoeven te maken tussen **Filament** en **Statamic**. Hij voegde daaraan toe dat de mogelijkheid van beide systemen om custom packages te maken een positieve bijkomstigheid is. Dit stelt ons niet alleen in staat om op maat gemaakte oplossingen eenvoudig over verschillende projecten te hergebruiken, maar maakt het ook mogelijk om het basis-CMS zo kaal mogelijk te houden.