
# Cache-Busting: A Hybrid Approach

If command line isn't your preferred way of doing development, but you'd still like to automate your cache-busting implementation, there's actually a hybrid approach you can use that reduces the amount of work you have to do by quite a bit.

In the theme header of your `style.css` file, make sure that the `version` for your theme matches the version suffix for your updated files.

Now, open up `functions.php`. The `wp_get_theme()` function gets a bunch of theme about our WordPress theme. We want to set it to a variable---`$theme`---and use it in our filenames.

Where we load and inline files in `functions.php`, append the filename with `$theme->get( 'Version' )`. This gets the version number from our theme header in `style.css` and adds it to the filename.

```php
function load_theme_files() {
	$theme = wp_get_theme();
	wp_enqueue_style( 'theme-styles', get_template_directory_uri() . '/path/to/your/main.min.' . $theme->get( 'Version' ) . '.css', null, null, 'all' );
}
add_action( 'wp_enqueue_scripts', 'load_theme_files' );
```

Now, anytime you change your files, update the filename and version number in `style.css`, and your new files will automatically load. Just make sure you add the same version suffix to all files.