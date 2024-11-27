# CMS Prototypes Realisatie

Dit document geeft een overzicht van de basisfunctionaliteiten van een Content Management Systeem (CMS) en beschrijft hoe de prototypes kunnen worden opgebouwd.
## Contentbeheer in AllesOnline CMS

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
Binnen de `ContentManager` worden `Page`-objecten dynamisch gegenereerd via XML-templates die specificeren welke velden (zoals tekst, afbeeldingen en blokken) kunnen worden toegevoegd. Dit stelt ontwikkelaars in staat om template-schema's te definiëren voor CMS-gebruikers.

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

## Contentbeheer in CMS met Filament

In Filament kan een soortgelijke functionaliteit worden gerealiseerd door gebruik te maken van Resources en een Eloquent-model gebaseerd op het `Page`-model van AllesOnline CMS. Dit model bevat de gegevens en relaties van pagina’s en hun hiërarchie.

*Een simpel UML-diagram met het onderstaande concept voor content management kan je [hier bekijken](../Bijlagen/UmlEntiteitenDiagramContentManagementFilament.md)*

### Eloquent Model: Page
Het `Page`-model slaat paginagegevens in de database op en beheert relaties tussen pagina's.

**Page-model in CMS met Filament**

```php
<?php

namespace App\Models;

use App\Models\Interfaces\HasContent;
use App\Models\Interfaces\HasUrl;
use App\Models\Traits\ProvideContent;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\BelongsToMany;

class Page extends Model implements HasContent, HasUrl
{
    use ProvideContent;

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
Zoals eerder omschreven bevat de `ProvidesContentTrait` de logica voor de polymorfe relatie tussen het model dat `HasContent` implementeert en het Content-model.

```php
<?php

namespace App\Models\Traits;

use App\Models\Content;
use Illuminate\Database\Eloquent\Relations\MorphMany;

trait ProvidesContent
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
Met dit `Page`-model kan een Filament `PageResource` een formulier genereren voor het beheren van pagina’s. Een selectievak stelt de gebruiker in staat een template te kiezen, waarna het formulier dynamisch wordt aangepast op basis van de geselecteerde template.

**Schema voor Content Management in CMS met Filament**

```php
public static function form(Form $form): Form
    {
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

                    Forms\Components\Select::make('template')
                        ->options(TemplateFactory::getTemplateNames())
                        ->required()
                        ->live(),

                    Forms\Components\Section::make()
                        ->schema(function (Get $get) {
                            if ($get('template')) {
                                return 
									TemplateFactory::loadTemplateSchema(
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
De `TemplateFactory`-class ondersteunt de selectie en dynamische weergave van velden op basis van het door de gebruiker gekozen template.

**TemplateFactory in CMS met Filament**

```php
<?php

namespace App\Cms;

use App\Cms\Templates\Enums\Templates;
use App\Cms\Templates\Interfaces\HasTemplateSchema;

class TemplateFactory
{
    public static function getTemplateNames(): array
    {
        return Templates::getFormattedNames();
    }

    public static function getTemplateFields(string $template): array
    {
        return self::extractFieldNames(self::loadTemplateSchema($template));
    }

    public static function loadTemplateSchema(string $template): array
    {
        if (! class_exists($template)) {
            abort(404);
        }

        $templateClass = new $template;
        return $templateClass instanceof HasTemplateSchema 
        ? $templateClass->getForm() 
        : [];
    }

    protected static function extractFieldNames(array $components): array
    {
        $fields = [];

        foreach ($components as $component) {
            if (method_exists($component, 'getName')) {
                $fields[] = $component->getName();
            }

            if (method_exists($component, 'getSchema')) {
                $fields = array_merge(
	                $fields, 
	                self::extractFieldNames($component->getSchema())
                );
            }
        }

        return $fields;
    }
}
```

### Class voor Templates in CMS met Filament
Template-velden worden beheerd in specifieke classes die een array met Filament FormField-componenten teruggeven aan de Resource.

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

class Homepage implements HasTemplateSchema
{
	public static function getName(): string
	{
		return 'Homepage';
	}

    public static function getForm(): array
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

Het bovenstaande template bevat, net zoals het AO CMS-template, een veld voor een headerafbeelding en een titel. Gebruikers kunnen ook blokken selecteren om toe te voegen.

### Interfaces
Alle templates implementeren de interface `HasTemplateSchema`. Deze interface vereist dat de classes die het implementeren een schema voor hun CMS content beschikbaar stelt. Door dit te implementeren, kunnen we garanderen dat een template - dat in de `TemplateFactory` wordt geïnjecteerd - over de functionaliteit beschikt om dit schema op te halen.

Om het ophalen van het _FormSchema_ gemakkelijker te maken voor developers, is ervoor gekozen om de functie `static` te maken. Dit kan het DIP-principe van SOLID schenden en leiden tot een _tight coupling_. Echter, in gevallen waarin dit gebeurt, is het vanzelfsprekend dat er een sterke afhankelijkheid bestaat tussen het schema en de functie die het schema opvraagt. Neem bijvoorbeeld het `Review`-model, dat voor zijn CMS-content gebruik zal maken van de `ReviewTemplate`-class.

**HasTemplateSchema interface in het CMS met Filament**

```php
<?php

namespace App\Cms\Templates\Interfaces;

interface HasTemplateSchema
{
    public static function getForm(): array;
}
```

### Enums
Alle templates die gebruikers kunnen selecteren binnen het `Page`-model worden centraal beheerd in `Templates`. Deze `enum` bevat verschillende cases, waarbij elke case de classname van een specifieke template vertegenwoordigt. Naast het eenvoudig bijhouden van deze classnames biedt de enum aanvullende functionaliteit om de cases op een voor de gebruiker leesbare manier te presenteren.

```php
<?php

namespace App\Cms\Templates\Enums;

use App\Cms\Templates\BlogTemplate;
use App\Cms\Templates\HomeTemplate;
use App\Cms\Templates\Interfaces\HasTemplateSchema;
use App\Cms\Templates\ReviewTemplate;

enum Templates: string
{
    case HOMEPAGE = HomeTemplate::class;
    case BLOG = BlogTemplate::class;
    case REVIEW = ReviewTemplate::class;

    public static function getFormattedNames(): array
    {
        $cases = self::cases();
        $formattedNames = [];

        foreach ($cases as $case) {
            $templateClass = new $case->value;
            
            if ($templateClass instanceof HasTemplateSchema) {
                $formattedNames[$templateClass::class] 
		            = $templateClass->getName();
            }
        }

        return $formattedNames;
    }
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
        if (! empty($data['template'])) {
            $templateFields = 
	            TemplateFactory::getTemplateFields($data['template']);
	            
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
        $templateFields = TemplateFactory::getTemplateFields($data['template']);
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

    public static function resolve(array $blockData): array
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

    public static function resolve(array $blockData): array;
}
```