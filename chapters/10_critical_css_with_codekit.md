
# Critical CSS with CodeKit

In our project folder, the `css` directory contains our `main.css` file. This file is created from the `main.scss` file in `src` > `sass`. This file tells Sass to import files from the `components` subdirectory.

Make a copy of `main.scss` and rename is `critical.scss`.

This approach requires us to know what's in all of our stylesheet components and which ones affect elements that are rendered above-the-fold. As long as you know that, this is relatively easy to do.

In my case, I only need:

- Normalize
- Grid
- Typography
- Overrides

I can delete everything else and save the file. CodeKit will run and compile a new `critical.css` file for me that's about half the size of my `main.css` file.