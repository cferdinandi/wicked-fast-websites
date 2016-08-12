
# Minify with CodeKit

In the dummy project, there are two directories:

1. `js` for our JavaScript files.
2. `sass` for our CSS files.

## Sass

We're using Sass to let us compile a bunch of modular files into a single file. You don't actually need to use Sass's advanced features to take advantage of this. All you need to do is change the file name extension form `.css` to `.scss` on your CSS files. This converts them to Sass files (CSS is valid Sass).

## Using CodeKit

![Minifying JavaScript with CodeKit](img/codekit-js-settings.jpg)

![Minifying CSS with CodeKit](img/codekit-sass-settings.jpg)

1. Click the gear icon
2. Select `JavaScript` from the `Languages` dropdown. Choose `Minified` from the output style dropdown (this is the default).
3. Select `Sass` from the `Languages` dropdown. Choose `Compressed` from the output style dropdown.

Your files will now be minified when they compile. In my tests, files were about 50% - 75% smaller after minification.