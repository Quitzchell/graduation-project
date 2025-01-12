# **Advies CMS migratie**

# Inleiding
In dit document deel ik nieuwe inzichten over het migreren van het AllesOnline CMS naar Filament. Met dit document wil ik benadrukken dat het migreren van echte AllesOnline-projecten naar een nieuw systeem complexer is dan het lijkt. Er spelen diverse factoren die in acht moeten worden genomen om een succesvolle migratie te garanderen.

# Verschil in architectuur
In de loop der jaren zijn er verschillende benaderingen geweest voor het opzetten van AllesOnline-projecten. Er zijn monolithische projecten met `Blade`-templates, quasi-monolithische projecten met `Inertia`, en projecten met een *decoupled architecture* waarin de backend API’s beschikbaar stelt voor een frontend die gebruikmaakt van een modern frontend-framework. De diversiteit in de manier waarop informatie binnen deze architecturen wordt verwerkt, vereist dat er per project zorgvuldig onderzocht moet worden welke technieken zijn gebruikt en hoe deze automatisch kunnen worden overgezet met behulp van een tool.

# Verschil in verantwoordelijkheid voor validatie
In het AllesOnline CMS worden validatieregels voor invoervelden in de Eloquent-modellen gedefinieerd, terwijl deze in Filament in schema's aan `Form`-componenten worden gechained. Dit maakt het migreren van de regels complex omdat de migratietool bestanden in principe één voor één overzet, maar validatieregels en schema's nu uit verschillende bestanden moeten worden samengevoegd. Dit maakt het ontwikkelen van een migratietool uitdagender.

# Conclusie
Gezien de complexiteit die naar voren komt in de verschillende soorten AllesOnline projecten, is het belangrijk om voor een migratie een goede voorbereiding te doen. Onderzoek welke keuzes gemaakt zijn en hoe deze opgelost kunnen worden naar het nieuwe CMS. 

Begin daarnaast met kleinere projecten en werk op deze manier iteratief naar grotere en uitdagendere projecten. Op deze manier wordt het fundament van de tool iteratief uitgebreid en verbeterd.

Daarnaast zou het een groot gewin zijn als er meer uniformiteit komt in de architectuur manier waarop gegevens in de backend verwerkt worden.

