# **Advies CMS migratie**

# Inleiding
Vanwege de beperkte tijd om een grondig onderzoek te doen naar een migratie van het AllesOnline CMS naar Statamic, heb ik ervoor gekozen dit niet te realiseren. Hoewel er geen PoC beschikbaar is, ben ik ervan overtuigd dat een migratie naar Statamic mogelijk is. Toch wil ik het advies geven om niet te makkelijk over dit soort migraties te denken. Er zijn verschillende factoren die het migreren van echte AllesOnline-projecten naar een nieuw systeem complexer zullen maken.


# Verschil in architectuur
In de loop der jaren zijn er verschillende benaderingen geweest voor het opzetten van projecten. Zo zijn er monolitische projecten met blade-templates, quasi-monolitische projecten met intertia en decoupled projecten waar de backend api's beschikbaar stellen voor een nextjs frontend. De diversiteit aan de manier waarop informatie binnen deze architecturen vanuit het CMS opgevraagd worden om doorgestuurd te worden aan de frontend zorgt ervoor dat er zorgvuldig onderzocht wordt welke technieken er gebruikt worden en hoe deze geautomatiseerd omgezet kunnen worden.

# Verschil in verantwoordelijkheid voor validatie-regels
In het AllesOnline CMS worden de validatieregels voor de invoervelden van entiteiten gedefinieerd in de Eloquent-modellen. Deze enigszins unieke benadering maakt het migreren van de validatieregels naar zowel Filament als Statamic complexer. In beide systemen worden de validatieregels namelijk gedefinieerd in de CMS-schema's. Het probleem zit hem in het feit dat bestanden doorgaans één voor één worden overgezet, maar de validatieregels op een specifieke manier moeten worden geïntegreerd.

# Conclusie
Gezien de complixiteit die naar voren komt in de verschillende soorten projecten die AllesOnline heeft, is het belangrijk om voor een migratie een goede voorbereiding te doen. Onderzoek welke keuzes gemaakt zijn en hoe deze opgelost kunnen worden naar het nieuwe CMS. Uiteraard zou het een groot gewin zijn als er meer uniformiteit komt in de manier waarop gegevens in het CMS gebruikt worden. Dit zal helpen om eventuele migraties in de toekomst soepeler te laten verlopen.

