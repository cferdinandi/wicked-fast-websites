
# Cache-Busting with Gulp

I can update my version number in a single place---my `package.json` file---and have it automatically update all of my filenames, the theme header in my `style.css` file, and references to all of my files in my `functions.php` file.

## How this works

In our `gulpfile.js`, the `theme` key under the `path` variable points to an empty `style.css` file in our `src` directory. We're going to grab that file and dynamically add our theme header with our version number.

Similarly, there's a `banner` key for `theme` as well. This is our dynamic theme header, and it pulls in information---including the version number---from our `package.json` file.

## `gulpfile.js` tasks

In `build:scripts` and `build:styles`, the `.min` suffix that's added to our minified files is appended with `. + package.version`. This grabs the version number from `package.json` and adds it to our filename. Anytime you update the version number, the filename changes to match.

The `build:theme` task grabs that empty `style.css` file in `src`, adds our dynamic theme banner, and renders the new file in the primary directory.

## `functions.php`

The `wp_get_theme()` function gets a bunch of info about our WordPress theme. We want to set it to a variable---`$theme`---and use it in our filenames.

Where we load and inline files in `functions.php`, append the filename with `$theme->get( 'Version' )`. This gets the version number from our theme header in `style.css` and adds it to the filename.

```php
function load_theme_files() {
	$theme = wp_get_theme();
	wp_enqueue_style( 'theme-styles', get_template_directory_uri() . '/path/to/your/main.min.' . $theme->get( 'Version' ) . '.css', null, null, 'all' );
}
add_action( 'wp_enqueue_scripts', 'load_theme_files' );
```

## Putting it all together

Now, all you have to do is update your version number in `package.json` and run `gulp`. All of your files will be renamed, your theme will get an updated header, and references to your files will update to match the new file names.