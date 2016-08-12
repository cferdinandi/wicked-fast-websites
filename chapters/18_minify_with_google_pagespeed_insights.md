
# Minify with Google PageSpeed Insights

1. Visit Google PageSpeed Insights^[[https://developers.google.com/speed/pagespeed/insights/](https://developers.google.com/speed/pagespeed/insights/)].
2. Paste in your URL.
3. Click `Analyze`.

There will be a handful of recommendations, but if you scroll down, you'll see "Minify Javascript" and "Minify CSS." If you expand those, they'll even show you how much you'll reduce your file size by.

The reason they know this is because Google actually minifies your files for you and compares them to your unminified versions.

You can download the minified versions by scrolling down to "Download optimized image, JavaScript, and CSS resources for this page." Click the link to save a zipped version of your files.

Google is a bit more conservative with their minification than Gulp or CodeKit, so their files aren't quite as small. But if you don't have access to a GUI and aren't comfortable using command line, this is a great alternative.