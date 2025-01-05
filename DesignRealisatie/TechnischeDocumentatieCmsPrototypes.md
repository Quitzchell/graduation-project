# Technische Documentatie CMS Prototypes

Dit document geeft een overzicht over de werking van de verschillende Content Management Systemen en beschrijft hoe de ze opgebouwd worden.

## AllesOnline CMS

### ContentManagerController

In het AllesOnline CMS worden pagina’s en hun content beheerd via de `ContentManagerController`. Deze controller wordt meestal zonder aanpassingen geïmporteerd vanuit het AllesOnline CMS-pakket en biedt toegang tot de `ManagedContent`, `Page`, en `CMSContent`-modellen voor de configuratie van content-elementen.

**ContentManagerController in AO CMS**

```php
<?php

namespace App\Http\Controllers;

use ContentManager;

class ContentManagerController extends ContentManager
{
    // Controller implementatie
}
```

### XML-schema voor Templates met CMS-content

Binnen de `ContentManager` worden `Page`-objecten dynamisch gegenereerd via XML-templates die specificeren welke velden (zoals tekst, afbeeldingen en blokken) kunnen worden toegevoegd. Dit stelt developers in staat om template-schema's te definiëren voor CMS-gebruikers.

**XML-schema voor Template in AO CMS**

```xml
<template name="Home page">
    <field type="text" name="header_title" title="Header title"/>
    <field type="media-item" name="header_image" title="Header image"/>
    <field type="blocks" name="blocks" title="Content blokken"
		blocks="common/*"/>
</template>
```

In het bovenstaande template zijn velden voor een headerafbeelding en een titel opgenomen. Gebruikers kunnen daarnaast blokken selecteren om toe te voegen, wat flexibiliteit biedt bij het vormgeven van de pagina.

### Blok-schema's voor Blokken met CMS-content

Contentblokken kunnen ook worden gedefinieerd met XML en vervolgens in verschillende templates worden hergebruikt.

**XML-schema voor Blok in AO CMS**

```xml
<template name="Paragraph">
    <field type="text" name="title" title="Title"/>
    <field type="html" name="text" title="Text" height="40"
		toolbar="formatselect | bold italic underline | charmap | link"/>
</template>
```

Dit blok bevat een titel- en tekstveld met RichText-functionaliteit voor tekstopmaak.

## CMS met Filament

In Filament kan op een soortgelijke manier een CMS worden gerealiseerd door gebruik te maken van Resources en een Eloquent-model geinspireerd op het `Page`-model van AllesOnline CMS. Dit model bevat de gegevens en relaties van pagina’s en hun hiërarchie.

*Een UML class-diagram met het onderstaande concept voor content management kan je [hier bekijken](../Bijlagen/UmlEntiteitenDiagramContentManagementFilament.md)*

### Eloquent Model: Page

Het `Page`-model slaat paginagegevens in de database op en beheert relaties tussen pagina's.

**Page-model in CMS met Filament**

```php
<?php

namespace App\Models;

use App\Models\Interfaces\HasContent;
use App\Models\Interfaces\HasUrl;
use App\Models\Traits\ProvidesContentTrait;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\BelongsToMany;

class Page extends Model implements HasContent, HasUrl
{
    use ProvidesContentTrait;

    protected $table = 'pages';

    protected $fillable = [
        'name',
        'parent_id',
        'menu_id',
        'uri',
        'template',
        'content',
    ];

    protected $casts = [
        'content' => 'array',
    ];

    /* Relations */
    public function menus(): BelongsToMany
    {
        return $this->belongsToMany(Menu::class);
    }

    /* Urlable */
    public function uri($lang = null): string
    {
        return strtolower($this->name);
    }

    public function url($lang = null): string
    {
        return config('app.url').'Page.php/'.$this->uri($lang);
    }
}
```

### Interfaces

Het Page model implementeert verschillende interfaces die voorschrijven waar het model aan moet voldoen. 

#### HasContent

De `HasContent`-interface schrijft voor dat het model een relatie heeft met het Content-model, waarin de daarvoor aangewezen CMS-content wordt opgeslagen. In het Filament CMS wordt deze interface gecombineerd met de `ProvidesContentTrait`.

**HasContent interface in CMS met Filament**

```php
<?php

namespace App\Models\Interfaces;

use Illuminate\Database\Eloquent\Relations\MorphMany;

interface HasContent
{
    public function contents(): MorphMany;

    public function content(string $name): string|array|null;
}
```

#### HasUrl

De `HasUrl`-interface schrijft voor dat het model functionaliteit moet implementeren waarmee een `uri` en een `url` worden gegenereerd voor de objecten die uit het model voortkomen.

**HasUrl interface in het CMS met Filament**

```php
<?php

namespace App\Models\Interfaces;

interface HasUrl
{
    public function uri($lang = null): string;

    public function url($lang = null): string;
}
```

### Traits

#### ProvidesContentTrait

De `ProvidesContentTrait` bevat de logica voor de polymorfe relatie tussen het model dat `HasContent` implementeert en het Content-model. Door gebruik te maken van een **trait** kan deze logica op een eenvoudige manier worden hergebruikt. Als er andere functionaliteit nodig is, kan er een nieuwe trait worden gedefinieerd, of kunnen de functies uit de `HasContent`-interface direct in de betreffende class worden geïmplementeerd.

```php
<?php

namespace App\Models\Traits;

use App\Models\Content;
use Illuminate\Database\Eloquent\Relations\MorphMany;

trait ProvidesContentTrait
{
    public function contents(): MorphMany
    {
        return $this->morphMany(Content::class, 'contentable');
    }

    public function content(string $name): string|array|null
    {
        return $this->contents()->where('name', $name)->first()?->value;
    }
}
```

### Filament PageResource en Dynamische Templates

Met het `Page`-model kan een Filament `PageResource` een formulier genereren voor het beheren van pagina’s. Een select-veld stelt de gebruiker in staat om een template te kiezen, waarna het formulier dynamisch de velden van de bijbehorende template inlaadt.

**Schema voor Content Management in CMS met Filament**

```php
public static function form(Form $form): Form
    {
        $templateFactory = app(TemplateFactory::class);

        return $form
            ->schema([
                Forms\Components\Section::make()->schema([
                    Forms\Components\TextInput::make('name')
                        ->label('Page title')
                        ->required()
                        ->maxLength(255)
                        ->live(onBlur: true)
                        ->afterStateUpdated(static::createSlug('uri')),

                    Forms\Components\TextInput::make('uri')
                        ->label('Page uri')
                        ->unique(ignoreRecord: true)
                        ->required(),

					// Het select veld voor het selecteren van een template
                    Forms\Components\Select::make('template')
                        ->options($templateFactory->getTemplateNames())
                        ->required()
                        ->live(),

					// Het onderdeel van het formulier dat dynamisch wordt 
					//  ingeladen nadat de gebruiker een template selecteert
                    Forms\Components\Section::make()
                        ->schema(function (Get $get) use ($templateFactory) {
                            if ($get('template')) {
                                return 
	                                $templateFactory->loadTemplateSchema(
		                                $get('template')
		                            );
                            }

                            return [];
                        }),
                ]),
            ]);
    }
```

### TemplateFactory voor Dynamische Template-selectie

De `TemplateFactory`-class ondersteunt de selectie en dynamische weergave van velden op basis van het door de gebruiker gekozen template in de `PageResource`.

**TemplateFactory in CMS met Filament**

```php
<?php

namespace App\Cms;

use App\Cms\Templates\Interfaces\HasTemplateSchema;

readonly class TemplateFactory
{
    public function __construct(private array $templates) {}

    public function getTemplateFields(string $template): array
    {
        return $this->extractFieldNames($this->loadTemplateSchema($template));
    }

    protected function extractFieldNames(array $components): array
    {
        $fields = [];

        foreach ($components as $component) {
            if (method_exists($component, 'getName')) {
                $fields[] = $component->getName();
            }
        }

        return $fields;
    }

    public function getTemplateNames(): array
    {
        $formattedNames = [];

        foreach ($this->templates as $className => $templateClass) {
            if ($templateClass instanceof HasTemplateSchema) {
                $formattedNames[$className] = $templateClass->getName();
            }
        }

        return $formattedNames;
    }

    public function loadTemplateSchema(string $template): array
    {
        if (! class_exists($template)) {
            abort(404);
        }

        $templateClass = new $template;

        return $templateClass instanceof HasTemplateSchema
            ? $templateClass->getForm()
            : [];
    }
}
```

### Class voor Templates in CMS met Filament

Een `Template` wordt beheerd in een specifieke class die een array met Filament FormField-componenten teruggeven aan een `Resource`.

**Template-class in CMS met Filament**

```php
<?php

namespace App\Cms\Templates;

use App\Cms\Blocks\Common\CallToAction;
use App\Cms\Blocks\Common\Image;
use App\Cms\Blocks\Common\Map;
use App\Cms\Blocks\Common\Paragraph;
use App\Cms\Templates\Interfaces\HasTemplateSchema;
use Filament\Forms\Components\Builder;
use Filament\Forms\Components\FileUpload;
use Filament\Forms\Components\TextInput;

class HomeTemplate implements HasTemplateSchema
{
    public function getName(): string
    {
        return 'Homepage';
    }

    public function getForm(): array
    {
        return [
            TextInput::make('header_title')
                ->label('Header title'),

            FileUpload::make('header_image')
                ->label('Header Image')
                ->image()
                ->preserveFilenames()
                ->required(),

            Builder::make('blocks')
                ->schema([
                    CallToAction::getBlock(),
                    Image::getBlock(),
                    Map::getBlock(),
                    Paragraph::getBlock(),
                ]),
        ];
    }
}
```

Het bovenstaande sjabloon bevat een veld voor een headerafbeelding, een titel en biedt de mogelijkheid om contentblokken toe te voegen.

### Interfaces

Een `Template` implementeert de interface `HasTemplateSchema`. Deze interface vereist dat classes een schema voor hun CMS-content en een naam beschikbaar stellen. Het schema wordt verwacht in de `TemplateFactory`, terwijl de naam wordt gebruikt in het select-veld waarin gebruikers in de `Page`-resource een template kiezen.

**HasTemplateSchema interface in het CMS met Filament**

```php
<?php

namespace App\Cms\Templates\Interfaces;

interface HasTemplateSchema
{
    public function getName(): string;

    public function getForm(): array;
}
```

### Functionaliteit voor het aanmaken, bewerken en ophalen van CMS Content

Filament biedt naast `Resource`-classes, een soort van manager voor models, ook aparte classes aan waarin de logica wordt beheerd voor de pagina's waar gebruikers modellen kunnen aanmaken, bewerken en ophalen. Om ervoor te zorgen dat onze CMS-content daadwerkelijk als content wordt opgeslagen, is het noodzakelijk om een wijziging in de functionaliteit toe te passen op alle classes die content aanbieden. Hiervoor zijn Traits aangemaakt die – wanneer toegepast – de logica voor het persisteren en ophalen van CMS-content afhandelt.

**MutateDataBeforeCreateTrait**

```php
<?php

namespace App\Filament\Resources\Traits;

use App\Cms\TemplateFactory;
use Illuminate\Support\Arr;

trait MutateDataBeforeCreateTrait
{
    private array $cmsContent;

    protected function mutateFormDataBeforeCreate(array $data): array
    {
        $templateFactory = app(TemplateFactory::class);
        if (! empty($data['template'])) {
            $templateFields = 
	            $templateFactory->getTemplateFields($data['template']);
            $this->cmsContent = Arr::only($data, $templateFields);
        }

        return Arr::except($data, $templateFields ?? []);
    }

    protected function afterCreate(): void
    {
        if ($this->cmsContent) {
            foreach ($this->cmsContent as $name => $value) {
                $this->record->contents()->create([
		            'name' => $name,
		            'value' => $value
                ]);
            }
        }
    }
}
```

Om een object met bijbehorende content aan te maken, wordt eerst het hoofdmodel (bijvoorbeeld een `Page`) opgeslagen. Vervolgens wordt de content via een relatie opgeslagen in een apart Content-model dat naar het hoofdmodel verwijst. In het bovenstaande voorbeeld wordt in `mutateFormDataBeforeCreate()` de CMS-content uit het door de gebruiker ingevulde formulier gefilterd en tijdelijk opgeslagen tijdens het aanmaken van het hoofdmodel. Nadat het hoofdmodel succesvol is aangemaakt, wordt de content via een Eloquent-relatie in de `afterCreate()`-methode opgeslagen.

**MutateDataBeforeFill**Trait

```php
<?php

namespace App\Filament\Resources\Traits;

trait MutateDataBeforeFillTrait
{
    protected function mutateFormDataBeforeFill(array $data): array
    {
        if ($this->record->contents) {
            foreach ($this->record->contents as $templateData) {
                $data[$templateData->name] = $templateData->value;
            }
        }

        return $data;
    }
}
```

Wanneer een bestaand object wordt bewerkt, moet de bijbehorende content in het formulier worden geladen. In `mutateFormDataBeforeFill()` wordt de content die aan het object is gekoppeld opgehaald uit de `contents`-relatie en toegevoegd aan de formulierdata. Hierdoor worden de opgeslagen gegevens in de bijbehorende velden weergegeven, zodat de gebruiker de bestaande gegevens kan aanpassen zonder ze opnieuw in te voeren.

**MutateBeforeSave**Trait

```php
<?php

namespace App\Filament\Resources\Traits;

use App\Cms\TemplateFactory;
use Illuminate\Support\Arr;

trait MutateDateBeforeSaveTrait
{
    protected function mutateFormDataBeforeSave(array $data): array
    {
        $templateFactory = app(TemplateFactory::class);
        $templateFields = $templateFactory->getTemplateFields($data['template']);
        $cmsContent = Arr::only($data, $templateFields);

        foreach ($cmsContent as $name => $value) {
            $this->record->contents()->updateOrCreate(
                ['name' => $name],
                ['value' => $value]
            );
        }

        return Arr::except($data, $templateFields);
    }
}
```

Wanneer CMS-content wordt toegevoegd of bijgewerkt in een bestaand object, wordt het opslaan een stuk eenvoudiger. In dit geval hoeft alleen de relevante content uit de door de gebruiker ingevulde formuliergegevens te worden gehaald en via een Eloquent-relatie te worden opgeslagen. De gegevens die betrekking hebben op het hoofdmodel worden pas na deze handeling opgeslagen.

### Blockschema's voor Contentblokken in CMS met Filament

In het CMS met Filament worden contentblokken gedefinieerd via PHP-classes. deze kunnen in verschillende templates hergebruikt worden. In de blokken wordt ook gespecificeerd hoe de informatie uit het blok moet worden opgehaald wanneer deze wordt opgevraagd.

**Block-class in CMS met Filament**

```php 
<?php

namespace App\Cms\Blocks\Common;

use App\Cms\Blocks\Interfaces\BlockContract;
use Filament\Forms\Components\Builder\Block;
use Filament\Forms\Components\RichEditor;
use Filament\Forms\Components\TextInput;

class Paragraph implements HasBlockSchema
{
    public static function getBlock(): Block
    {
        return Block::make('common\paragraph')
            ->label('Paragraph')
            ->schema([
                TextInput::make('title')
                    ->label('Title'),
                RichEditor::make('text')
                    ->label('Text')
                    ->toolbarButtons([
                        'h1',
                        'h2',
                        'bold',
                        'italic',
                        'underline',
                        'orderedList',
                        'bulletList',
                    ]),
            ]);
    }

    public function resolve(array $blockData): array
    {
        return [
            'title' => $blockData['title'],
            'text' => $blockData['text'],
        ];
    }
}
```

### Interfaces

Ook voor de blocks is er een interface voorbereid, namelijk `HasBlockSchema`, die voorschrijft welke functionaliteiten een block moet bieden. Dit betreft het beschikbaar stellen van de formuliervelden die bij het block horen en een methode om de content van het block, die als JSON in het CMS wordt opgeslagen, op te halen.

**HasBlockSchema interface in het CMS met Filament**

```php
<?php

namespace App\Cms\Blocks\Interfaces;

use Filament\Forms\Components\Builder\Block;

interface HasBlockSchema
{
    public static function getBlock(): Block;

    public function resolve(array $blockData): array;
}
```


## Statamic CMS met flat-file / eloquent-driver

In de flat-file en eloquent-driver configuratie van Statamic wordt het CMS anders opgezet dan bij traditionele CMS-systemen. Een belangrijk verschil is dat Statamic geen specifieke Eloquent-modellen per entiteit gebruikt, zoals bij veel andere Laravel-gebaseerde applicaties. In plaats daarvan maakt Statamic gebruik van `Collection`-modellen, die objecten zoals `Entry`-modellen beheren. Dit zorgt voor een structuur die geen complexe databasetabellen of acties vereist.

### Collections

In de huidige configuratie worden pagina's beheerd binnen een `Collection`. Deze collecties bepalen de globale instellingen voor de entries die tot de collectie behoren. Denk hierbij aan zaken zoals de manier waarop de overzichtspagina gesorteerd moet worden, of entries genest kunnen worden, en het definiëren van de route die aangeroepen moet worden om een specifieke entry uit de collectie op te halen. Je kunt een collectie vergelijken met een Eloquent-model dat communiceert met de databasetabel van een bepaalde entiteit. Dit kunnen naast pagina's ook andere entiteiten zijn, zoals blogposts of reviews.

**configuratie voor de `Pages`-collection**

```yaml
title: Pages  
revisions: false  
route: '/{slug}'  
sort_dir: asc  
date_behavior:  
  past: public  
  future: private  
structure:  
  root: true  
  max_depth: 1
```

### Blueprints

In de huidige configuratie worden de templates die gebruikt worden voor het beheren van de dynamische content op de pagina's gedefinieerd in een `Blueprint`. In het geval van pagina's waarvoor we meerdere templates gebruiken, kunnen we voor de `Pages`-collectie meerdere Blueprints aanmaken. Dit maakt het mogelijk om per template verschillende soorten velden te definiëren die aan een pagina meegegeven kunnen worden. Voor objecten waarvoor maar één soort beschikbaar is, maak je dus slechts één Blueprint aan.

Een Blueprint definieert niet alleen het schema voor de invoervelden, maar bepaalt ook de validatie-regels, standaardwaarden en de zichtbaarheid van velden in de gebruikersinterface.

**Blueprint voor het homepage-template**

```yaml
order: 1  
tabs:  
  main:  
    display: Main  
    sections:  
      -  
        display: 'General information'  
        fields:  
          -  
            handle: template  
            field:  
              dictionary: templates  
              default: homepage  
              type: dictionary  
              display: Template  
              listable: false  
              visibility: hidden  
          -  
            handle: title  
            field:  
              type: text  
              required: true  
              validate:  
                - required  
              display: 'Page title'  
              listable: true  
          -  
            handle: slug  
            field:  
              type: slug  
              localizable: true  
              validate: 'max:200'  
              display: 'Page uri'  
              show_regenerate: true  
      -  
        display: Content  
        fields:  
          -  
            handle: header_title  
            field:  
              type: text  
              display: 'Header title'  
              listable: false  
          -  
            handle: header_image  
            field:  
              container: default  
              type: assets  
              display: 'Header image'  
          -  
            handle: blocks  
            field:  
              type: replicator  
              display: Blocks  
              listable: false  
              sets:  
                cms_content:  
                  display: 'CMS content'  
                  sets:  
                    paragraph:  
                      display: Paragraph  
                      fields:  
                        -  
                          import: common.paragraph  
                    image:  
                      display: Image  
                      fields:  
                        -  
                          import: common.image  
                    map:  
                      display: Map  
                      fields:  
                        -  
                          import: common.map  
                    call_to_action:  
                      display: 'Call to Action'  
                      fields:  
                        -  
                          import: common.call_to_action  
title: Homepage
```

### Fieldsets

Wellicht is het je al opgevallen dat er in de home-blueprint een `replicator`-veld is gedefinieerd met de handle `blocks`. Hieronder kunnen voorgedefinieerde sets van invoervelden worden meegegeven, die op hun beurt weer als contentblokken gebruikt kunnen worden. Om te voorkomen dat deze voorgedefinieerde sets elke keer dat ze gebruikt worden opnieuw gedefinieerd moeten worden, is het mogelijk om ze vooraf te definiëren in `Fieldsets`. Binnen de `replicator` op de pagina geef je vervolgens aan welke `Fieldsets` je wilt gebruiken.

**Fieldset voor het Paragraph block**

```yaml
title: Paragraph  
fields:  
  -  
    handle: title  
    field:  
      type: text  
      display: Title  
  -  
    handle: text  
    field:  
      buttons:  
        - h1  
        - h2  
        - h3  
        - h4  
        - h5  
        - h6  
        - bold  
        - italic  
        - underline  
        - unorderedlist  
        - orderedlist  
        - removeformat  
        - quote  
        - anchor  
        - image  
        - table  
      save_html: true  
      remove_empty_nodes: trim  
      type: bard  
      display: Text  
      listable: false  
  - handle: namespace  
    field:  
      default: common  
      type: text  
      display: Namespace  
      sortable: false  
      visibility: hidden  
      replicator_preview: false  
      duplicate: false
```

### Entries

De daadwerkelijke pagina's en objecten worden beheerd als `Entries` binnen de gedefinieerde collecties. Een `Entry` is in feite de representatie van een individueel item binnen een collectie, zoals een pagina of blogpost. Elke `Entry` bevat specifieke gegevens voor de velden die zijn gedefinieerd in de bijbehorende `Blueprint`. `Entries` worden in de flat-file configuratie opgeslagen als markdown-bestanden in de `content`-directory van de repository. Wanneer je gebruik maakt van de Eloquent-driver (en aangeeft dat `Entries` in de database opgeslagen moeten worden), wordt een `Entry` in de `entries`-databasetabel opgeslagen.

 **Entry in de `Pages`-collection, gebaseerd op de `Homepage`-blueprint.** 
```markdown
---  
id: 78f550f5-b9a4-404a-86e5-fc684b3e3b77  
blueprint: home  
title: Home  
header_title: "La gloire est éphémère, mais l'oubli est éternel."  
blocks:  
  -    id: m4szm60u    title: 'Bonjour à tous,'    text: '<p>I am Napoleon Bonaparte, exiled here on the remote island of Saint Helena. Once, I commanded vast armies, reshaped nations, and navigated the turbulent waters of European politics. Today, however, I find myself in the serene isolation of this distant land, where the ocean whispers tales of glory and defeat.</p><p>As I pen my thoughts for you, dear readers, I invite you into my world—a realm of ambition, strategy, and, yes, introspection. Here, I shall share my reflections on leadership, the nature of power, and the lessons learned from both triumphs and trials.</p><p>Join me as I explore the intricate tapestry of history, the weight of legacy, and the fleeting nature of fame. Whether you seek inspiration, knowledge, or simply the musings of a man who once stood at the pinnacle of power, I welcome you to my journey.</p><p>À bientôt,<br>Napoleon</p>'    namespace: common    type: paragraph    enabled: true  -    id: m4ua63d6    title: 'A new conquest'    text: '<p>I invite you to join me on a new conquest, not of nations but of cinema and literature! Much as I once sought to reshape Europe, I now seek to navigate the vast empire of film, and I need you by my side. Do you dare follow me, loyal subjects, in this new adventure? Then visit my reviews page into the world of film and literature!</p>'    urlable_id: 8a254262-90b1-4eb7-918a-e7d60d81a683    button_text: 'Read more'    namespace: common    type: call_to_action    enabled: true  -    id: m4udna2o    namespace: common    type: map    enabled: true    google_maps:      formatted_address: '129 Rue de Grenelle, 75007 Paris, France'      coordinates:        lat: 48.8581983        lng: 2.3130361    title: 'I await your visit'    text: '<p>Visit me at Les Invalides, where I, Napoleon Bonaparte, rest. Stand before me, and discuss the ambition, triumphs, and sacrifices that shaped our history.</p>'updated_by: 1  
updated_at: 1735046888  
template: homepage  
---
```

### Synchronisatie van relaties

In de flat-file configuratie van Statamic is er geen automatische synchronisatie van bi-directionele relaties tussen objecten. Dit kan leiden tot inconsistenties binnen een project wanneer een bepaalde relatie aan de ene kant van het object wordt verwijderd, maar in het andere object nog steeds bestaat. Een mogelijke oplossing is om een Listener te maken voor modellen met bi-directionale relaties, die ervoor zorgt dat wanneer deze Entries de `saved` actie aanroepen, het gerelateerde object van de relatie ook wordt aangepast. Hierbij moet worden meegenomen dat de Listener geactiveerd wordt op het `Entry`-model. Dit betekent dus dat we bij elke `saved`-actie door alle mogelijke relaties van alle entiteiten moeten, moeten voorkomen dat we een infinite-loop veroorzaken, rekening houden met race conditions en wanneer een er in de Listener-actie nogmaals een `saved`-actie geactiveerd wordt terwijl we er ook voor zorgen dat alle acties zoals het assicioeren als loskoppelen goed uitgevoerd wordt. Daarnaast moeten wijzigingen in de blueprints - zoals de naam van een veld - ook worden doorgevoerd in de Listener. Kortom, veel bloating voor functionaliteit die wanneer je van Eloquent-modellen gebruikt maakt geen last hebt. 

Dit is het moment waarop de marketplace en de community rondom een tool of library zich van hun beste kant laten zien. Je bent immers niet de eerste developer die dit probleem tegenkomt; vaak zijn anderen je al voor geweest, en soms hebben zij een kant-en-klare oplossing beschikbaar gesteld. Dit geldt ook voor dit specifieke issue: in de marketplace vinden we de addon **Entry Relationships**.

Om deze addon, die bi-directionale relaties verwerkt, te configureren, hoef je enkel de package te importeren en binnen een serviceprovider een functie aan te roepen die de gerelateerde velden met elkaar verbindt.

Een kleine kanttekening bij dit soort addons is dat ze vaak onderhouden worden door een enkele developer. Wanneer er problemen optreden, kan het zijn dat deze niet direct binnen het package opgelost worden.

> [Entry Relationship addon in de Statamic Marketplate](https://statamic.com/addons/stillat/entry-relationships)

**Voorbeeld van de serviceProvider**
```php
<?php  
  
namespace App\Providers;  
  
use Illuminate\Support\ServiceProvider;  
use Stillat\Relationships\Support\Facades\Relate;  
  
class RelationshipServiceProvider extends ServiceProvider  
{  
    public function boot(): void  
    {  
        Relate::manyToMany(  
            'actors.movies',  
            'movies.actors',  
        );  
    }  
  
    public function register(): void  
    {}  
}
```