# **Technische Documentatie CMS Prototypes**

Dit document biedt een technisch overzicht van de werking en structuur van de onderzochte CMS'en.

# AllesOnline CMS

## ContentManagerController

In het AllesOnline CMS worden pagina's en content beheerd via de `ContentManagerController`, deze wordt meestal ongewijzigd geïmporteerd vanuit de AllesOnline CMS-packages. Deze controller beheert pagina's binnen het CMS en biedt functionaliteiten voor CRUD-operaties en gebruikersrechtenverificatie.

**Naast deze basisfunctionaliteiten biedt de controller:**
* **Contenthiërarchie en volgorde**: Beheer van de hiërarchie en volgorde van pagina-items in de database via methoden zoals updateManagedContent.
* **Autorisatie**: Controle of gebruikers de juiste rechten hebben om acties uit te voeren, met methoden zoals can en methodPermission, ondersteund door middleware en configuratiebestanden.
* **Paginavergrendeling**: Mogelijkheid om pagina's te vergrendelen en ontgrendelen om ongewenste wijzigingen te voorkomen.
Replicatie van pagina's: Het dupliceren van pagina's en hun gekoppelde content via de getCopy-methode.
* **Templatebeheer**: Beheer van beschikbare pagina-templates, inclusief filtering, sortering en integratie met specifieke resolvers.
* **Menu-integratie**: Koppeling van pagina's aan menu's en het beheer van de menustructuur.

Binnen de `ContentManagerController` worden de weergaven voor verschillende pagina's, zoals info-, create-, edit- en overview-pagina's, dynamisch gegenereerd via XML-templates en de `BaseView`-class. De `BaseView` stelt de juiste namespace in en zorgt ervoor dat de correcte weergave wordt geladen op basis van de context, zoals templates, menu-instellingen en andere data. Tegelijkertijd bepaalt de `PageTemplateResolver` welke templates beschikbaar zijn en laadt deze in.

```
XML-schema voor Template in AO CMS
```
```xml
<template name="Home page">
    <field type="text" name="header_title" title="Header title"/>
    <field type="media-item" name="header_image" title="Header image"/>
    <field type="blocks" name="blocks" title="Content blokken"
		blocks="common/*"/>
</template>
```
---

```
Dynamische weergavegeneratie en het gebruik van BaseView
```
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
---

```
Inladen van templates via de PageTemplateResolver
```
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
---

## CMS-content

De `CmsContent`-class beheert inhoud binnen een CMS-systeem en maakt dynamisch renderen mogelijk op basis van XML-templates. 


```
CMS-content instellen vanuit een array
```
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
---

```
Content ophalen en verwerken binnen het CMS
```
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
---

```
Recursieve doorlopen van contentstructuren
```
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
---

```
Dynamisch renderen van content met behulp van templates
```
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
---

## Contentblokken

Binnen de templates kan naar andere XML-bestanden worden verwezen die contentblokken genereren, welke door de `CmsContent`-class worden verwerkt.

```
XML-schema voor contentblokken in AO CMS
```
```xml
<template name="Paragraph">
    <field type="text" name="title" title="Title"/>
    <field type="html" name="text" title="Text" height="40"
		toolbar="formatselect | bold italic underline | charmap | link"/>
</template>
```
---

```
Dynamisch renderen van contentblokken
```
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
---

# CMS met Filament

In **Filament** kan een CMS, vergelijkbaar met het AllesOnline CMS, worden opgebouwd met behulp van `Resources` en een `Eloquent`-model, zoals het `Page`-model van AllesOnline. Dit model beheert de gegevens en relaties van pagina's en hun hiërarchie.

> [UML class-diagram met het concept content management in Filament](../Bijlagen/UmlEntiteitenDiagramContentManagementFilament.md)

## Eloquent Model: Page

De `Page`-class is een **Eloquent**-model in Laravel dat pagina-informatie beheert, relaties legt met menu's en URL's genereert op basis van de paginanaam.

```
Page-model
```
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
---

## Interfaces

Het `Page`-model implementeert interfaces die richtlijnen definiëren voor specifieke functionaliteiten, zoals het beheren van content (`HasContent`) en het genereren van URL's (`HasUrl`), waardoor het model consistent en uitbreidbaar blijft.

### HasContent

De `HasContent`-interface vereist dat het model een relatie met het `Content`-model definieert, waarin CMS-content wordt opgeslagen. In het Filament CMS wordt deze functionaliteit aangevuld met de `ProvidesContentTrait`.

```
HasContent interface
```
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
---

### HasUrl

De `HasUrl`-interface vereist dat het model functionaliteiten implementeert om een `uri` en een volledige `url` te genereren voor de bijbehorende objecten.

```
HasUrl interface
```
```php
<?php

namespace App\Models\Interfaces;

interface HasUrl
{
    public function uri($lang = null): string;

    public function url($lang = null): string;
}
```
---

## Traits

### ProvidesContentTrait

De `ProvidesContentTrait` bevat de logica voor de polymorfe relatie tussen modellen die `HasContent` implementeren en het `Content`-model. Door deze **Trait** te gebruiken, kan de logica eenvoudig worden hergebruikt. Voor aanvullende functionaliteiten kan een nieuwe trait worden aangemaakt of kunnen de methoden uit de `HasContent`-interface rechtstreeks in de class worden geïmplementeerd.

```
ProvidesContentTrait
```
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
---

## PageResource en dynamische templates

De `Resource`-classes in Filament dienen als de brug tussen Eloquent-modellen en de adminpanelen binnen het CMS. In de `Resource` die is gekoppeld aan het `Page`-model, kan in de `form`-methode een `Select`-component worden toegevoegd. Hiermee kunnen gebruikers vooraf gedefinieerde **templates** voor een pagina selecteren.

```
Schema voor Content Management
```
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
---

## TemplateFactory

De `TemplateFactory`-class ondersteunt het dynamisch ophalen van templates en zijn velden op basis van het gekozen template. Het valideert templates die de `HasTemplateSchema`-interface implementeren en biedt methoden voor het ophalen van veldnamen en een lijst van beschikbare templates.

```
TemplateFactory
```
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
---

## Classes voor Templates

De `Template`-class definieert de specifieke structuur van een pagina-template. De `getName`-methode retourneert de naam van het template, terwijl de `getForm`-methode een array van componenten retourneert die eindgebruikers kunnen gebruiken om gegevens in te voeren.

```
Template-class in CMS met Filament
```
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
                ->imageEditor()
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
---

## Interfaces

Een `Template`-class implementeert de `HasTemplateSchema`-interface, die vereist dat classes zowel een schema voor CMS-content als een naam beschikbaar stelt. Het schema wordt ingeladen via de `TemplateFactory`, terwijl de naam wordt gebruikt in het select-veld waarin gebruikers een template kiezen in de `Page`-resource.

```
HasTemplateSchema interface
```
```php
<?php

namespace App\Cms\Templates\Interfaces;

interface HasTemplateSchema
{
    public function getName(): string;

    public function getForm(): array;
}
```
---

## Uitbreiding voor het create, edit en fill van CMS Content

Naast `Resource`-classes heeft Filament aparte classes voor het persisteren van de CMS gegevens. Om CMS-content op dezelfde manier als in het **AllesOnline CMS** te persisteren, moeten de functionaliteiten binnen deze classes worden aangepast. Filament biedt uitbreidbare, abstracte methoden om dit te realiseren. Omdat deze wijziging in alle `Resource`-classes doorgevoerd moet worden, zijn `Traits` ontwikkeld die de logica voor het persisteren en ophalen van CMS-content correct afhandelen.

### Create

Om een object met bijbehorende content aan te maken, wordt eerst het hoofdmodel (bijvoorbeeld een `Page`) opgeslagen. Vervolgens wordt de content via een relatie opgeslagen in het `Content`-model dat aan het hoofdmodel wordt gekoppeld. 

In het onderstaande voorbeeld wordt in de `mutateFormDataBeforeCreate`-methode de CMS-content uit het formulier gefilterd en tijdelijk opgeslagen tijdens het aanmaken van het hoofdmodel. Nadat het hoofdmodel succesvol is aangemaakt, wordt de content via een Eloquent-relatie in de `afterCreate`-methode opgeslagen.

```
MutateDataBeforeCreateTrait
```
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
---

### Edit

Wanneer een bestaand object wordt bewerkt, wordt de bijbehorende content in het formulier geladen. In de `mutateFormDataBeforeFill`-methode wordt de gekoppelde content opgehaald uit de `contents`-relatie en toegevoegd aan de formulierdata. Dit zorgt ervoor dat de opgeslagen gegevens in de velden worden weergegeven, zodat de gebruiker bestaande gegevens kan aanpassen zonder ze opnieuw in te voeren.

Wanneer CMS-content wordt toegevoegd of bijgewerkt in een bestaand object, wordt het opslaan eenvoudiger. In dit geval hoeft alleen de relevante content uit de formuliergegevens te worden gehaald en via een Eloquent-relatie te worden opgeslagen. Pas daarna wordt het hoofdmodel (bijvoorbeeld het object dat de `contents` bevat) opgeslagen.

```
MutateDataBeforeFillTrait
```
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
---

```
MutateBeforeSaveTrait
```
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
---

## Schema's voor Contentblokken

Voor het CMS met Filament kunnen `Blocks` gedefinieerd worden via PHP-classes, die eenvoudig in verschillende templates hergebruikt kunnen worden door ze te importeren. In de blokken wordt ook aangegeven hoe de informatie moet worden opgehaald voor verwerking.

```
Block-class in CMS met Filament
```
```php 
<?php

namespace App\Cms\Blocks\Common;

use App\Cms\Blocks\Interfaces\HasBlockSchema;
use Filament\Forms\Components\Builder\Block;
use Filament\Forms\Components\Hidden;
use Filament\Forms\Components\RichEditor;
use Filament\Forms\Components\TextInput;
use Filament\Forms\Set;

class Paragraph implements HasBlockSchema
{
    public static function getNamespace(): string
    {
        return 'Common';
    }

    public static function getBlock(): Block
    {
        return Block::make('paragraph')
            ->label('Paragraph')
            ->schema([
                Hidden::make('namespace')
                    ->afterStateHydrated(fn (Set $set) => $set('namespace', static::getNamespace())),
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
---

## Interfaces

Voor de blocks is er de interface `HasBlockSchema`, die bepaalt welke functionaliteiten een `Block`-class moet bieden. Dit omvat het beschikbaar stellen van formuliervelden en een methode om de content van het block te verwerken.

```
HasBlockSchema interface in het CMS met Filament
```
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
---

# Statamic CMS met flat-file / eloquent-driver

In de **flat-file** en **Eloquent-driver** configuratie van **Statamic** wordt het CMS anders opgebouwd dan bij de andere systemen die we gezien hebben. In plaats van specifieke Eloquent-modellen per entiteit, gebruikt Statamic `Collections` waarin gerelateerde `Entries` worden opgeslagen. Dit vereenvoudigt de opzet, doordat er geen specifieke databasetabellen voor entiteiten hoeven te worden voorbereid.

## Collections

In de huidige configuratie worden pagina's beheerd binnen een `Collection`. Deze collecties bepalen de globale instellingen voor de entries die tot de collectie behoren. Denk hierbij aan zaken zoals de manier waarop de overzichtspagina gesorteerd moet worden, of entries genest kunnen worden, en het definiëren van de route die aangeroepen moet worden om een specifieke entry uit de collectie op te halen. Je kunt een collectie vergelijken met een Eloquent-model dat communiceert met de databasetabel van een bepaalde entiteit. Dit kunnen naast pagina's ook andere entiteiten zijn, zoals blogposts of reviews.

In de **flat-file** en **eloquent-driver** configuratie worden ook de pagina's beheerd binnen een `Collection`, die de globale instellingen voor de `Page`-entries binnen de collectie bepaalt. Je zou `Collections` kunnen vergelijken met een repository voor vergelijkbare entiteiten binnen een systeem. 

```
Configuratie voor de Pages-collection
```
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
---

## Blueprints

In de **flat-file** en **eloquent-driver** configuratie worden de templates voor het beheren van dynamische content gedefinieerd binnen een `Blueprint`. Voor pagina's die meerdere templates vereisen, kunnen meerdere `Blueprints` worden aangemaakt voor de `Pages`-collectie. Dit stelt ons in staat om per template verschillende velden te definiëren. Voor entiteiten waar slechts één definitie hoort te zijn, dient slechts één enkele `Blueprint` aangemaakt te worden.

Een `Blueprint` bepaalt niet alleen het schema voor invoervelden, maar ook de validatieregels, standaardwaarden en de zichtbaarheid van velden in de gebruikersinterface.

```
Blueprint voor het homepage-template
```
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
---

## Fieldsets

Zoals je kunt zien in de `Blueprint` voor de homepage, is er een `replicator`-veld gedefinieerd met de handle `blocks`.

Dit veld stelt je in staat om vooraf gedefinieerde sets van invoervelden toe te voegen, die vervolgens als contentblokken gebruikt kunnen worden. Om te voorkomen dat deze sets telkens opnieuw gedefinieerd moeten worden, kunnen ze als `Fieldset` worden voorbereid. Deze `Fieldsets` kunnen daarna worden toegevoegd aan een `replicator`-veld.

```
Fieldset voor het Paragraph block
```
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
---

## Entries

In de **flat-file** en **eloquent-driver** confiugraties worden objecten gepersisteerd onder het `Entry`-model.

`Entries` worden in de flat-file configuratie opgeslagen als markdown-bestanden in de directory van de bijhorende `Collection`. Wanneer je gebruik maakt van de Eloquent-driver configuratie (en aangeeft dat `Entries` in de database opgeslagen moeten worden), wordt een `Entry` als JSON in een `entries`-tabel opgeslagen.

```
Entry in de `Pages`-collection, gebaseerd op de `Homepage`-blueprint.
```
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
---

## Synchronisatie van relaties

In de **flat-file** en **eloquent-driver** configuratie van Statamic is er geen automatische synchronisatie van bi-directionele relaties, wat kan leiden tot inconsistenties wanneer een relatie aan de ene kant van het object wordt verwijderd, maar aan de andere kant behouden blijft. Via de **Statamic Marketplace** is er echter de `Entry Relationships`-addon beschikbaar die automatische verwerking van bi-directionele relaties mogelijk maakt. 
> * [Entry Relationship addon in de Statamic Marketplate](https://statamic.com/addons/stillat/entry-relationships)

<br>

```
Voorbeeld van de RelationShipServiceProvider met EntryRelationship-addon
```
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
---

# Statamic CMS met Runway

In de configuratie waarbij gebruik wordt gemaakt van de **Runway**-addon verandert de architectuur vrij drastisch. In tegenstelling tot `Collections` en `Entries` maken we nu gebruik van `Eloquent`-modellen en specifieke databasetabellen voor alle entiteiten binnen het systeem. Hierdoor verliezen we helaas de standaard `navigation`- en `navigation-tree`-functionaliteiten van Statamic, inclusief de drag-and-drop functionaliteit om een menustructuur te definiëren. Om dit op te vangen is er in deze configuratie gekozen om een `menu manager` te gebruiken, zoals ook in het **Filament** CMS het geval is.

In de configuratie waarbij de **Runway**-addon wordt gebruikt, verandert de architectuur aanzienlijk. In plaats van `Collections` en `Entries`, maken we nu gebruik van `Eloquent`-modellen en specifieke databasetabellen voor alle entiteiten binnen het systeem. Dit betekent echter dat we de standaard `navigation`- en `navigation-tree`-functionaliteiten van Statamic verliezen, waaronder de **drag-and-drop-interface** om een menustructuur te definiëren. Om dit op te vangen is er in deze configuratie gekozen om een `menu manager` te gebruiken, zoals ook in het **Filament** CMS het geval is.

## Eloquent models

In principe zijn er weinig verschillen zichtbaar tussen het `Eloquent`-model voor **Statamic** en dat van **Filament**. Een opvallend verschil is de aanwezigheid van een andere **Trait**: `HasRunwayResource`. Deze Trait maakt het mogelijk om de modelen in Statamic weer te beheren.

Een ander belangrijk punt is dat de `template`-attribute wordt gecast naar een `array`. Dit wijst op een belangrijke eigenschap van deze configuratie: alle gegevens die via een `replicator` worden meegegeven (lees: de dynamische content) kunnen niet in een aparte tabel worden opgeslagen. In tegenstelling tot het **AllesOnline CMS** en de realisatie in **Filament**, wordt alle content onder het bijbehorende model als JSON gepersisteert.

```
Eloquent model in Statamic met Runway-addon**
```
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
---

## Runway Resources

Als we gebruik maken van de Runway configuratie is het niet meer mogelijk om gebruik te maken van `blueprints`. Dit omdat Runway gebruikt maakt van Runway Resources die eigenlijk op het zelfde niveau als `blueprints` zitten. Dit zijn dus dus de bestanden waarin de CMS schemas voor de entiteiten gedefineerd worden. 

Wanneer we gebruikmaken van de **Runway**-configuratie, is het niet meer mogelijk om gebruik te maken van `Blueprints`. Dit komt doordat Runway gebruikmaakt van Runway `Resources`, die zich op hetzelfde niveau als `Blueprints` bevinden. In plaats van `Blueprints` worden de CMS-schema’s voor de entiteiten nu gedefinieerd binnen deze Runway `Resources`.

```
Runway resource voor het schema van het page-model**
```
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

Doordat we met Runway de `collections` en daarmee de abstractielaag verliezen, is het niet langer mogelijk om verschillende templates voor een pagina als `Blueprint` op te geven. In plaats daarvan moeten de templates anders gedefinieerd worden, namelijk als `FieldSet`. In het voorbeeld hierboven kunnen gebruikers een template selecteren via een `replicator`. Binnen de `replicator` bevinden zich de velden die specifiek aan de gekozen template zijn gekoppeld.

```
FieldSet voor Homepage template
```
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

Binnen de `fieldSets` voor de templates is het mogelijk om door te verwijzen naar een `replicator` met `fieldSets` voor contentblokken. Deze blijven in principe ongewijzigd, wat betekent dat de dynamische content die via de `replicator` wordt ingevoerd, zoals tekst of afbeeldingen, in de templates kan worden opgenomen.

```
FieldSet voor het Paragraph block
```
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