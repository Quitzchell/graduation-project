# **Gesprekken met developers rond mid-term review**

# Stefan Grevelink 

## Gesprek rond mid-term review

Tijdens de midterm reviews werd duidelijk dat ik met mijn examenproject niet goed op schema lag. Dit inzicht maakte me bewust van de noodzaak om mijn verantwoordelijkheden binnen AllesOnline anders aan te pakken. In plaats van voortdurend brandjes te blussen die door anderen worden veroorzaakt, besef ik dat ik de developers die verantwoordelijk zijn voor de opkomende issues hierover moet informeren en hen zelf de problemen moet laten oplossen.

Dit onderwerp kwam ook aan bod in een gesprek met Stefan naar aanleiding van de mid-term review. Hij bevestigde dat er niet van mij wordt verwacht dat ik problemen oplos waarvoor ik oorspronkelijk niet verantwoordelijk ben. Een betere aanpak zou zijn om bijvoorbeeld hem op de hoogte te brengen van de gevonden issues, of de verantwoordelijke developer direct aan te spreken op de bottlenecks die zij veroorzaken.

Doordat ik dit eerder niet deed, heb ik binnen een aantal projecten van AllesOnline flink wat overuren gemaakt. Hierdoor heb ik de tijd die ik voor mijn examenproject had, niet optimaal benut. In het gesprek met Stefan heb ik dan ook aangegeven dat ik bewuster met mijn tijd zal omgaan, het aantal overuren zal beperken en de beschikbare tijd voor mijn examenproject effectief zal inzetten. Dit zodat ik de verloren tijd in kan halen en kan voorkomen dat ik mezelf tijdens een werkweek overbelast.

## Gesprek rond afronding van CMS met Filament

Rond de afronding van het prototype CMS met Filament benadrukte Stefan opnieuw het belang van het onderzoeken of er een gemakkelijke manier is om van het AllesOnline CMS naar het Filament CMS te migreren. Naar aanleiding van deze opmerking besloot ik, voordat ik mijn onderzoek naar Statamic begin, eerst een spike te doen om de mogelijkheden tot migratie te verkennen.

# Wilco Kuijpers

## Gesprek tijdens realisatie van Filament CMS

In een gesprek met Wilco, waarin ik kort uit kon leggen hoe pagina’s en blokken met CMS-content toegepast kunnen worden in een CMS met Filament, bespraken we een aantal onderwerpen. Wilco gaf aan dat hij het prettig vond dat de codebase van Filament grotendeels in PHP is geschreven, wat autocompletion mogelijk maakt — iets wat met XML beperkter en minder intuïtief is. Daarnaast vond hij het belangrijk dat de overhead voor developers wordt verminderd. In Filament zou dit bereikt kunnen worden door, in plaats van telkens een FormField te definiëren en dezelfde functies eraan te koppelen, een veelgebruikt FormField met deze specifieke functies in een factory te abstraheren. Zo kan een developer eenvoudiger dit veelgebruikte FormField definiëren.

Toen ik hem liet zien hoe, naar mijn idee, de content voor het CMS gepersisteerd kon worden — namelijk in een JSON, waarbij de content als key-value pairs in het Page-model werd opgeslagen — gaf hij aan dat dit mogelijk niet schaalbaar genoeg zou zijn wanneer er veel content op een pagina voorkomt. Helemaal op het moment dat de content in meerdere talen beschikbaar moet zijn. Daarnaast was het op deze manier ook niet mogelijk om content te koppelen aan objecten die geen Page waren, tenzij deze objecten allemaal een 'content'-attribute in de database toegewezen kregen.

Naar aanleiding van deze feedback ben ik gaan onderzoeken of het mogelijk is de content onder een eigen model te persisteren. Dit heb ik gerealiseerd door een kleine aanpassing te maken in de manier waarop ik de key-value pairs op dat moment persisteerde. In plaats van de content als JSON in het Page-model op te slaan, heb ik nu een polymorfe relatie gecreëerd tussen het object dat content kan bevat en een Content-object.