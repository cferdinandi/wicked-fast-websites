
# Combine files

It’s actually often faster for a browser to download one 150kb file than it is to download three 50kb files, even though the total file weight is the same.

DNS lookups, HTTP headers, redirects, 404s... there's a lot of places things can go wrong. Each HTTP request adds additional load time. How much time? From a now defunct article by Google:

> Every time a client sends an HTTP request, it has to send all associated cookies that have been set for that domain and path along with it. Most users have asymmetric Internet connections: upload-to-download bandwidth ratios are commonly in the range of 1:4 to 1:20. This means that a 500-byte HTTP header request could take the equivalent time to upload as 10 KB of HTTP response data takes to download. The factor is actually even higher because HTTP request headers are sent uncompressed. In other words, for requests for small objects (say, less than 10 KB, the typical size of a compressed image), the data sent in a request header can account for the majority of the response time.

And because browsers only download two files at a time (except JavaScript files, which block all other downloads), downloads create bottlenecks in the rendering process.

This is addressed in HTTP2^[[https://www.youtube.com/watch?v=fJ0C4zN5uOQ](https://www.youtube.com/watch?v=fJ0C4zN5uOQ)], but browser support isn't broad enough that you can bank on it yet.

## Combining Files

Combining similar file types together—--a process known as concatenation—--improves web performance.

Combine your scripts into just a few logical groups. For example, an `svg.js` file to detect SVG support, and a `canvas.js` file to detect canvas support, might get grouped into a single `detects.js` file.

Similarly, `houdini.js`, `smooth-scroll.js`, and `fluidVids.js`, a set of DOM manipulation scripts, might get grouped into `main.js`.

To keep stylesheet size down, I often see people do something like this:

```css
<link rel="stylesheet" href="style.css">
<link rel="stylesheet" media="(min-width: 20em)" href="phone.css">
<link rel="stylesheet" media="(min-width: 40em)" href="tablet.css">
<link rel="stylesheet" media="(min-width: 60em)" href="desk.css">
```

Here, the `style.css` file contains base styles, with additional stylesheets containing styles specific to smaller or larger viewports.

The problem is, browsers download all of these stylesheets regardless. The reason they do this is because if a browser window is resized or a device is rotated and this triggers a different stylesheet, the browser doesn't want to have to wait for another file to download before re-rendering the content.

So... it downloads all of the files ahead of time.

A better approach is to put all of your styles in a single stylesheet and use media queries to override styles within that file.

```html
<link rel="stylesheet" href="style.css">
```

```css
.base-styles { … }
@media (min-width: 20em) { ... }
@media (min-width: 40em) { ... }
@media (min-width: 60em) { ... }
```

Don't think you can cheat by using `@import`, either. It still triggers an additional HTTP request.

## Tools and Techniques

**GUIs**

- CodeKit for Mac^[[https://incident57.com/codekit/](https://incident57.com/codekit/)]
- Prepos for Windows^[[https://prepros.io/](https://prepros.io/)]

**Command Line**

- Gulp^[[http://gulpjs.com/](http://gulpjs.com/)]
- Grunt^[[http://gruntjs.com/](http://gruntjs.com/)]

**Plugins**

- MinQueue^[[https://wordpress.org/plugins/minqueue/](https://wordpress.org/plugins/minqueue/)]

**Other**

- Manually