
# Fixing Fonts

Loading web fonts often results in a Flash of Invisible Text (FOIT) that leaves the page unusable until it loads.

Ilya Grigorik, a web performance engineer at Google, reports:^[[https://www.igvita.com/2015/04/10/fixing-the-blank-text-problem/](https://www.igvita.com/2015/04/10/fixing-the-blank-text-problem/)]

> 29% of page loads on Chrome for Android displayed blank text: the user agent knew the text it needed to paint, but was blocked from doing so due to the unavailable font resource. In the median case the blank text time was ~350 ms, ~750 ms for the 75th percentile, and a scary ~2300 ms for the 95th.

Even the median value of 350ms is an unacceptable performance hit. Fortunately, there's a simple fix.

## A better way

A better approach is to have a system font load immediately, and then when your web font is available, switch over to that. It results in a repaint, but your content is immediately available to visitors.

To achieve this, we're going to use the same approach we use to for loading CSS better: `loadCSS.js`^[[https://github.com/filamentgroup/loadCSS](https://github.com/filamentgroup/loadCSS)].

However, just loading asynchronously isn't enough. You'll still get a FOIT doing that---it will just happen a second or three later.

We also need a way to detect when the font is fully loaded. Fortunately, Font Face Observer from Bram Stein^[[https://github.com/bramstein/fontfaceobserver](https://github.com/bramstein/fontfaceobserver)] does just that!

We'll load the font asynchronously, and use Font Face Observer to detect when it's loaded. When it loads, we'll add a class to the `<html>` element that we can hook into for styling.

```css
body {
	font-family: Georgia, serif;
}

.fonts-loaded body {
	font-family: 'PT Serif', serif;
}
```

## The WordPress Way

Traditionally, you would load your web font using the `wp_enqueue_style` function:

```php
// Load theme fonts
function load_theme_fonts() {
    wp_enqueue_style( 'theme-fonts', 'https://fonts.googleapis.com/css?family=Open+Sans', null, null, 'all' );
}
add_action('wp_enqueue_scripts', 'load_theme_fonts');
```

Instead, we want to hook into the `wp_head` action to inline loadCSS:

```php
// Load fonts inline in the header
function load_fonts_inline_header() {
	?>
		<script>
			// Inline our JS files
			function loadCSS(){ ... };
			// Also inline all of the contents from fontfaceobserver.js

			// Async load the CSS file
			loadCSS('//fonts.googleapis.com/css?family=PT+Serif:400,400italic,700,700italic');

			// When the font loads, as the .fonts-loaded class to the <html> element
			var font = new FontFaceObserver('PT Serif');
			font.load().then(function () {
				document.documentElement.className += ' fonts-loaded';
			});
		</script>
	<?php
}
add_action('wp_head', 'load_fonts_inline_header', 30);
```

This is technically not the "WordPress Way" of doing things. The Codex stress using `wp_enqueue_style` so that stylesheets are registered and accessible to plugins and other functions. But I think the performance benefits far outweigh the cost of deviating from the best practice here.

## What about browser caching?

CSS files loaded from places like Typekit and Google Web Fonts get cached for about a week.

Once they're stored in the browser cache, we don't need to use the technique described above. We can load them the traditional way to avoid the font switch that happens once the font CSS loads asynchronously.

To make this happen, we'll set a cookie with Font Face Observer when the font loads. In our `functions.php` file, we'll use a different approach depending on whether or not that cookie exists.


```php
// If cookie set, load font traditional way
function load_theme_fonts() {
	if ( isset($_COOKIE['fontsLoaded']) && $_COOKIE['fontsLoaded'] === 'true' ) {
    	wp_enqueue_style( 'theme-fonts', 'https://fonts.googleapis.com/css?family=Open+Sans', null, null, 'all' );
    }
}
add_action('wp_enqueue_scripts', 'load_theme_fonts');


// Otherwise, load the font async
function load_theme_font_async() {
	?>
		<script>
			<?php if ( !isset($_COOKIE['fontsLoaded']) || $_COOKIE['fontsLoaded'] !== 'true' ) : ?>
				// Inline our JS files
				function loadCSS(){ ... };
				// Also inline all of the contents from fontfaceobserver.js

				// Async load the CSS file
				loadCSS('//fonts.googleapis.com/css?family=PT+Serif:400,400italic,700,700italic');

				// When the font loads, as the .fonts-loaded class to the <html> element
				var font = new FontFaceObserver('PT Serif');
				font.load().then(function () {
					var expires = new Date(+new Date() + (7 * 24 * 60 * 60 * 1000)).toUTCString();
					document.cookie = 'fontsLoaded=true; expires=' + expires;
					document.documentElement.className += ' fonts-loaded';
				});
			<?php endif; ?>
		</script>
	<?php
}
add_action('wp_head', 'load_theme_font_async', 30);
```