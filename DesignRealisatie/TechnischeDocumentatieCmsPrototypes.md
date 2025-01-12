# **Technische Documentatie CMS Prototypes**

Dit document biedt een technisch overzicht van de werking en structuur van de onderzochte CMS'en.

# AllesOnline CMS

## ContentManagerController

In het AllesOnline CMS worden pagina's en content beheerd via de `ContentManagerController`, die meestal ongewijzigd wordt geïmporteerd vanuit de AllesOnline CMS-packages. Deze controller beheert pagina's binnen het CMS en biedt functionaliteiten voor CRUD-operaties en gebruikersrechtenverificatie.

**Naast deze basisfunctionaliteiten biedt de controller:**
* **Content hiërarchie en volgorde**: Beheer van de hiërarchie en volgorde van pagina-items in de database via methoden zoals updateManagedContent.
* **Autorisatie**: Controle of gebruikers de juiste rechten hebben om acties uit te voeren, met methoden zoals can en methodPermission, ondersteund door middleware en configuratiebestanden.
* **Pagina-vergrendeling**: Mogelijkheid om pagina's te vergrendelen en ontgrendelen om ongewenste wijzigingen te voorkomen.
Replicatie van pagina's: Het dupliceren van pagina's en hun gekoppelde content via de getCopy-methode.
* **Templatebeheer**: Beheer van beschikbare pagina-templates, inclusief filtering, sortering en integratie met specifieke resolvers.
* **Menu-integratie**: Koppeling van pagina's aan menu's en het beheer van de menustructuur.

## Dynamische Weergavegeneratie en Templatebeheer

Binnen de `ContentManagerController` worden de weergaven voor verschillende pagina's, zoals info-, create-, edit- en overview-pagina's, dynamisch gegenereerd via XML-templates en de `BaseView`-class. De `BaseView` stelt de juiste namespace in en zorgt ervoor dat de correcte weergave wordt geladen op basis van de context, zoals templates, menu-instellingen en andere data. Tegelijkertijd bepaalt de `PageTemplateResolver` welke templates beschikbaar zijn en laadt deze.

**XML-schema voor Template in AO CMS**

```xml
<template name="Home page">
    <field type="text" name="header_title" title="Header title"/>
    <field type="media-item" name="header_image" title="Header image"/>
    <field type="blocks" name="blocks" title="Content blokken"
		blocks="common/*"/>
</template>
```

**Dynamische weergavegeneratie en het gebruik van BaseView**
```php
public function getIndex($modelInstance = null)
{
    $data = [
        'ctrl' => $this,
    ];

    return BaseView::make('index', $data);
}

public function getAdd()
{
    $templates = $this->templates();
    $menuRoots = \ManagedContent::getRoots();

    $resolver = PageTemplateResolver::getInstance();

    return BaseView::make('add')
        ->with('templates', $templates)
        ->with('resolver', $resolver)
        ->with('menuRoots', $menuRoots);
}

public function getEdit($id)
{
    if (!isset($id))
        return redirect(Config::get('component-skins.prefix') . 'content');

    $page = Page::find($id);

    if (!isset($page))
        return redirect(Config::get('component-skins.prefix') . 'content');

    return $this->editWithPage($page);
}

protected function editWithPage($page)
{
    $templates = $this->templates();
    $menuRoots = \ManagedContent::getRoots();
    $manageable = true;

    if ($page->permissions('edit') === 'content') {
        $manageable = false;
    }

    return BaseView::make('edit')->with('menuRoots', $menuRoots)->with('page', $page)->with('templates', $templates)->with('manageable', $manageable);
}

```

**Inladen van templates via de PageTemplateResolver**
```php
protected function templates()
{
    return collect(PageTemplateResolver::getInstance()->getTemplates())
        ->keyBy(function (Template\Template $template) {
            return $template->getKey();
        })
        ->map(function (Template\Template $template) {
            if ($template->getAttribute('hidden') && !\Request::input('show_hidden'))
                return false;

            return $template->getAttribute('name');
        })
        ->filter()->sort()->all();
}
```

## CMS-content

De `CmsContent`-class beheert inhoud binnen een CMS-systeem en maakt dynamisch renderen mogelijk op basis van XML-templates. 

**CMS-content instellen vanuit een array**
``` php
public function setContentFromArray($content, &$i = 1) {
    $items = [];
    foreach ($content as $key => $blocks) {
        $template = $this->getTemplate();
        if ($template) {
            $f = $template->getField($key);
            $ls = $f->getLanguages();
            if (count($ls) > 1) {
                if (!is_array($blocks)) {
                    $blocks = [default_language() => $blocks];
                }
                foreach ($ls as $l) {
                    if (!isset($blocks[$l])) {
                        continue;
                    }
                    $items[] = $child = new CmsContent();
                    $child->fill(['value' => $blocks[$l], 'tag' => $key, 'id' => $i++]);
                    $child->language = $l;
                }
                continue;
            }
        }
        if (!is_array($blocks)) {
            $items[] = $child = new CmsContent();
            $child->fill(['value' => $blocks, 'tag' => $key, 'id' => $i++]);
            continue;
        }
        foreach ($blocks as $block) {
            $items[] = $child = new CmsContent();
            $child->fill(['value' => $block['value'], 'tag' => $key, 'id' => $i++]);
            if (array_key_exists('content', $block)) {
                $child->setContentFromArray($block['content']);
            }
        }
    }
    $this->setContent($items);
}
```

**Content ophalen en verwerken binnen het CMS**
``` php
public function content($tag = null, $default = '', $forceArray = false) {
    if ($tag === null) {
        return $this;
    }
    $field = null;
    if ($this->getTemplate()) {
        $field = $this->getTemplate()->getField($tag);
    }
    $v = null;
    if ($field && count($field->getLanguages()) > 1) {
        $v = $this->contentLanguageFilter($tag, app('language'));
        if (in_array($v, [null, '']) && $fb = config('content.fallback_language')) {
            $v = $this->contentLanguageFilter($tag, $fb);
        }
    }
    if (!$v && $this->objectCount($tag) && config('content.return_fallback_language_if_empty', true)) {
        $v = array_first_value($this->content[$tag])->value;
    }
    if (in_array($v, [null, ''])) {
        $v = $default;
    }
    if (!config('cms.use_process_value')) {
        return $v;
    }
    if (!$field) {
        return $v;
    }
    $m = $this->getTemplate()->getField($tag)->getModuleClass();
    $m = new $m();
    if ($m instanceof ProcessValue) {
        $v = $m->processValue($v);
    }
    return $v;
}

```

**Recursieve doorlopen van contentstructuren**
``` php
public function traverse(callable $fn, $recursive = true) {
    foreach ($this->content as $objects) {
        foreach ($objects as $object) {
            $fn($object);
            if ($recursive) {
                $object->traverse($fn);
            }
        }
    }
}
```

**Dynamisch renderen van content met behulp van templates**
``` php
public function render($data = []) {
    $provider = $this->getTemplate()->getProvider();
    if ($provider instanceof TemplateRenderer) {
        $data['__content'] = $this;
        $viewData = [];
        if ($dataSources = $this->getTemplate()->getDataSources()) {
            foreach ($dataSources as $name => $source) {
                $instance = new $source($this, $data);
                $viewData[$name] = $instance;
            }
        }
        return $this->wrapFrontendCmsMarker($provider->render($this, $data, $viewData));
    }
    return null;
}

```

## Contentblokken

Binnen de templates kan naar andere XML-bestanden worden verwezen die contentblokken genereren, welke door de `CmsContent`-class worden verwerkt.

**XML-schema voor contentblokken in AO CMS**

```xml
<template name="Paragraph">
    <field type="text" name="title" title="Title"/>
    <field type="html" name="text" title="Text" height="40"
		toolbar="formatselect | bold italic underline | charmap | link"/>
</template>
```

**Dynamisch renderen van contentblokken**
```php
public function blocks(string $tag = 'blocks', array $data = [])
    {
        $ret = '';

        $objects = array_values($this->objects($tag));

        foreach ($objects as $index => $block) {
            if (config('content.blocks_state_option') && !$block->active) {
                continue;
            }

            /** @var CmsContentInterface $block */
            $ret .= $block->render(array_merge($data, [
                '__block_index' => $index,
                '__block_count' => count($objects)
            ]));
        }

        return $ret;
    }
```

# CMS met Filament

In **Filament** kan een CMS, vergelijkbaar met het AllesOnline CMS, worden opgebouwd met behulp van `Resources` en een `Eloquent`-model, zoals het `Page`-model van AllesOnline. Dit model beheert de gegevens en relaties van pagina's en hun hiërarchie.

> [UML class-diagram met het concept content management in Filament](../Bijlagen/UmlEntiteitenDiagramContentManagementFilament.md)

## Eloquent Model: Page

De `Page`-class is een **Eloquent**-model in Laravel dat pagina-informatie beheert, relaties legt met menu's en URL's genereert op basis van de paginanaam.

**Page-model**

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

    /* Relations */
    public function menus(): BelongsToMany
    {
        return $this->belongsToMany(Menu::class);
    }

    /* HasUrl */
    public function uri($lang = null): string
    {
        return strtolower($this->name);
    }

    public function url($lang = null): string
    {
        return url($this->uri($lang));
    }
}
```

## Interfaces

Het `Page`-model implementeert interfaces die richtlijnen definiëren voor specifieke functionaliteiten, zoals het beheren van content (`HasContent`) en het genereren van URL's (`HasUrl`), waardoor het model consistent en uitbreidbaar blijft.

### HasContent

De `HasContent`-interface vereist dat het model een relatie met het Content-model definieert, waarin CMS-content wordt opgeslagen. In het Filament CMS wordt deze functionaliteit aangevuld met de `ProvidesContentTrait`.

**HasContent interface**

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

### HasUrl

De `HasUrl`-interface vereist dat het model functionaliteit implementeert om een `uri` en een volledige `url` te genereren voor de bijbehorende objecten.

**HasUrl interface**

```php
<?php

namespace App\Models\Interfaces;

interface HasUrl
{
    public function uri($lang = null): string;

    public function url($lang = null): string;
}
```

## Traits

### ProvidesContentTrait

De `ProvidesContentTrait` bevat de logica voor de polymorfe relatie tussen modellen die `HasContent` implementeren en het `Content`-model. Door deze **Trait** te gebruiken, kan de logica eenvoudig worden hergebruikt. Voor aanvullende functionaliteiten kan een nieuwe trait worden aangemaakt of kunnen de methoden uit de `HasContent`-interface rechtstreeks in de class worden geïmplementeerd.

**ProvidesContentTrait**

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

## PageResource en dynamische templates

De `Resource`-classes in Filament dienen als de brug tussen Eloquent-modellen en de adminpanelen binnen het CMS. In de `Resource` die is gekoppeld aan het `Page`-model, kan in de `form()`-methode een `Select`-component worden toegevoegd. Hiermee kunnen gebruikers vooraf gedefinieerde **templates** voor een pagina selecteren.

**Schema voor Content Management**

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

                    Forms\Components\Select::make('template')
                        ->options($templateFactory->getTemplateNames())
                        ->required()
                        ->live(),

                    Forms\Components\Section::make()
                        ->schema(function (Get $get) use ($templateFactory) {
                            if ($get('template')) {
                                return $templateFactory->loadTemplateSchema($get('template'));
                            }

                            return [];
                        }),
                ]),
            ]);
    }
```

## TemplateFactory

De `TemplateFactory`-class ondersteunt het dynamisch ophalen van templates en zijn velden op basis van het gekozen template. Het valideert templates die de `HasTemplateSchema`-interface implementeren en biedt methoden voor het ophalen van veldnamen en een lijst van beschikbare templates.

**TemplateFactory**

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

## Class voor Templates

De `Template`-class worden gebruikt om specifieke structuur voor de template van een pagina te definiëren. Deze wordt 
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

In de flat-file configuratie van **Statamic** is er geen automatische synchronisatie van bi-directionele relaties, wat kan leiden tot inconsistenties wanneer een relatie aan de ene kant van het object wordt verwijderd, maar aan de andere kant blijft bestaan.

Een oplossing is het maken van een **Listener** voor modellen met bi-directionele relaties, die bij de `saved`-actie ook het gerelateerde object aanpast. Dit vereist aandacht voor infinite loops, race conditions en onbedoelde meerdere `saved`-acties. Wijzigingen in de blueprints, zoals veldnamen, moeten ook in de Listener worden verwerkt, wat de complexiteit verhoogt vergeleken met **Eloquent**-modellen.

In de marketplace is er een oplossing in de vorm van de addon **Entry Relationships**, die bi-directionele relaties beheert. Door de package te importeren en een functie in een serviceprovider aan te roepen, worden de velden met elkaar verbonden. Dit neemt nog niet weg dat de naam van de velden zowel in de blueprint als in de RelationServiceProvider aangepast moet worden.

Een ander nadeel is dat dergelijke addons ontwikkeld worden door derde partijen die niet altijd dezelfde urgentie voelen om hun packages goed te onderhouden.

> [Entry Relationship addon in de Statamic Marketplate](https://statamic.com/addons/stillat/entry-relationships)

**Voorbeeld van de RelationShipServiceProvider**

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

## Statamic CMS met Runway

In de configuratie waarbij gebruik wordt gemaakt van de Runway-addon verandert de architectuur vrij drastisch. In tegenstelling tot `Collections` maken we nu gebruik van `Eloquent`-modellen en specifieke databasetabellen voor alle entiteiten binnen het systeem. Hierdoor verliezen we helaas de standaard `navigation`- en `navigation-tree`-functionaliteiten van Statamic, inclusief de drag-and-drop functionaliteit om een menustructuur te definiëren. Om dit op te vangen is er in deze configuratie gekozen om een `menu manager` te gebruiken, zoals ook in het **Filament** CMS het geval is.
## Eloquent models

In principe zijn er weinig verschillen zichtbaar tussen het `Eloquent`-model voor **Statamic** die van **Filament**. Wat opvalt is een andere Trait: `HasRunwayResource`. Deze maak het mogelijk om onze gegevens in `Statamic` weer te geven.

Daarnaast zien we dat de `template`-attribute naar een `array` wordt gecast. Dit hint naar de eerste crux binnen deze configuratie, want alle gegevens die via een `replicator` worden meegegeven – dus de dynamische content voor een website – kunnen niet worden opgeslagen in een aparte tabel. In tegenstelling tot het AllesOnline CMS, wordt alle content onder het bijbehorende model als JSON opgeslagen.

**Eloquent model in Statamic met Runway-addon**

```php
<?php  
  
namespace App\Models;  
  
use App\Models\Interfaces\HasUrl;  
use Illuminate\Database\Eloquent\Model;  
use StatamicRadPack\Runway\Traits\HasRunwayResource;  
  
class Page extends Model implements hasUrl  
{  
    use HasRunwayResource;  
  
    protected $table = 'pages';  
  
    protected $casts = [  
        'template' => 'array',  
    ];  
  
    protected $fillable = [  
        'name',  
        'uri',  
        'template',  
        'blocks',  
    ];  
  
    /* HasUrl */  
    public function uri($lang = null): string  
    {  
        return strtolower($this->name);  
    }  
  
    public function url($lang = null): string  
    {  
        return url($this->uri($lang));  
    }  
}
```


## Runway Resources

Als we gebruik maken van de Runway configuratie is het niet meer mogelijk om gebruik te maken van `blueprints`. Dit omdat Runway gebruikt maakt van Runway Resources die eigenlijk op het zelfde niveau als `blueprints` zitten. Dit zijn dus dus de bestanden waarin de CMS schemas voor de entiteiten gedefineerd worden. 

**Runway resource voor het schema van het page-model**

```yaml 
tabs:  
  main:  
    display: Main  
    sections:  
      -  
        fields:  
          -  
            handle: name  
            field:  
              type: text  
              display: Name  
          -  
            handle: slug  
            field:  
              type: slug  
              localizable: true  
              validate: 'max:200'  
              display: 'Page uri'  
              show_regenerate: true  
              listable: false  
              sortable: false  
              replicator_preview: false  
              duplicate: false  
              from: name  
          -  
            handle: template  
            field:  
              previews: false  
              max_sets: 1  
              type: replicator  
              display: Template  
              listable: false  
              sortable: false  
              replicator_preview: false  
              duplicate: false  
              sets:  
                new_set_group:  
                  display: Template  
                  sets:  
                    homepage:  
                      display: Homepage  
                      fields:  
                        -  
                          import: template.home  
                    blog:  
                      display: Blog  
                      fields:  
                        -  
                          import: template.blog  
                    review:  
                      display: Review  
                      fields:  
                        -  
                          import: template.review  
title: Page
```
## Formfields voor templates

Omdat we met Runway de `collections` en daarmee een laag abstractie verliezen, is het niet meer mogelijk om de verschillende templates voor een page als blueprint op te geven. Daarom is het nodig om de templates een laag dieper te defineren, namelijk als `FieldSet`. In het voorbeeld hierboven zie je dat gebruikers een template via een `replicator` kunnen selecteren, in de `replicator` zelf zitten weer de velden die tot de template toe behoren. 

**FieldSet voor Homepage template**

```yaml 
title: Homepage  
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
      max_files: 1  
      min_files: 1  
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
```

## Formfields voor contentblokken

Binnen de `fieldSets` voor de templates is het mogelijk om weer verder te verwijzen naar een `replicator` met fieldSets voor content blokken. Deze blijven in principe onveranderd

**Nogmaals FieldSet voor het Paragraph block**
```php
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