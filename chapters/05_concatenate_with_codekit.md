
# Concatenate with CodeKit

In the Codekit Boilerplate, there are two directories:

1. `js` for our JavaScript files.
2. `sass` for our CSS files.

## Sass

We're using Sass to let us compile a bunch of modular files into a single file. You don't actually need to use Sass's advanced features to take advantage of this. All you need to do is change the file name extension from `.css` to `.scss` on your CSS files. This converts them to Sass files (CSS is valid Sass).

In the `components` subdirectory, the files are all prefixed with an underscore. This tells our Sass compiler not to compile these files directly, since we're referencing them somewhere.

That somewhere else is in our `main.scss` file. In this file, you'll see several lines of `@import "components/filename"`. This tells our compiler to fetch the contents of that file and drop it in. You'll notice that both the underscore and the extension are missing from the filename. Sass just figures it out.

## JavaScript

In our `js` directory, there are a few files and a `components` directory. The `components` directory contains all of the modular files we want to combine into a one file. The two primary files in the `js` directory—--`detects.js` and `main.js`---are where we import those files into.

In those files, you'll see a few lines of `// @codekit-prepend "components/filename.js"` or `// @codekit-append "components/filename.js"`. These comments tell CodeKit to import the referenced file before or after the content of the file we're in. We're just using these files as placeholders for important content, but you can include code in them if you want.

## Using CodeKit

In CodeKit, go to `File` > `Add Project`, locate your project on your computer, and click `Add`. You can also just drag-and-drop the project folder into the CodeKit window.

![The CodeKit project window](img/codekit-concatenate.jpg)

There are a few ways to compile your code:

1. In the CodeKit window, right click the JavaScript file you want to compile and click `Process 'filename.js'`.
2. In the CodeKit window, right click the Sass file you want to compile and click `Compile 'filename.scss'`.
3. Any time you make a modification to one of your files, CodeKit will detect it and recompile your files.

CSS is compiled into the `css` directory, while JavaScript files are compiled into the `min` subdirectory under `js`.

![The CodeKit JavaScript settings window](img/codekit-js-settings.jpg)

By default, JavaScript is minified. You can change this by clicking on the gear icon and selecting `JavaScript` from the `Languages` dropdown. Choose `Non-minified` from the output style dropdown.