# Laravel Window Size and Breakpoints
Laravel `blade` directives and `php helpers` for **server side rendered content**, based on browser window size WITHOUT css.

An example to show the purpose of this package:
```php 
<?php

if(windowXs()) {
 //execute a tiny Eloquent query and return a minimalistic view
}
if(window2xl()) {
 //execute a huge Eloquent query and return a gigantic view
}
```

[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Travis](https://img.shields.io/travis/tanthammar/laravel-window-size.svg?style=flat-square)]()
[![Total Downloads](https://img.shields.io/packagist/dt/tanthammar/laravel-window-size.svg?style=flat-square)](https://packagist.org/packages/tanthammar/laravel-window-size)

# Requirements
* php 8
* Laravel 8
* IE11 not supported, (but you can update the blade component yourself :).

# Do you use Livewire?
There is a Livewire version of this package. [Link >>>](https://github.com/TinaHammar/livewire-window-size)


## Description
The main purpose of this package is not to avoid duplicated html,
but to avoid unnecessary server side code execution, just to render content that will never be seen.

* Vanilla JS syncs the browsers inner `width` and `height`, in realtime (debounced 750ms), when the browser window is resized.
* The corresponding Laravel controller will store the values to the Laravel `Session`.
* The package has a `config/breakpoints` file where you set your breakpoints
* The package provides multiple `@blade` directives and Laravel `helpers()`
* You have access to the browser window width/height via `session('windowW')` and `session('windowH')`
* You can change the config dynamically with Laravel `Config::set(...)`
* The route is throttling the request `15:1`. (The route is: `/update-laravel-window-size`)

# Important note
It's important to understand the difference between the server side rendered breakpoints, that this package provides, and css media queries.

When using css, the content of the page will update in realtime as the user resizes the window,
whereas this package debounces a network request and updates the page on the next page request.

It's important that you place the `<x-window-size::save-to-session />` at first point of contact with the user, for your application to look its best. I have it in my `app.blade.php` and on the `login`/`register` pages.


## Installation
```
composer require tanthammar/laravel-window-size
```

## Publish config
```
php artisan vendor:publish --tag=laravel-window-size
```


The default settings are based on TailwindCSS breakpoints
```php
'window-width' => [
    // 0 = undefined|false
    // @windowXs (>= 1px && < Sm)
    'Sm' => 640, // => @windowSm (>= 640px && < Md)
    'Md' => 768, // => @windowMd (>= 768px && < Lg)
    'Lg' => 1024, // => @windowLg (>= 1024px && < Xl)
    'Xl' => 1280, // => @windowXl (>= 1280px && < 2xl)
    '2xl' => 1536, // => @window2xl (>= 1536px)
],
```

## Add the component to your layout
* Add this to all layouts where you want to keep track of the browser window size.
* You will have access to the browser window width/height via `session('windowW')` and `session('windowH')`

Example: `app.blade.php`
```blade
<x-window-size::save-to-session />
```

## Blade directives
@elsif..., @else..., @end..., @unless... and @endif works with all the directives. Explanation in [Laravel docs](https://laravel.com/docs/8.x/blade#custom-if-statements).
```blade
//Browser width, with example values
@windowWidthLessThan(400)
@windowWidthGreaterThan(399)
@windowWidthBetween(400, 1500)

//Browser height, with example values
@windowHeightLessThan(500)
@windowHeightGreaterThan(499)
@windowHeightBetween(400, 900)

//Breakpoints based on config values
@windowXs()
@windowSm()
@windowMd()
@windowLg()
@windowXl()
@window2xl()
```
Example
```blade 
@windowXs()
<div>This window is extra small</div>
@endif

@window2xl()
<div>This window is very large</div>
@endif
```

## Helpers
Same name as Blade directives
```php
//Browser width, with example values
windowWidthLessThan(400)
windowWidthGreaterThan(399)
windowWidthBetween(400, 1500)

//Browser height, with example values
windowHeightLessThan(500)
windowHeightGreaterThan(499)
windowHeightBetween(400, 900)

//Breakpoints based on config values
windowXs()
windowSm()
windowMd()
windowLg()
windowXl()
window2xl()
```

Example php
```php 
if(windowXs()) {
 //execute a tiny Eloquent query and return a minimalistic view
}
if(window2xl()) {
 //execute a huge Eloquent query and return a gigantic view
}
```

## Blade directives test component
Add this to any blade view to test the blade directives
```blade 
<x-window-size::test-windowsize />
```

## 💬 Let's connect
* [🔗 **Twitter**](https://twitter.com/TinaHammar)
*  🔗 Please 💗 [sponsor me](https://github.com/sponsors/tanthammar) if you like my work.

## Credits
* [Martin Krisell](https://github.com/Krisell)


## Changelog
Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Contributing
Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## License
The MIT License (MIT). Please see [License File](/LICENSE.md) for more information.
