# Onderzoek naar tool(s) voor migratie

## Inleiding

Dit document bevat inzichten en overwegingen die zijn opgedaan tijdens het onderzoek naar beschikbare tools voor het migreren van het AllesOnline CMS naar een nieuwe CMS-oplossing.

## Kiezen voor de juiste tool

Bij het kiezen van een tool of framework voor de realisatie heb ik verschillende opties onderzocht, waaronder PHP-Rector, Laravel en Symfony.

### PHP-Rector

Binnen AllesOnline is PHP-Rector meerdere keren genoemd als hulpmiddel voor het automatiseren van een Laravel- en/of PHP-versie-update. PHP-Rector is een tool die het proces van refactoren en upgraden van PHP-code automatiseert, met als doel ontwikkelaars tijd en moeite te besparen door repetitieve taken over te nemen. Dit wordt bereikt door middel van regels die via een configuratie in bepaalde sets kunnen worden gespecificeerd.

Het is mogelijk om voor deze tool op maat gemaakte regels te definiëren. Echter, omdat PHP-Rector niet specifiek is ontworpen om bestanden te transformeren (bijvoorbeeld van een XML-bestand naar een PHP-klasse), kan dit vrij complex worden. Het advies is daarom om PHP-Rector vooral te gebruiken waarvoor het in eerste instantie bedoeld is: het updaten van PHP- en/of Laravel-versies.

### Laravel

Een andere optie voor het migreren van het AllesOnline CMS naar een nieuw systeem was het ontwikkelen van een op maat gemaakte CLI-applicatie. Hiervoor heb ik in eerste instantie Laravel overwogen. Het framework biedt namelijk de mogelijkheid om als CLI-applicatie te functioneren, wat in principe voldoende is om een migratie te realiseren. Daarnaast is het voor de developers binnen AllesOnline een bekend framework, wat de samenwerking vergemakkelijkt en de leercurve verkleint.

Met Laravel zou het mogelijk zijn om bestanden uit een bestaande directory te lezen en deze te controleren. Waar nodig kunnen we de bestanden aanpassen om ze geschikt te maken voor het nieuwe CMS. Vervolgens kan een nieuw project worden opgezet in een andere directory, waarbij de gewijzigde gegevens worden overgezet naar het nieuwe systeem.

### Symfony

Omdat het gebruik van het Laravel Framework naar mijn mening overkill is voor een CLI-tool die uitsluitend lokaal nodig is voor het omzetten van een AllesOnline CMS-project naar een project met een nieuw CMS, heb ik besloten om te onderzoeken of er alternatieve frameworks bestaan waarmee je eenvoudiger een CLI-tool kunt realiseren zonder onnodige boilerplate.

Na wat onderzoek kwam ik uit bij Symfony, een van de frameworks waarvan Taylor Otwell (de bedenker van Laravel) componenten gebruikt om de CLI-functionaliteit in Laravel te implementeren. Om die reden zou een CLI-applicatie met Symfony waarschijnlijk snel herkenbaar en begrijpelijk zijn voor iemand met ervaring in Laravel.\

Omdat Symfony minder "opinionated" is dan Laravel, maakt het de mogelijkheid om losse componenten van het framework te gebruiken zonder de volledige boilerplate mee te nemen. Dit zorgt ervoor dat het project relatief klein en flexibel kan worden opgebouwd.

## Conclusie

Na onderzoek en afweging van de verschillende opties heb ik besloten om voor Symfony te kiezen als migratietool. De keuze is voornamelijk gebaseerd op de behoefte aan een lichte, flexibele oplossing voor het bouwen van een CLI-tool, zonder de uitgebreide boilerplate die bij Laravel hoort.

Symfony biedt de mogelijkheid om specifieke componenten te gebruiken zonder het volledige framework te implementeren. Dit maakt het ideaal voor het realiseren van een eenvoudige CLI-applicatie die uitsluitend lokaal nodig is. De modulaire opzet van Symfony zorgt voor een efficiënte aanpak, wat de ontwikkelingstijd ten goede komt. Daarnaast zorgt de bekendheid van de componenten bij ontwikkelaars met ervaring in Laravel ervoor dat de leercurve relatief klein blijft, terwijl de flexibiliteit en schaalbaarheid van Symfony de mogelijkheid bieden om het project eenvoudig uit te breiden indien nodig.

Los daarvan zie ik wel een rol voor het gebruik van PHP-Rector in het automatiseren van PHP- en Laravel-versie-updates. Dit zou eventueel vóór of na het migreren van een AllesOnline CMS-project kunnen worden uitgevoerd, om naast het migreren van het CMS-systeem ook de PHP- en Laravel-versies up-to-date te brengen.