# Stefan Grevelink 

## Initiële gesprek aan start van project

Stefan is de eigenaar en directeur van AllesOnline, een bedrijf dat gespecialiseerd is in weboplossingen. Ongeveer tien jaar geleden heeft hij zelf een Content Management Systeem (CMS) ontwikkeld, wat tot op de dag van vandaag een belangrijk onderdeel van zijn bedrijf vormt. In gesprekken met Stefan komt naar voren dat zijn beslissing om een eigen CMS te bouwen destijds niet uit de lucht kwam vallen. Op het moment dat hij dit project startte, begon het Laravel-framework net populariteit te winnen onder PHP-webontwikkelaars. Dit nieuwe framework werd geprezen om zijn eenvoud en flexibiliteit, en leek een goede basis voor toekomstgerichte webontwikkelingen.

Volgens Stefan was er op dat moment naar zijn idee geen kwalitatief, breed beschikbaar CMS van derde partijen dat goed aansloot bij Laravel. Deze situatie leidde ertoe dat hij besloot een eigen CMS te ontwikkelen, specifiek gericht op een optimale samenwerking met Laravel. Dankzij deze beslissing kon Stefan zijn klanten een op maat gemaakte oplossing bieden die zowel gebruiksvriendelijk als goed te onderhouden was, en bovendien kon meegroeien met de veranderingen binnen het Laravel-ecosysteem.

Vandaag de dag blijft het CMS een belangrijk onderscheidend kenmerk van AllesOnline, waarmee het bedrijf zich weet te positioneren in een concurrerende markt. Stefan kijkt tevreden terug op zijn keuze, die zijn bedrijf niet alleen een uniek product en daarmee een voorsprong opleverde, maar ook de mogelijkheid bood om klanten op een flexibele en efficiënte manier te bedienen. 

Echter, is Stefan zich er ook van bewust dat het onderhouden van een eigen CMS een kostbare en intensieve bezigheid is. Dit besef heeft ertoe geleid dat hij wilt laten onderzoekt wat de mogelijkheden zijn om over te stappen naar CMS-pakketten van derde partijen. Door gebruik te maken van een extern CMS zou hij niet alleen de onderhoudskosten kunnen verlagen, maar mogelijk ook profiteren van de doorontwikkeling en ondersteuning die deze pakketten bieden. Dit zou AllesOnline in staat stellen om zich nog sterker te richten op haar kernactiviteiten en de klantbehoeften, zonder de operationele lasten van het onderhouden van een eigen systeem.

Een belangrijke laatste wens van Stefan is dat hij het belangrijk vindt om het voor klanten mogelijk te maken om niet een geheel nieuwe project te realiseren. Voor zover het mogelijk is, zou hij graag in het onderzoek willen zien of het mogelijk is om een migratie van het huidige CMS naar een nieuwe CMS te maken.

## Gesprek rond mid-term review

Tijdens de mid-term reviews werd duidelijk dat ik met mijn examenproject niet goed op schema lag. Dit besef maakte mij ervan bewust dat ik mijn verantwoordelijkheden binnen AllesOnline anders moest aanpakken. In plaats van voortdurend brandjes te blussen die door anderen zijn veroorzaakt, zou ik de developers die verantwoordelijk zijn voor de issues moeten informeren over wat er binnen een project fout gaat.

Dit onderwerp kwam ook aan bod in een gesprek met Stefan naar aanleiding van de mid-term review. Hij bevestigde dat er niet van mij wordt verwacht dat ik problemen oplos waarvoor ik oorspronkelijk niet verantwoordelijk ben. Een betere aanpak zou zijn om bijvoorbeeld hem of de verantwoordelijke developer op de hoogte te brengen van de gevonden issues, of hen direct aan te spreken op de zaken die zij hadden moeten oplossen.

Doordat ik dat eerder niet deed, heb ik binnen een aantal projecten van AllesOnline flink wat overuren gemaakt. Hierdoor heb ik de tijd die ik voor mijn examenproject had, niet goed benut. In het gesprek met Stefan heb ik aangegeven dat ik voortaan bewuster zal omgaan met mijn tijd. Ik wil het aantal overuren beperken en de beschikbare tijd voor mijn examenproject effectief gebruiken.

Op deze manier hoop ik de verloren tijd in te halen en te voorkomen dat ik mezelf tijdens de werkweek overbelast. Zo behoud ik ook voldoende energie om buiten werktijd aan mijn examenproject te werken.

## Gesprek rond afronding van CMS met Filament

Rond de afronding van het prototype CMS met Filament benadrukte Stefan opnieuw het belang van het onderzoeken of er een gemakkelijke manier is om van het AllesOnline CMS naar het Filament CMS te migreren. Naar aanleiding van deze opmerking besloot ik, voordat ik mijn onderzoek naar Statamic begin, eerst een spike te doen om dit onderzoek te verkennen.

Tijdens de afronding van het CMS met Filament merkte Stefan bovendien op dat er mogelijk een nieuwe opdracht aan komt, waarbij hij Filament zou willen gebruiken in plaats van het AllesOnline CMS. Dit komt doordat de wens van de klant steeds meer gericht is op het aanbieden van een CRM in plaats van een CMS.

# Wilco Kuijpers

## Algemene gesprekken
 
Wilco werkt al ongeveer acht jaar bij AllesOnline en is onze senior software developer. In de gesprekken die ik tijdens werktijd met hem voer, komt het CMS regelmatig ter sprake. Deze gesprekken vinden vooral plaats wanneer we samen in de wirwar van spaghetticode op zoek zijn naar een bug of een specifieke casus proberen mogelijk te maken binnen het CMS. Hij erkent dat het CMS, hoewel functioneel, erg ongestructureerd is. Volgens hem beperkt dit niet alleen onze flexibiliteit bij het realiseren van maatwerkoplossingen, maar ook de mogelijkheid om een grondige test-suite voor het systeem op te zetten.

Een voorbeeld van deze gesprekken vond plaats tijdens een onderzoek naar hoe we de waarden die voor een gebruiker zichtbaar zijn in een selectieveld binnen het CMS kunnen filteren. Tijdens dit onderzoek ontdekten we dat er voor verschillende soorten selectievelden niet alleen verschillende syntaxis beschikbaar is, maar ook dat de mogelijkheden voor het filteren bij de ene beperkter zijn dan bij de andere. Wilco legde mij uit dat dit probleem in andere websites was opgelost door een nieuw model aan te maken met een global scope die voldoet aan de eisen van het filter dat we proberen op te zetten en de relatie voor het veld hierop te baseren. Naar mijn idee een omslachtige oplossing die indruist tegen best practices, maar wel een typische pragmatische oplossing die vaak door developers bij AllesOnline wordt gedaan om bepaalde cases te realiseren.

## Gesprek tijdens realisatie van Filament CMS

In een gesprek met Wilco, waarin ik kort uitlegde hoe pagina’s en blokken met CMS-content toegepast kunnen worden in een CMS met Filament, bespraken we een aantal onderwerpen. Wilco gaf aan dat hij het prettig vond dat de codebase van Filament grotendeels in PHP is geschreven, wat autocompletion mogelijk maakt — iets wat met XML beperkter en minder intuïtief is. Daarnaast vond hij het belangrijk dat de overhead voor developers wordt verminderd. In Filament zou dit bereikt kunnen worden door, in plaats van telkens een FormField te definiëren en dezelfde functies eraan te koppelen, een veelgebruikt FormField met deze specifieke functies in een factory te abstraheren. Zo kan een developer eenvoudiger dit veelgebruikte FormField definiëren.

Toen ik hem liet zien hoe, naar mijn idee, de content voor het CMS gepersisteerd kon worden — namelijk in een JSON, waarbij de content als key-value pairs in het Page-model werd opgeslagen — gaf hij aan dat dit mogelijk niet schaalbaar genoeg zou zijn wanneer er veel content op een pagina voorkomt. Helemaal op het moment dat de content in meerdere talen beschikbaar moet zijn. Daarnaast was het op deze manier ook niet mogelijk om content te koppelen aan objecten die geen Page waren, tenzij deze objecten allemaal een 'content'-attribute in de database toegewezen kregen.

Naar aanleiding van deze feedback ben ik gaan onderzoeken of het mogelijk is de content onder een eigen model te persisteren. Dit heb ik gerealiseerd door een kleine aanpassing te maken in de manier waarop ik de key-value pairs op dat moment persisteerde. In plaats van de content als JSON in het Page-model op te slaan, heb ik nu een polymorfe relatie gecreëerd tussen het object dat content kan bevat en een Content-object.

# Yoran van Driel

## Algemene mening over het CMS

Yoran - een van onze nieuwe medior developers met ervaring bij andere webbureaus - vindt het CMS verouderd. Hij waardeert echter de modulariteit van het systeem, zoals de mogelijkheid om bijvoorbeeld SEO-instellingen te configureren. Het inbouwen van maatwerk is naar zijn idee wel tijdrovend. Wanneer je alleen gebruikmaakt van de beschikbare functies in het CMS, werkt het systeem prettig, maar zodra specifieke aanpassingen nodig zijn, wordt het een uitdaging om dit goed te realiseren. Hij heeft zelf nog niet eerder aanpassingen in het CMS hoeven doorvoeren en werkt er in de meeste gevallen omheen door een alternatieve oplossing te bedenken.

In zijn ogen is het CMS op zichzelf een degelijk stuk maatwerk dat aangepast kan worden, maar we kiezen er volgens hem te vaak voor om dit niet te doen. Yoran denkt dat dit komt omdat aangenomen wordt dat bepaalde functionaliteiten niet voor andere klanten nodig zijn. Hij is het hier echter niet mee eens, omdat hij ervan overtuigd is dat dit soort features juist als verkoopargumenten voor het CMS kunnen dienen en nuttige functionaliteiten bieden die verschillende klanten goed van pas kunnen komen.

Wat betreft de developer experience zou hij het ook fijn vinden als er in het CMS minder tijdrovende taken waren om bijvoorbeeld een objectmanager, template of block beschikbaar te maken.