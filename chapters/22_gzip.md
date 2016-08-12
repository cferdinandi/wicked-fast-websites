
# Gzip

Your server can actually compress your website files—--a process known as gzipping—--before sending them to the browser. This results in about a 70-percent reduction in website size^[[https://developer.yahoo.com/performance/rules.html](https://developer.yahoo.com/performance/rules.html)].

Gzipping requires a server configuration. Some web hosts do this automatically, while others require you to activate it.

You can test your site to see if it's being gzipped using gzipWTF^[[http://gzipwtf.com/](http://gzipwtf.com/)]. It will tell you which of your files are being gzipped and which ones are not.

***Note:*** *Third-party scripts and styles may not be gzipped, and unforunately, there's not much you can do to change that.*

## Enabling gzip

If your site isn't already enabled, you can activate it in your `.htaccess` file.

This is located in the root directory of WordPress, the same place where your `wp-config.php` file is. If you're on a Mac, this file might not be visible. Files that start with a `.` are hidden, so we need to display hidden files to see it.

### Displaying hidden files on a Mac

Open up your Terminal app, paste in the following code, and hit return:

```
defaults write com.apple.finder AppleShowAllFiles YES
```

Next, hold down the Option key and right click your Finder icon in the dock. The click `Relaunch`. Now your hidden files should be visible.

If at some point in the future you no longer want to see hidden files, repeat this process, but use this code in Terminal instead:

```
defaults write com.apple.finder AppleShowAllFiles NO
```

### Modifying `.htaccess`

Your `.htaccess` file will contain some WordPress-specific stuff near the top:

```
# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /wordpress/
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /wordpress/index.php [L]
</IfModule>

# END WordPress
```

We'll be adding code after the `#END WordPress` comment.

Pull up the `.htacess` file from HTML5 Boilerplate^[[https://github.com/h5bp/html5-boilerplate/blob/master/dist/.htaccess](https://github.com/h5bp/html5-boilerplate/blob/master/dist/.htaccess)] and do a search for `Compression`. You want to copy-and-paste in everything between the opening and closing `<IfModule mod_deflate.c>` tags.

Different servers are configured differently. This contains a set of checks and commands to enable gzip based on different configurations.

Once you're done, retest your site in gzipWTF to check that it's working. All of your files should now be gzipped.