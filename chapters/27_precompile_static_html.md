
# Pre-Compile Static HTML

![](img/precompile.jpg)

When a browser requests a page from your site, your server:

1. Grabs content from the database.
2. Gets the template files from your theme.
3. Compiles the database content and template files into HTML.
4. Sends this static HTML file to the browser.

This happens every single time someone visits any page on your site. If someone visits your homepage, the visits another page, then comes back to your homepage 30 seconds later, that page is still recompiled, even though it's unlikely anything changed.

This process is slow and increases your time to first byte. Particularly on inexpensive, shared hosting, it can add quite a bit of latency to your site.

## Tell WordPress to compile HTML files ahead of time

Fortunately, you can tell WordPress to pre-compile static HTML files ahead of time.

This is something that a plugin will handle for you. You don't even need to think about it.

Two often recommended plugins are W3 Total Cache^[[https://wordpress.org/plugins/w3-total-cache/](https://wordpress.org/plugins/w3-total-cache/)] and WP Super Cache^[[https://wordpress.org/plugins/wp-super-cache/](https://wordpress.org/plugins/wp-super-cache/)]. I personally find both of these plugins far too complicated.

![](img/comet-cache.jpg)

My favorite plugin for this is Comet Cache^[[https://wordpress.org/plugins/comet-cache/](https://wordpress.org/plugins/comet-cache/)]. I've used it on every site I've built for the last two years. While it does have some advanced settings, the only thing you need to do is activate it and check "Enable Comet Cache." The plugin handles the rest, and it just works.

## Caching and critical path CSS

When you're a caching plugin, the server sends the same pre-compiled HTML file to everyone. If you're using the critical path CSS technique, the server never gets a chance to check for the cookie that says the CSS file was loaded, so it will load asynchronously for everyone. The same is true of the asynchronous font loading technique.

```php
// Load theme styles
function load_theme_styles() {
    if ( isset($_COOKIE['fullCSS']) && $_COOKIE['fullCSS'] === 'true' ) {
        wp_enqueue_style( 'theme-styles', get_template_directory_uri() . '/path/to/your/main.css', null, null, 'all' );
    }
}
add_action('wp_enqueue_scripts', 'load_theme_styles');

// If cookie set, load font traditional way
function keel_load_theme_files() {
    if ( isset($_COOKIE['fontsLoaded']) && $_COOKIE['fontsLoaded'] === 'true' ) {
        wp_enqueue_style( 'keel-theme-fonts', '//fonts.googleapis.com/css?family=PT+Serif:400,400italic,700,700italic', null, null, 'all' );
    }
}
add_action('wp_enqueue_scripts', 'keel_load_theme_files');
```

Fortunately, Comet Cache provides a way for you to create different pre-compiled versions based on whether or not a cookie is set.

![Adding dynamic caching via the Comet Cache GUI](img/comet-cache-cookies.jpg)

If you pay for the pro version, this is as simple as adding the cookie to the GUI in the WordPress Dashboard. You can also do this manually (for free) with some PHP and an FTP client.

### Creating a dynamic version salt

In your `wp-content` directory, we want to add a new file, `critical-path-salt.php` to the `ac-plugins` sub-directory (if that folder doesn't exist, create it):

```
wp-content/ac-plugins/critical-path-salt.php
```

In the `critical-path-salt.php` file, we want to do two things:

1. Modify the `$version_salt` variable based on whether or not our cookie exists.
2. Add a filter against Comet Cache's salting function to update our variable.

```php
// Prevent people from accessing this file directly
if ( !defined( 'WPINC' ) ) {
    exit( 'Do NOT access this file directly: '.basename(__FILE__) );
}

/*
 * Dynamically generate a new version salt based on the cookie
 */
function critical_css_salt_shaker( $version_salt ) {

    // Check for fullCSS cookie
    if ( isset($_COOKIE['fullCSS']) && $_COOKIE['fullCSS'] === 'true' ) {
        $version_salt .= 'fullcss'; // Give users with cached CSS files their own variation of the cache.
    } else {
        $version_salt .= 'inlinecss'; // A default group for all others.
    }

    // Check for fontsLoaded cookie
    if ( isset($_COOKIE['fontsLoaded']) && $_COOKIE['fontsLoaded'] === 'true' ) {
        $version_salt .= 'fontsloaded'; // Give users with cached CSS files their own variation of the cache.
    } else {
        $version_salt .= 'fontsnotloaded'; // A default group for all others.
    }

    return $version_salt;

}

/**
 * Add a new version salt
 */
function critical_css_salt_plugin() {
	  $ac = $GLOBALS['comet_cache_advanced_cache'];
	  $ac->addFilter('comet_cache_version_salt', 'critical_css_salt_shaker');
}
critical_css_salt_plugin();
```