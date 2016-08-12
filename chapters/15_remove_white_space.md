
# Remove White Space

Minification is the process of removing spaces, line breaks and comments from your CSS and JavaScript. Though it might not seem like a big deal, removing all of that unused whitespace can have a big impact on the overall size of your files.

I ran a quick test and and minifying my stylesheet reduced its size by 75-percent.

## There's just one problem

Minified files are essentially unusable if you need to make updates or changes. Everything is on a single line, and with JavaScript files, your intuitive variables are often renamed to single letters for even better size reduction.

![Minified CSS](img/minified-css.jpg)

Keep an un-minified version of your file that you use for development. Save your minified version as a separate file with a `.min` suffix. This also makes debugging easier during the development phase.

```
main.css
main.min.css

main.js
main.min.js
```

### The WordPress Way

In your `functions.php` file, update your `wp_enqueue_script` function with the new filename. This:

```php
// Load theme files
function load_theme_files() {

    // Feature detects
    wp_enqueue_script( 'theme-detects', get_template_directory_uri() . '/path/to/your/detects.js', null, null, false );

    // Main scripts
    wp_enqueue_script( 'theme-scripts', get_template_directory_uri() . '/path/to/your/main.js', null, null, true );

}
add_action('wp_enqueue_scripts', 'load_theme_files');
```

Becomes this:

```php
// Load theme files
function load_theme_files() {

    // Feature detects
    wp_enqueue_script( 'theme-detects', get_template_directory_uri() . '/path/to/your/detects.min.js', null, null, false );

    // Main scripts
    wp_enqueue_script( 'theme-scripts', get_template_directory_uri() . '/path/to/your/main.min.js', null, null, true );

}
add_action('wp_enqueue_scripts', 'load_theme_files');
```

## Tools and Techniques

Removing all of the whitespace from your files by hand would be insane. There are a bunch of tools that make it easier:

**GUIs**

- CodeKit for Mac^[[https://incident57.com/codekit/](https://incident57.com/codekit/)]
- Prepos for Windows^[[https://prepros.io/](https://prepros.io/)]
- Google PageSpeed Insights^[[https://developers.google.com/speed/pagespeed/insights/](https://developers.google.com/speed/pagespeed/insights/)]

**Command Line**

- Gulp^[[http://gulpjs.com/](http://gulpjs.com/)]
- Grunt^[[http://gruntjs.com/](http://gruntjs.com/)]

**Plugins**

- MinQueue^[[https://wordpress.org/plugins/minqueue/](https://wordpress.org/plugins/minqueue/)]