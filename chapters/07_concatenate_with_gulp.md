
# Concatenate with Gulp

Unlike GUIs like CodeKit, Gulp can be configured in a seemingly endless number of combinations. For this course, I've put together a simple Gulp Boilerplate that we can use to concatenate our JavaScript and CSS.

Obviously if you're comfortable with Gulp or command line, go ahead and configure your own setup to best fit your needs.

In the Gulp Boilerplate, the `src` directory contains two subdirectories:

1. `js` for our JavaScript files.
2. `sass` for our CSS files.

## Sass

We're using Sass to let us compile a bunch of modular files into a single file. You don't actually need to use Sass's advanced features to take advantage of this. All you need to do is change the file name extension from `.css` to `.scss` on your CSS files. This converts them to Sass files (CSS is valid Sass).

In the `components` subdirectory, the files are all prefixed with an underscore. This tells our Sass compiler not to compile these files directly, since we're referencing them somewhere.

That somewhere else is in our `main.scss` file. In this file, you'll see several lines of `@import "components/filename"`. This tells our compiler to fetch the contents of that file and drop it in. You'll notice that both the underscore and the extension are missing from the filename. Sass just figures it out.

## JavaScript

In our `js` directory, we have two subdirectories:

1. `detects`
2. `main`

The files in `detects` will be compiled into `detects.js`, and the files in `main` will be compiled into `main.js`. The way this Gulp build is setup, you can rename those folders or add others. The files in the subdirectories always compile into a file named after the folder the files are in.

## Using Gulp

1. Open up your terminal window.
2. Go into the project folder.
3. Run `gulp`, the default Gulp command.

There should now be a new `dist` directory in the project, with `css` and `js` subdirectories. Those subdirectories should now contain concatenated versions of your JavaScript and CSS files.

You can also run the `gulp watch` command. When this is running, anytime you make changes to your JS or CSS, Gulp will rerun the build and reconcatenate your files. Type `control+c` into the Terminal window to stop this from running.