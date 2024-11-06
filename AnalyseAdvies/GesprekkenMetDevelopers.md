# Stefan Grevelink 

* Initiële gesprek aan start van project

Stefan is de eigenaar en directeur van AllesOnline, een bedrijf dat gespecialiseerd is in weboplossingen. Ongeveer tien jaar geleden heeft hij zelf een Content Management Systeem (CMS) ontwikkeld, wat tot op de dag van vandaag een belangrijk onderdeel van zijn bedrijf vormt. In gesprekken met Stefan komt naar voren dat zijn beslissing om een eigen CMS te bouwen destijds niet uit de lucht kwam vallen. Op het moment dat hij dit project startte, begon het Laravel-framework net populariteit te winnen onder PHP-webontwikkelaars. Dit nieuwe framework werd geprezen om zijn eenvoud en flexibiliteit, en leek een goede basis voor toekomstgerichte webontwikkelingen.

Volgens Stefan was er op dat moment naar zijn idee geen kwalitatief, breed beschikbaar CMS van derde partijen dat goed aansloot bij Laravel. Deze situatie leidde ertoe dat hij besloot een eigen CMS te ontwikkelen, specifiek gericht op een optimale samenwerking met Laravel. Dankzij deze beslissing kon Stefan zijn klanten een op maat gemaakte oplossing bieden die zowel gebruiksvriendelijk als goed te onderhouden was, en bovendien kon meegroeien met de veranderingen binnen het Laravel-ecosysteem.

Vandaag de dag blijft het CMS een belangrijk onderscheidend kenmerk van AllesOnline, waarmee het bedrijf zich weet te positioneren in een concurrerende markt. Stefan kijkt tevreden terug op zijn keuze, die zijn bedrijf niet alleen een uniek product opleverde voorsprong, maar ook de mogelijkheid bood om klanten op een flexibele en efficiënte manier te bedienen. 

Echter, is Stefan zich er ook van bewust dat het onderhouden van een eigen CMS een kostbare en intensieve bezigheid is. Dit besef heeft ertoe geleid dat hij wilt laten onderzoekt wat de mogelijkheden zijn om over te stappen naar CMS-pakketten van derde partijen. Door gebruik te maken van een extern CMS zou hij niet alleen de onderhoudskosten kunnen verlagen, maar mogelijk ook profiteren van de doorontwikkeling en ondersteuning die deze pakketten bieden. Dit zou AllesOnline in staat stellen om zich nog sterker te richten op haar kernactiviteiten en de klantbehoeften, zonder de operationele lasten van het onderhouden van een eigen systeem.

Een belangrijke laatste wens van Stefan is dat hij het belangrijk vindt om het voor klanten mogelijk te maken om niet een geheel nieuwe project te realiseren. Voor zover het mogelijk is, zou hij graag in het onderzoek willen zien of het mogelijk is om een migratie van het huidige CMS naar een nieuwe CMS te maken.

# Wilco Kuipers

* Algemeen
 
Wilco werkt al ongeveer acht jaar bij AllesOnline en is onze senior software developer. In de gesprekken die ik tijdens werktijd met hem voer, komt het CMS regelmatig ter sprake. Deze gesprekken vinden vooral plaats wanneer we samen in de wirwar van spaghetticode op zoek zijn naar een bug of een specifieke casus proberen mogelijk te maken binnen het CMS. Hij erkent dat het CMS, hoewel functioneel, erg ongestructureerd is. Volgens hem beperkt dit niet alleen onze flexibiliteit bij het realiseren van maatwerkoplossingen, maar ook de mogelijkheid om een grondige test-suite voor het systeem op te zetten.

* Gesprek rondom realisatie van Filament CMS

In een gesprek met Wilco, waarin ik kort uitlegde hoe pagina’s en blokken met CMS-content toegepast kunnen worden in een CMS met Filament, bespraken we een aantal onderwerpen. Wilco gaf aan dat hij het prettig vond dat de codebase van Filament grotendeels in PHP is geschreven, wat autocompletion mogelijk maakt — iets wat met XML beperkter en minder intuïtief is. Daarnaast vond hij het belangrijk dat de overhead voor developers wordt verminderd. In Filament zou dit bereikt kunnen worden door, in plaats van telkens een FormField te definiëren en dezelfde functies eraan te koppelen, een veelgebruikt FormField met deze specifieke functies in een factory te abstraheren. Zo kan een developer eenvoudiger dit veelgebruikte FormField definiëren.