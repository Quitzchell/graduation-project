# CMS Prototype Realisaties

Dit document biedt een aantal suggesties voor het opzetten van de basisfunctionaliteiten van een Content Management Systeem (CMS), waarbij gebruik wordt gemaakt van libraries van derde partijen.

## Contentbeheer

### AllesOnline CMS

In het AllesOnline CMS worden pagina’s, hun volgorde en de beschikbare content beheerd via de `ContentManagerController`. Deze functionaliteit wordt doorgaans zonder aanpassingen overgenomen uit het AllesOnline CMS-pakket. Vanuit deze controller heeft het systeem directe toegang tot de `ManagedContent`, `Page` en `CMSContent`-modellen.

```php
<?php

namespace App\Http\Controllers;

use ContentManager;

class ContentManagerController extends ContentManager
{
    //
}
```

Binnen de `ContentManager` worden `Page`-objecten beheerd, die gebruikmaken van XML-templates. Deze templates definiëren welke extra velden, naast de standaard velden op een pagina, als `CMSContent` kunnen worden toegevoegd. Een voorbeeld van een XML-template ziet er als volgt uit:

```xml
<template name="Review page">
    <field type="media-item" name="header_image" title="Header: Image"/>
    <field type="text" name="header_title" title="Header: Title"/>
    <field type="blocks" name="blocks" title="Content blocks" 
           blocks="common/*" exclude_blocks="home/*"/>
</template>
```

Zoals zichtbaar in het template, wordt hier bepaald welke velden beschikbaar zijn en welk type deze hebben. Naast individuele velden kunnen ook blokken worden toegevoegd. Deze blokken bevatten vaak een verzameling velden en vertegenwoordigen onderdelen van de website die herbruikbaar zijn, zoals meerdere keren op dezelfde pagina of op verschillende pagina’s.

Een voorbeeld van een blok kan er als volgt uitzien:

```xml
<template name="Paragraph">
    <field type="text" name="title" title="Title"/>
    <field type="html" name="text" title="Text" height="40"
           toolbar="formatselect | bold italic underline | charmap | link"/>
</template>
```

## Filament

In Filament kunnen we iets vergelijkbaars realiseren met behulp van Resources en een Eloquent-model dat gebaseerd is op het Page-model van het AllesOnline CMS.

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

    /* Relations */
    public function parent(): BelongsTo 
    {
        return $this->belongsTo(Page::class, 'parent_id');
    }

    public function children(): HasMany
    {
        return $this->hasMany(Page::class, 'parent_id');
    }

    /* Urlable */
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

Met dit model kan een Filament `PageResource` een formulier genereren voor het aanmaken en bewerken van pagina's. Binnen dit formulier is een selectieveld toegevoegd dat, afhankelijk van het gekozen template, de bijbehorende velden via de TemplateFactory dynamisch aan het formulier toevoegt en aan de gebruiker in het CMS toont.

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