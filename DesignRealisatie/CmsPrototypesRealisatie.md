# CMS Prototype Realisaties

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
<template name="Review page">
    <field type="media-item" name="header_image" title="Header: Image"/>
    <field type="text" name="header_title" title="Header: Title"/>
    <field type="blocks" name="blocks" title="Content blocks" 
           blocks="common/*" exclude_blocks="home/*"/>
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

---

## Contentbeheer in CMS met Filament

In Filament kan een soortgelijke functionaliteit worden gerealiseerd door gebruik te maken van Resources en een Eloquent-model gebaseerd op het `Page`-model van AllesOnline CMS. Dit model bevat de gegevens en relaties van pagina’s en hun hiërarchie.

### Eloquent Model: Page
Het `Page`-model slaat paginagegevens in de database op en beheert relaties tussen pagina's.

**Page-model in CMS met Filament**

```php
<?php

namespace App\Models;

use App\Models\Interface\UrlableContract;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\BelongsTo;
use Illuminate\Database\Eloquent\Relations\HasMany;

class Page extends Model implements UrlableContract
{
    protected $table = 'pages';

    protected $fillable = [
        'name',
        'template',
        'content',
        'parent_id',
    ];

    protected $casts = [
        'content' => 'array',
    ];

    // Relaties
    public function parent(): BelongsTo 
    {
        return $this->belongsTo(Page::class, 'parent_id');
    }

    public function children(): HasMany
    {
        return $this->hasMany(Page::class, 'parent_id');
    }

    // URL-functies
    public function uri($lang = null) { 
        $uri = strtolower($this->name); 
        if ($this->parent) { 
            $uri = $this->parent->uri($lang) . '/' . $uri; 
        } 
        return $uri; 
    } 
    
    public function url($lang = null) { 
        return config('app.url') . '/' . $this->uri($lang);
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
                    ->required(),

                Forms\Components\Select::make('template')
                    ->options(TemplateFactory::getTemplateNames())
                    ->required()
                    ->live(),

                Forms\Components\Section::make()->schema(function (Get $get) {
                    if ($get('template')) {
                        return TemplateFactory::loadTemplateSchema($get('template'));
                    }

                    return [];
                }),
            ]),
        ])
        ->model(Page::class);
}
```

### TemplateFactory voor Dynamische Template-selectie
De `TemplateFactory`-klasse ondersteunt de selectie en dynamische weergave van velden op basis van het door de gebruiker gekozen template.

**TemplateFactory in CMS met Filament**

```php
<?php

namespace App\Cms;

use App\Cms\Templates\Homepage;

class TemplateFactory
{
    public static function getTemplateNames(): array
    {
        return [
            Homepage::class => class_basename(Homepage::class),
        ];
    }

    public static function getTemplateFields(string $template): array
    {
        return self::extractFieldNames(self::loadTemplateSchema($template));
    }

    public static function loadTemplateSchema(string $template): array
    {
        return class_exists($template) ? (new $template)->getForm() : [];
    }

    protected static function extractFieldNames(array $components): array
    {
        $fields = [];

        foreach ($components as $component) {
            if (method_exists($component, 'getName')) {
                $fields[] = $component->getName();
            }

            if (method_exists($component, 'getSchema')) {
                $fields = array_merge($fields, self::extractFieldNames($component->getSchema()));
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

use App\Cms\Blocks\About;
use App\Cms\Blocks\CallToAction;
use App\Cms\Blocks\Image;
use App\Cms\Blocks\Map;
use App\Cms\Blocks\Paragraph;
use App\Cms\Templates\Interfaces\TemplateContract;
use Filament\Forms\Components\Builder;
use Filament\Forms\Components\FileUpload;
use Filament\Forms\Components\TextInput;

class Homepage implements TemplateContract
{
    public static function getForm(): array
    {
        return [
            TextInput::make('header_title')
                ->label('Header title'),

            FileUpload::make('header_image')
                ->label('Header Image')
                ->image()
                ->required(),

            Builder::make('content')->schema([
                About::getBlock(),
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

### Blok-schema's voor Contentblokken in CMS met Filament
In het CMS met Filament worden contentblokken gedefinieerd via PHP-classes die in verschillende templates hergebruikt kunnen worden.

**Blok-class in CMS met Filament**

```php 
<?php

namespace App\Cms\Blocks;

use App\Cms\Blocks\Interfaces\BlockContract;
use Filament\Forms\Components\Builder\Block;
use Filament\Forms\Components\RichEditor;
use Filament\Forms\Components\TextInput;

class Paragraph implements BlockContract
{
    public static function getBlock(): Block
    {
        return Block::make('paragraph')
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
}
```

Dit schema biedt, net als in het AO CMS, een titel- en tekstveld met RichText-opties voor tekstopmaak.