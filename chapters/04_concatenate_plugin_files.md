
# Concatenate plugin files

One often repeated piece of advice I see for improving web performance is to minimize the number of plugins you use.

Plugins, conventional wisdom says, increase the load on your server and database and slow down your site. It's even in the WordPress Codex^[[https://codex.wordpress.org/WordPress_Optimization/WordPress_Performance](https://codex.wordpress.org/WordPress_Optimization/WordPress_Performance)].

It's also 100% wrong.

## Plugins aren't the problem

I run multiple WordPress sites that run 20, even 30 plugins. They all start rendering content in around 1 second (slightly more on initial load and under that on subsequent page views).

Plugins are not bad for performance. Badly written plugins are.

And in our case, one of the things we should really be concerned about is plugins that load multiple scripts and styles on the front end. This is very common.

Fortunately, one of the tools we talked about in the last chapter, MinQueue^[[https://wordpress.org/plugins/minqueue/](https://wordpress.org/plugins/minqueue/)], is an excellent tool for addressing this issue.

MinQueue hasn't been updated in a few years, but it still works perfectly and doesn't generate any errors. And it's still the best plugin I've found for this sort of thing, so I feel comfortable using and recommending it.

## Getting a baseline

Visit any page on your site, but ideally one that seems to run a bit slow or where you know plugin files might be getting loaded (for example, a store if you're using an ecommerce plugin or a calendar if you're using an events plugin).

![There are 13 different scripts being loaded on this page](img/minqueue-before.jpg)

Open up developer tools and click on the "Network" tab. Now reload the page, and watch how many CSS and JavaScript files are loaded. This is our baseline.

## How to use MinQueue

First, install and activate the plugin.

In the Dashboard, go to `Settings` > `MinQueue`, and check, "Enable the helper in the front end of the site." This provides you with a list of all the styles and scripts being loaded on any given page. Make sure you have the WordPress toolbar enabled on the front end (under `Users` > `Your Profile`) or the helper won't show up.

In a new tab, open up the page you tested for your baseline. Click `MinQueue` from the admin toolbar. A popup will appear with a list of "Enqueued Scripts" and "Enqueued Styles," color coded by whether they're loaded in the header or the footer. Under "Scripts," highlight and copy the entire list.

![The MinQueue Helper](img/minqueue-1.jpg)

Back in your MinQueue settings, click the "Enable Script Minification" radio button, and paste in the entire list you copied from the front end. It doesn't matter where the scripts load currently. MinQueue will figure it out.

![Adding files to MinQueue in the dashboard](img/minqueue-2.jpg)

Now repeat the process with the "Styles." Copy the list, check the radio button to "Enable Style Minification," and paste the list in. Scroll to the bottom of the page and click "Save."

## Checking that it's working

Go back to the page you used for a baseline. With the "Network" tab open, reload the page again.

The number of styles and scripts should have gone down, and you should see CSS and JavaScript files that start with the name `minqueue-` and have a random string after them. These are your concatenated files.

![Now there are only 3 scripts being loaded. That's a huge difference!](img/minqueue-before.jpg)

You may find that not all files were concatenated. Some plugins that do the same thing as MinQueue are a lot more aggressive about this, but every single other one I tested ended up breaking my site or plugin functionality as a result.

MinQueue is a lot smarter about how it does things, making sure that dependencies and conditional loading are all respected. This is why it's my go-to concatenation plugin.