<p align="center">
    <img src="https://user-images.githubusercontent.com/7388088/32410265-fe5f8d72-c1c4-11e7-97d7-c7693d44f961.png">
</p>

# Odin

is a GUI to manage model revisions.

## Installation

- package requires Laravel v5.4+

- `composer require ctf0/odin`

- (Laravel < 5.5) add the service provider

```php
'providers' => [
    ctf0\Odin\OdinServiceProvider::class,
]
```

- publish the package assets with

`php artisan vendor:publish --provider="ctf0\Odin\OdinServiceProvider"`

- after installation, package will auto-add
    + package routes to `routes/web.php`
    + package assets compiling to `webpack.mix.js`

- check http://www.laravel-auditing.com/docs/4.1/installation for configuration

- install dependencies

```bash
yarn add vue vuemit vue-notif vue-scrollto keycode
# or
npm install vue vuemit vue-notif vue-scrollto keycode --save
```

## Features

- support single & nested values
- delete & restore revisions.
- auto clean audit table if `old_value & new_value` are empty
- shortcuts

    |      navigation      |  keyboard  |    mouse (click)    |
    |----------------------|------------|---------------------|
    | go to next revision  | right/down | * *(revision date)* |
    | go to prev revision  | left/up    | * *(revision date)* |
    | go to first revision | home       | * *(revision date)* |
    | go to last revision  | end        | * *(revision date)* |
    | hide revision window | esc        | * *(x)*             |

- events "JS"

    | event-name |       description       |
    |------------|-------------------------|
    | odin-show   | when revision is showen |
    | odin-hide   | when revision is hidden |

## Usage

- add `Revisions` trait & `AuditableContract` contract to your model

```php

use ctf0\Odin\Traits\Revisions;
use Illuminate\Database\Eloquent\Model;
use OwenIt\Auditing\Contracts\Auditable as AuditableContract;

class Post extends Model implements AuditableContract
{
    use Revisions;

    // ...
}
```

- inside the model view ex.`post edit view` add

```blade
@if (count($post->revisions))
    @include('Odin::list', ['revisions' => $post->revisions])
@endif
```

- for styling we use ***bulma***

- add this one liner to your main js file and run `npm run watch` to compile your `js/css` files.
    + if you are having issues [Check](https://ctf0.wordpress.com/2017/09/12/laravel-mix-es6/).

```js
require('./../vendor/Odin/js/manager')

new Vue({
    el: '#app'
})
```

### Note About `data:uri`

- if you use `data:uri` in your revisionable content, saving it atm will give error because the `audits_table columns are in Text`, so to solve that simply change the column type to either `mediumText` or `longText` before migrating.

    also note because `data:uri` is a render blocking, so opening the sidebar will have some delay.
