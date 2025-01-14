# **Runway configuratiebestand**

```php
<?php

use App\Models\Actor;
use App\Models\BlogPost;
use App\Models\Book;
use App\Models\Category;
use App\Models\Director;
use App\Models\Menu;
use App\Models\MenuManager;
use App\Models\Movie;
use App\Models\Page;
use App\Models\Review;

return [

    /*
    |--------------------------------------------------------------------------
    | Resources
    |--------------------------------------------------------------------------
    |
    | Configure the resources (models) you'd like to be available in Runway.
    |
    */

    'resources' => [
        Menu::class => [
            'name' => 'Menu',
            'hidden' => true,
        ],
        Page::class => [
            'name' => 'Pages',
            'cp_icon' => 'drawer-file',
        ],
        MenuManager::class => [
            'name' => 'Menu manager',
            'title_field' => 'name',
            'cp_icon' => 'list',
        ],
        BlogPost::class => [
            'name' => 'Blog Post',
            'title_field' => 'title',
            'cp_icon' => 'entries',
        ],
        Category::class => [
            'name' => 'Category',
            'title_field' => 'name',
            'cp_icon' => 'tags',
        ],
        Movie::class => [
            'name' => 'Movies',
            'title_field' => 'title',
            'cp_icon' => 'video',
        ],
        Actor::class => [
            'name' => 'Actors',
            'title_field' => 'full_name',
            'cp_icon' => 'user',
        ],
        Director::class => [
            'name' => 'Directors',
            'title_field' => 'full_name',
            'cp_icon' => 'user',
        ],
        Book::class => [
            'name' => 'Books',
            'title_field' => 'title',
            'cp_icon' => 'book-pages',
        ],
        Review::class => [
            'name' => 'Reviews',
            'title_field' => 'title',
            'cp_icon' => 'entries',
        ],
    ],

    /*
    |--------------------------------------------------------------------------
    | Runway URIs Table
    |--------------------------------------------------------------------------
    |
    | When using Runway's front-end routing functionality, Runway will store model
    | URIs in a table to enable easy "URI -> model" lookups. If needed, you can
    | customize the table name here.
    |
    */

    'uris_table' => 'runway_uris',

    /*
    |--------------------------------------------------------------------------
    | Disable Migrations?
    |--------------------------------------------------------------------------
    |
    | Should Runway's migrations be disabled?
    | (eg. not automatically run when you next vendor:publish)
    |
    */

    'disable_migrations' => false,

];
```