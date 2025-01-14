# **Requirements**

# Inleiding

Om vast te stellen aan welke eisen een CMS moet voldoen voor een typische AllesOnline-website, heb ik op basis van gesprekken met developers en een analyse van het AllesOnline CMS de volgende lijst met requirements opgesteld. Deze requirements vormen de basis voor het ontwikkelen van een prototype, zowel met het AllesOnline CMS als met andere systemen.

# Requirements

| **Vereiste**                  | **Beschrijving**                                                                        | **MoSCoW**  |
| ----------------------------- | --------------------------------------------------------------------------------------- | ----------- |
| **Blocks FormField**          | Dynamische content blokken.                                                             | Must Have   |
| **Date FormField**            | Datumvelden met ondersteuning voor verschillende datumnotaties en selectie.             | Must Have   |
| **HTML / RichText FormField** | Velden voor het invoeren en bewerken van HTML-inhoud met een WYSIWYG-editor.            | Must Have   |
| **Number FormField**          | Ondersteuning voor numerieke velden en validatie van getallen.                          | Must Have   |
| **Relation FormField**        | Ondersteuning voor relaties tussen objecten.                                            | Must Have   |
| **Select FormField**          | Dropdown selectielijsten waarin vooraf gedefinieerde opties geselecteerd kunnen worden. | Must Have   |
| **Tags FormField**            | Selectielijst waarin meerdere vooraf gedefineerde opties geselecteerd kunnen worden.    | Must Have   |
| **Text FormField**            | Standaard tekstvelden voor korte tekstinvoer.                                           | Must Have   |
| **Relations Form Field**      | Veld om relaties tussen verschillende objecten binnen het CMS te defineren              | Must Have   |
| **Textarea FormField**        | Ondersteuning voor lange tekstvelden (meerdere regels).                                 | Must Have   |
| **URL FormField**             | Ondersteuning voor URL-invoer met validatie.                                            | Must Have   |
| **Media-item FormField**      | Ondersteuning voor het toevoegen van media (afbeeldingen).                              | Could Have   |
| **File FormField**            | Ondersteuning voor het toevoegen van bestanden                                          | Must Have   |
| **Relations module**          | Ondersteuning voor het zichtbaar maken van relaties tussen verschillende objecten.      | Must Have   |
| **Maps FormField**            | Ondersteuning voor het beheren van een Google Maps functionaliteiten                              | Must have   |
| **Video FormField**           | Ondersteuning voor het uploaden en beheren van embedded video's binnen het CMS.         | Should Have |
| **OAuth2 FormField**          | Ondersteuning voor OAuth2.                                                              | Should have |

## Niet-Functionele Vereisten

| **Vereiste**                | **Beschrijving**                                                                                              |
| --------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Gebruiksvriendelijkheid** | Het systeem moet intuïtief en eenvoudig te bedienen zijn, ook voor niet-technische gebruikers.                |
| **Developer experience**    | Het systeem moet intuïtief en gemakkelijk te developen zijn.                                                  |
| **Flexibiliteit**           | Het CMS moet uitbreidbaar zijn met nieuwe velden en modules op basis van toekomstige of specifieke behoeften. |
| **Schaalbaarheid**          | Het CMS moet grote hoeveelheden content en gebruikersverzoeken aankunnen zonder prestatieproblemen.           |
| **Integraties**             | Ondersteuning voor integraties met externe systemen of API's die nodig zijn voor de AllesOnline-website.      |
