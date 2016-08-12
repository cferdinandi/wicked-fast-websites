
# Expires Headers

Expires headers tell browsers to keep static assets stored locally so that the visitor’s browser doesn’t have to re-download them every time they visit your site.

While things like markup and RSS feeds should be refreshed on each page load, files like CSS, JavaScript, and images may not change for months or even years. You can ask browsers to hold on to these types of files for longer periods of time.

## Enabling Expires Headers

This is also something that’s done using the `.htaccess` file. We will again be using the code in HTML5 Boilerplate's server configuration file^[[https://github.com/h5bp/html5-boilerplate/blob/master/dist/.htaccess](https://github.com/h5bp/html5-boilerplate/blob/master/dist/.htaccess)]. Search for `Expires headers`.

This specifies how long the browser should retain various file formats.

Copy-and-paste the code between the opening and closing `<IfModule mod_expires.c>` tags into your `.htacess` file after the `#END WordPress` comment.

When you're done, run your page through Google PageSpeed Insights^[[https://developers.google.com/speed/pagespeed/insights/](https://developers.google.com/speed/pagespeed/insights/)] to check that it's working. If it is, you shouldn't see "Leverage Browser Caching" under things to be fixed.

## Cache-Busting

One problem with this approach: if you update a file, the visitor may already have an older version stored locally and won't load the new file with your changes. Fortunately, there's a really simple fix: giving a file a different name forces the browser to re-download it.

Instead of using a generic file name like this

```
main.min.js
```

Append a version number to the filename like this:

```
main.min.1.0.0.js
```

This is something that you can do manually, or you can use a tool like Gulp to automate it for you.