# **Onderzoek voor CMS migratie**

# Inleiding

Dit document bevat inzichten en aanbevelingen, die zijn verkregen tijdens het onderzoek naar beschikbare tools voor de migratie van het AllesOnline CMS naar een nieuw CMS. Bij het kiezen van een tool of framework voor de migratie zijn verschillende opties onderzocht, waaronder **PHP-Rector**, **Laravel** en **Symfony**.

# PHP-Rector

Binnen AllesOnline is **PHP-Rector** meerdere keren genoemd als tool voor het automatiseren van versie-updates van Laravel en/of PHP. PHP-Rector is een tool die het proces van refactoren en upgraden van PHP-code automatiseert, met als doel developers tijd en moeite te besparen door repetitieve taken over te nemen. Dit wordt bereikt door middel van regels die via een configuratie in specifieke sets kunnen worden gedefinieerd.

Hoewel het mogelijk is om op maat gemaakte regels voor deze tool te maken, is het belangrijk op te merken dat PHP-Rector niet specifiek is ontworpen om bestanden te transformeren (bijvoorbeeld van een XML-bestand naar een PHP-class). Het gebruik van PHP-Rector voor dergelijke complexe taken is daarom niet de eerste keus. Het advies is om PHP-Rector vooral te gebruiken voor het doel waarvoor het oorspronkelijk is ontworpen: het updaten van PHP- en/of Laravel-versies.

# Maatwerk

Voor de migratie van het AllesOnline CMS naar een nieuw systeem is het beter om gebruik te maken van een op maat gemaakte CLI-applicatie. Voor deze opties is gekeken naar Laravel en Symfony.

## Laravel

**Laravel** biedt de mogelijkheid om als CLI-applicatie te functioneren, wat voldoende is om de migratie te realiseren. Bovendien is Laravel voor de developers binnen AllesOnline een vertrouwd framework. Dit betekent dat er voor het werken met het framework theoretisch gezien geen leercurve is.

Echter, het gebruik van Laravel kan ook als overkill worden beschouwd voor eenvoudige CLI-applicaties die slechts lokaal draaien in een Docker-container. In dit geval kan een meer lichtgewicht oplossing, zoals een eenvoudige PHP-script of een minimalistisch CLI-framework, de benodigde functionaliteit bieden zonder de extra boilerplating die Laravel met zich meebrengt.

## Symfony

**Symfony** is een minder **opinionated** framework dan Laravel, dat developers in staat stelt de losse componenten van het framework te gebruiken zonder de volledige boilerplate mee te nemen. Sterker nog, het consolecomponent van Symfony wordt gebruikt om de CLI-functionaliteiten van Laravel te realiseren (Akter, 2024). Om deze reden zouden we kunnen stellen dat het gebruik van Symfony voor het realiseren van een CLI-applicatie geen steile leercurve heeft voor developers met ervaring in Laravel.

# Conclusie

Na de bovengenoemde drie opties te hebben onderzocht, is besloten om voor een Proof of Concept gebruik te maken van **Symfony**. Deze keuze is voornamelijk gebaseerd op de behoefte aan een lichte en flexibele oplossing voor het realiseren van een migratietool. Symfony biedt deze mogelijkheid door specifieke componenten te gebruiken, zonder het volledige framework te hoeven importeren. Dit maakt het ideaal voor het ontwikkelen van een compacte en overzichtelijke CLI-applicatie. Daarnaast zorgt de bekendheid van deze componenten bij ontwikkelaars met ervaring in Laravel ervoor dat de leercurve relatief klein blijft.

Naast de migratietool zie ik ook een rol voor **PHP-Rector** bij het automatiseren van PHP- en Laravel-versie-updates. Dit zou eventueel vóór of na het migreren van een AllesOnline CMS-project kunnen worden uitgevoerd, om naast het migreren van het CMS-systeem ook de PHP- en Laravel-versies up-to-date te brengen.

# Referenties

> Akter, A. (2024, oktober 22). Laravel Requires Require Symfony/Console: A Deep Dive. Medium. [https://medium.com/@asmatanhabd/laravel-requires-require-symfony-console-a-deep-dive-546d7d89b4a8](https://medium.com/@asmatanhabd/laravel-requires-require-symfony-console-a-deep-dive-546d7d89b4a8)