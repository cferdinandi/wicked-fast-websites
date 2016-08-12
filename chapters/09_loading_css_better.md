
# Loading CSS Better

Your CSS doesn't have to go in your `style.css` file. In fact, your CSS *shouldn't* go in your `style.css` file.

You still want to include a `style.css` file, but we just use it to hold our theme header information. All of our CSS will go in another file.

```css
/**
 * Theme Name: my-project
 * Theme URI: https://github.com/myusername/my-project
 * Description: A description of my project
 * Version: 1.0.0
 * Author: My Name
 * Author URI: http://myurl.com
 * License: MIT
 */
```

Why? We want more control over how, when, and where our CSS loads.

## CSS blocks rendering

Your stylesheet is necessary for rendering content properly, but while it’s being downloaded and parsed, it also blocks any rendering from happening.

To get around this challenge, an emerging technique recommended by folks like Google^[[https://developers.google.com/speed/pagespeed/service/PrioritizeCriticalCss](https://developers.google.com/speed/pagespeed/service/PrioritizeCriticalCss)] and Filament Group^[[http://www.filamentgroup.com/lab/performance-rwd.html](http://www.filamentgroup.com/lab/performance-rwd.html)] is to inline your critical path CSS.

Yes, that’s right. Inline your CSS. It works like this:

- Extract the styles that apply to above-the-fold content and load them inline in the `<head>`.
- Load your full stylesheet asynchronously so that the rest of your page can continue downloading and rendering.

Sounds wacky, but it makes a big difference, particularly on larger sites with big stylesheets.

## How to decide what to inline

When I mentioned this technique to a few folks on Twitter, the most common question I got was how I decided what to inline.

If you use a JS task runner like Gulp^[[http://gulpjs.com](http://gulpjs.com)] or Grunt^[[http://gruntjs.com](http://gruntjs.com)], there are a few plugins that you can use to automate this: Critical by Addy Osmani^[[https://github.com/addyosmani/critical](https://github.com/addyosmani/critical)] and Critical CSS by Filament Group^[[https://github.com/filamentgroup/criticalCSS](https://github.com/filamentgroup/criticalCSS)].

There is also an online generator^[[http://jonassebastianohlsson.com/criticalpathcssgenerator/](http://jonassebastianohlsson.com/criticalpathcssgenerator/)] if command line isn’t thing.

**I don’t use any of these.**

I tried using them, and found that they often left out styles important for rendering my layout on smaller or taller viewports (as in, my iPhone), so I create my critical CSS manually.

## The magic number

The magic number you should care about is 14kb.

That’s (give or take) how much data a server sends per round trip when the browser makes a request for a web page. You want your above-the-fold content---and its associated styles and scripts—--to weigh 14kb or less so that the browser can start rendering it as soon as that first packet of data is received.

## How to inline and async your CSS

There's no browser-native way to asynchronously load CSS.

Fortunately, Filament Group created loadCSS^[[https://github.com/filamentgroup/loadCSS](https://github.com/filamentgroup/loadCSS)], a lightweight JavaScript plugin that loads CSS asynchronously. Include loadCSS inline so that you don't have to wait for it download before running.

```html
<head>
    <style>
        /* Your inline CSS... */
    </style>
    <script>
        function loadCSS(){ ... }
        loadCSS( '/path/to/your/full.css' );
    </script>
</head>

You should also add a `<noscript>` fallback in the footer (again, to prevent render blocking) for browsers that don’t support JavaScript or have it turned off.

```html
<noscript>
    <link rel="stylesheet" type="text/css" href="/path/to/your/full.css">
</noscript>
```

## The WordPress Way

Here's how you do that the WordPress way. In your `functions.php` file, you'll typically have a function you use to register and load your scripts:

```php
// Load theme styles
function load_theme_styles() {
	wp_enqueue_style( 'theme-styles', get_template_directory_uri() . '/path/to/your/main.css', null, null, 'all' );
}
add_action('wp_enqueue_scripts', 'load_theme_styles');
```

Instead of doing that, we're going to hook into the `wp_head` action to inject some content inline:

```php
// Load styles inline in the header
function load_critical_css_inline_header() {
	?>
		<style type="text/css">
			/**
			 * Your Critical Path CSS
			 */
		</style>
		<script>
			function loadCSS(){ … }
			loadCSS( "<?php echo get_template_directory_uri() . '/path/to/your/main.css'; ?>" );
		</script>
	<?php
}
add_action('wp_head', 'load_critical_css_inline_header', 30);
```

If your server supports `file_get_contents`, you can even use that to avoid having to copy and paste the contents of your critical path CSS and loadCSS into your `functions.php` file:

```php
// Load styles inline in the header
function load_critical_css_inline_header() {
	?>
		<style type="text/css">
			<?php echo file_get_contents( get_template_directory_uri() . '/path/to/your/critical.css' ); ?>
		</style>
		<script> 
			<?php echo file_get_contents( get_template_directory_uri() . '/path/to/your/loadCSS.js' ); ?>
			loadCSS( "<?php echo get_template_directory_uri() . '/path/to/your/main.css'; ?>" );
		</script>
	<?php
}
add_action('wp_head', 'load_critical_css_inline_header', 30);
```

You also want to hook into the `wp_footer` action to inline a fallback link:

```php
// Load style fallbacks inline in the footer
function load_fallback_css_footer() {
	?>
		<noscript>
			<link rel="stylesheet" type="text/css" href="<?php echo get_template_directory_uri() . '/path/to/your/main.css'; ?>">
		</noscript>
	<?php
}
add_action('wp_footer', 'load_fallback_css_footer', 30);
```

## What about browser caching?

Using this approach means that the browser is no longer able to take advantage of having your stylesheet cached for reuse on subsequent pages and visits.

Fortunately, there is a relatively easy way to get around this, developed by the wonderfully talented folks at Filament Group: set a cookie when the full stylesheet is loaded asynchronously, and then check for that cookie on subsequent page visits. If it’s there, skip the critical CSS inlining and just load the stylesheet via a traditional `<link>` element using `wp_enqueue_style`.

### Setting the cookie with loadCSS

Setting the cookie requires one additional, super lightweight script from the Filament Group, onloadCSS, that runs a callback when the CSS file is loaded. onloadCSS comes bundled with loadCSS.

```javascript
function loadCSS () { ... }
function onloadCSS () { ... }

var stylesheet = loadCSS( "<?php echo get_template_directory_uri() . '/path/to/your/main.css'; ?>" );

onloadCSS( stylesheet, function() {
    var expires = new Date(+new Date + (7 * 24 * 60 * 60 * 1000)).toUTCString();
    document.cookie = 'fullCSS=true; expires=' + expires;
});
```

### The WordPress Way

```php
// Load theme styles
function load_theme_styles() {
    if ( isset($_COOKIE['fullCSS']) && $_COOKIE['fullCSS'] === 'true' ) {
        wp_enqueue_style( 'theme-styles', get_template_directory_uri() . '/path/to/your/main.css', null, null, 'all' );
    }
}
add_action('wp_enqueue_scripts', 'load_theme_styles');

// Load styles inline in the header
function load_critical_css_inline_header() {
	if ( !isset($_COOKIE['fullCSS']) || $_COOKIE['fullCSS'] !== 'true' ) :
	?>
		<style type="text/css">
			<?php echo file_get_contents( get_template_directory_uri() . '/path/to/your/critical.css' ); ?>
		</style>
		<script> 			<?php echo file_get_contents( get_template_directory_uri() . '/path/to/your/loadCSS.js’ ); ?>        <?php echo file_get_contents( get_template_directory_uri() . '/dist/js/onloadCSS.js’ ); ?>
			var stylesheet = loadCSS( "<?php echo get_template_directory_uri() . '/path/to/your/main.css'; ?>" );
			onloadCSS( stylesheet, function() { // Set cookie });
		</script>
	<?php
	endif;
}
add_action('wp_head', 'load_critical_css_inline_header', 30);

// Load styles fallback inline in the footer
function load_fallback_css_footer() {
	if ( !isset($_COOKIE['fullCSS']) || $_COOKIE['fullCSS'] !== 'true' ) :
	?>
		<noscript>
			<link rel="stylesheet" type="text/css" href="<?php echo get_template_directory_uri() . '/path/to/your/main.css'; ?>">
		</noscript>
	<?php
	endif;
}
add_action('wp_footer', 'load_fallback_css_footer', 30);
```