
# Using SVGs

JPGs, PNGs, and GIFs are raster images. They have specific pixels assigned to specific locations that render images at a specific size.

SVGs, or scalable vector graphics, use geometric shapes and mathematical relationships to render images. As a result, you can scale them up or down in size infinitely and they never lose their crispness or degrade in quality.

![](img/raster-vs-vector.jpg)

For detail-heavy graphics, like photographs, they can end up being much larger than a high-resolution JPG, but they're perfect for icons and simple images. You can also style them with CSS, which makes it easy to do things like changes colors, add hover effects, and more.

## How to use SVGs

There are many ways to use SVGs, including simply including them as the src in an `<img>` element.

```html
<img src="twitter.svg">
```

Because SVGs are really markup files, my preferred method is to copy-and-paste the contents directly into my HTML. This gives me more flexibility in styling them, and eliminates an HTTP request.

```html
<svg viewBox="0 0 320 320">...</svg>
```

You can also add classes to control styling with CSS.

```html
<svg class="icon" viewBox="0 0 320 320">
	...
</svg>
```

## Browser Support

SVG is well supported in IE 9 and up, all modern browsers, and Opera Mini.

I like to include a simple feature text inline in the `<head>` to check for browser support. This adds an `.svg` class to the `<html>` element that you can hook into for styling and fallback content.

```javascript
// SVG feature detection
var isSVG = !!document.createElementNS &&
            !!document.createElementNS('http://www.w3.org/2000/svg', 'svg').createSVGRect;

// If SVG is supported, add `.svg` class to <html> element
if ( isSVG ) {
	document.documentElement.className += ' svg';
}
```

In unsupported browsers, SVGs will display an empty content area equivalent to the size of the SVG. I use a little CSS to prevent this from happening:

```css
/**
 * Hide icons by default to prevent blank spaces
 * in unsupported browsers
 */
.icon {
	display: inline-block;
	fill: currentColor;
	height: 0;
	width: 0;
}

/**
 * Display icons when browser supports SVG.
 * Inherit height, width, and color.
 */
.svg .icon {
	height: 1em;
	width: auto;
}
```

I also include a class for fallback text that's only displayed when SVGs aren't supported and for people using assistive devices like screen readers.

```css
/**
 * Hide fallback text if browser supports SVG
 */
.svg .icon-fallback-text {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
```

In the example below, visitors on browsers that support SVG will see a Twitter icon, while unsupported browsers and screen readers will display "Follow me on Twitter" as fallback text:

```html
<svg xmlns="http://www.w3.org/2000/svg" class="icon" viewBox="0 0 32 32"><path d="M32 6.076c-1.177.522-2.443.875-3.77 1.034 1.354-.813 2.395-2.1 2.886-3.632-1.27.752-2.674 1.3-4.17 1.593C25.75 3.796 24.044 3 22.157 3c-3.627 0-6.566 2.94-6.566 6.565 0 .515.058 1.016.17 1.496-5.456-.275-10.294-2.89-13.532-6.86-.565.97-.89 2.096-.89 3.3 0 2.278 1.16 4.287 2.922 5.465-1.076-.034-2.088-.33-2.974-.82v.082c0 3.18 2.262 5.834 5.265 6.437-.55.15-1.13.23-1.73.23-.422 0-.833-.04-1.234-.118.835 2.608 3.26 4.506 6.133 4.56-2.248 1.76-5.08 2.81-8.155 2.81-.53 0-1.052-.032-1.566-.093 2.904 1.863 6.355 2.95 10.063 2.95 12.076 0 18.68-10.004 18.68-18.68 0-.285-.007-.568-.02-.85C30.006 8.55 31.12 7.394 32 6.077z"/></svg>
<span class="icon-fallback-text">Follow me on Twitter</span>
```

## The WordPress Way

The WordPress Media Uploader does not allow SVGs, and TinyMCE can strip them out, too.

WordPress SVG^[[https://github.com/cferdinandi/gmt-wordpress-svg](https://github.com/cferdinandi/gmt-wordpress-svg)] is a plugin I maintain on GitHub that enables SVG uploads in the WordPress Media Uploader. It also adds a shortcode you can use to inline your uploaded SVG files and pass in classes.

## Optimize your SVGs

SVGs often have a lot of junk and metadata in them, put there by your graphic design software. Optimizing them can reduce their overall size by 50-percent.

SVGO^[[https://github.com/svg/svgo](https://github.com/svg/svgo)] is a great cross-platform tool for optimizing your SVGs. If you're not a fan of command line, Jake Archibald created SVGOMG^[[https://jakearchibald.github.io/svgomg/](https://jakearchibald.github.io/svgomg/)], a GUI for SVGO.

## Want to learn more?

This lesson only scratched the surface of what you can do with SVGs.

If you want to learn more (and you absolutely should!), Sara Soueidan^[[https://sarasoueidan.com/](https://sarasoueidan.com/)] is hands-down the leading authority on SVG in our industry. She's constantly putting out great tutorials and pushing the boundaries of what you can do with them.