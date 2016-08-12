
# Minify with Gulp

In the dummy project, the `src` directory contains two subdirectories:

1. `js` for our JavaScript files.
2. `sass` for our CSS files.

## Sass

We're using Sass to let us compile a bunch of modular files into a single file. You don't actually need to use Sass's advanced features to take advantage of this. All you need to do is change the file name extension form `.css` to `.scss` on your CSS files. This converts them to Sass files (CSS is valid Sass).

## Doing the heavy lifting

Let's open up `gulpfile.js` and take a look at a the Gulp modules that are doing the minification.

- Gulp Uglify is a JavaScript minification plugin.
- Gulp CSS Nano is a CSS minification plugin.
- Gulp Rename will rename our files. This let's us create both minified and unminified versions of our file with a single build.

The `banner` variable includes a smaller version of our header for inclusion in our minified files.

In our `build:scripts` task, we're:

1. Renaming our file with a `.min` suffix.
2. Passing it through Gulp Uglify.
3. Adding the minified banner.

In our `build:styles` task, we're:

1. Renaming our file with a `.min` suffix.
2. Passing it through Gulp CSS Nano.
3. Adding the minified banner.

In both cases, the full banner is removed as part of the minification process, so you don't need to worry about two banners showing up in the same file.

## Using Gulp

Open up the project folder in Terminal and run `gulp`. You should now see both minified and unminified versions of our files in the `dist` subdirectory. In my tests, files were about 50% - 75% smaller after minification.