
# Loading JavaScript Better

Like your CSS, JavaScript also blocks content rendering. Unlike CSS, it also stops all other files from downloading until it has been downloaded and parsed.

Normally, a browser will download two assets from the same source at once. This is important because it means that one large file won't become a bottleneck for other files. However, because JavaScript often makes additions or changes to the DOM, the browser stops everything and waits until it's parsed before continuing.

That's smart, but it also means that your scripts can have both real and perceived negative impacts on performance. So what can you do about it?

## Be smart about script location in the markup

Feature detection scripts may affect how CSS renders content, so they should go in the `<head>`. Ideally they're small and lightweight, and you'll inline them so the browser doesn't have to wait for a file to download before running them.

Everything else should go in the footer.

### What about `async` and `defer`?

`async` and `defer` are attributes you can add to your `<script>` tag:

```html
<script src="path/to/your/main.js" async></script>
<script src="path/to/your/main.js" defer></script>
```

`async` tells the browser that it shouldn't block rendering or prevent other files from downloading while the script itself downloads. `defer` tells the browser to wait until everything else is loaded before downloading the file.

`defer` is well supported, but useless. It does the same thing as loading your scripts in the footer, so you might as well just put them there in the first place.

`async` is awesome, but has less support. On unsupported browsers, the script will still block rendering. And since most of your DOM manipulation scripts need to wait until the rest of the content has loaded and parsed anyways, you might as well just load your scripts on the footer.

### The WordPress Way

In WordPress, we use the `wp_enqueue_script` function to load scripts. It accepts a handful of arguments.

The very last one is `$in_footer`. Setting it to `false` loads the script in the header. Setting it to `true` loads the script in the footer.

```php
// Load theme scripts
function load_theme_scripts() {

    // Feature detects: loads in header
    wp_enqueue_script( 'theme-detects', get_template_directory_uri() . '/path/to/your/detects.js', null, null, false );

    // Main scripts: loads in footer
    wp_enqueue_script( 'theme-scripts', get_template_directory_uri() . '/path/to/your/main.js', null, null, true );

}
add_action('wp_enqueue_scripts', 'load_theme_scripts');
```

## The Magic Number

Remember, 14kb is the magic number-the approximate amount of data sent in one roundtrip HTTP request.

If your feature detection scripts are small and fall below that number, it might be worth inlining them in the header instead of putting them in an external file. The avoids a download-blocking external JavaScript file.

If your server supports it, you can use the `file_get_contents` function instead of copy/pasting content into your `functions.php` file. That way, if you ever update your scripts the new content is automatically added.

```php
// Load scripts inline in the header
function load_scripts_inline_header() {
	?>
		<script>
			<?php echo file_get_contents( get_template_directory_uri() . '/path/to/your/detects.js' ); ?>
		</script>
	<?php
}
add_action('wp_head', 'load_scripts_inline_header', 30);
```

It's worth noting that this is *not* the WordPress way of doing things, but I think it's worth doing for the improved performance benefits.