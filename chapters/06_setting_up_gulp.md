
# Setting up Gulp

***Note:*** *This is intended for people who are completely new to Gulp or command line. If that's not you, feel free to skip to the next chapter.*

First, we need to install a few files.

1. NodeJS^[[https://nodejs.org](https://nodejs.org)] - NodeJS is what powers Gulp. The site will autodetect your operating system. Download and run the installer.
2. Gulp^[[http://gulpjs.com/](http://gulpjs.com/)] - Click "Docs" and find "Getting Started." The first step, "Install gulp globally," is the one we want. Copy-and-paste the command there into Terminal without the leading `$`: `npm install --global gulp-cli`. Then click `return` on your keyboard.

## Installing dependencies

Once that's done, we're good to go. Now, we want to navigate into our project folder in Terminal. This really tripped me up when I first started working with command line, but there's a simple trick that makes it really easy.

1. Type `cd ` for "change directory" (don't forget the space afterwards).
2. Locate your project folder, and drag-and-drop it into the Terminal window. This automatically adds the file path for you.
3. Click `return` on your keyboard.

You're now in your project folder. Now we want to install of the dependencies we need to run our Gulp build.

1. Type `npm install`.
2. Click `return` on your keyboard.

***Note:*** *You don't need to run `npm install`. I've included these files for you, but wanted you to know how to do it in case you want to get more involved in Gulp.*

There's now a `node_modules` directory in our project. This is where all of the packages and scripts that run our Gulp build live.

Now we can type `gulp` in Terminal and hit `return`, and our Gulp build will run.

## Under the hood

There are two files that power our Gulp build.

1. `package.json`
2. `gulpfile.js`

### `package.json`

`package.json` contains all of the information about our project---the name, version number, description, and so on. It also contains a list of our dependencies and the version number we should use. When we run `npm install`, it grabs all of the packages from this list.

### `gulpfile.js`

`gulpfile.js` is where we tell Gulp exactly what to do when it runs.

At the top are a bunch of variables where we `require` packages. This is where we identify all of the packages we'll be using to do various things in our Gulp tasks.

I've also created a `paths` variable. This is where I specify where files are located and where they should go after they're compiled and processed.

The `banner` variable is where we create dynamically generated headers to get added to the top of our compiled files. This pulls information about the project from our `package.json` file, so it can be easily reused across projects.

The file also includes various tasks for building our scripts and styles, deleting our `dist` folder on each run so that we start clean, and listening for changes when we run `gulp watch`.

At the very end of our file are the task runners. This is where we pull together all of the build tasks into simple commands that can be run in Terminal.