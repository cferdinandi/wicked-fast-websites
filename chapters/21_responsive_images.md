
# Responsive Images

A common technique for making images responsive is to give them a `max-width` of `100%`:

```css
img {
	max-width: 100%;
	height: auto;
}
```

But why would you send the same image to a five-year old feature phone or a tiny smart watch that you would to a modern laptop attached to 32-inch, high-definition monitor?

Ideally, you would send different images to different viewports.

## Background images

You can load viewport-aware background images using media queries. You can even use different images based on screen density^[[https://css-tricks.com/snippets/css/retina-display-media-query/](https://css-tricks.com/snippets/css/retina-display-media-query/)].

```css
.hero {
    background-image: url("/path/to/the/image-small.jpg");
}

@media (min-width: 30em) {
	  .hero {
		    background-image: url("/path/to/the/image-medium.jpg");
	  }
}

@media (min-width: 60em),
       (min-width: 30em) and (-webkit-min-device-pixel-ratio: 2),
       (min-width: 30em) and (min-resolution: 192dpi) {
	  .hero {
		    background-image: url("/path/to/the/image-large.jpg");
	  }
}
```

Most browsers will only load the image required for the current viewport size. If a visitor resizes their browser or rotates their device, and this triggers a new break point, a subsequent image download will occur, but the benefits of using smaller sizes outweigh the edge case of a potential second download.

Of course, you're not always going to use a background image.

## The `<picture>` element and `srcset`

Being able to specify which version of an image to show at which breakpoint sounds great. And in many cases, it is (think art direction across a variety of viewports).

But it also involves a lot of math and decision making on your part that really should be offloaded to the browser. Rather than figuring which image to send and when, imagine if you could just tell a browser the sizes it has to choose from and let it figure out which image works. This opens up the door for some interesting opportunities:

- Browsers could automatically figure out which image would look best given the device's screen resolution.
- Visitors could choose to prioritize performance over aesthetics, forgoing, for example, the high-resolution images when in low bandwidth conditions or if trying to conserve data.
- Browsers could determine that grabbing the highest required resolution for an image and downsizing on orientation change makes more sense than a second download (or vice-versa).

Today, you as the developer are making decisions that really belong in the hands of the visitor and their browser. Fortunately, a pair of solutions are in the works.

### `<picture>`

The `<picture>` element let's you serve different images based on breakpoints. We provide an image, and the media query conditions under which to use it.

```html
<picture>
    <source srcset="path/to/image-xlarge.jpg"
media="(min-width: 60em)">
    <source srcset="path/to/image-large.jpg"
media="(min-width: 40em)">
    <img srcset="path/to/image-standard.jpg">
</picture>
```

This is great, but we're still making decisions that belong with the browser and the visitor who's using it.

### `srcset`

The `srcset` attribute allows you to provide a few image choices, and share some information with the browser that helps it decide which image to choose. It can be used with a standard `<img>` element, or with the `<picture>` element.

```html
<img src="path/to/image-small.jpg"
     srcset="path/to/image-large.jpg 1024w,
             path/to/image-medium.jpg 640w,
             path/to/image-small.jpg 320w"
     sizes="(min-width: 40em) 33.3vw,
            100vw"
>
```

Here, we provide a list of images and their pixel width in the `srcset` attribute. We also use the `sizes` attribute to pass in information about our layout, telling the browser what our breakpoints are and how wide the content area is at those breakpoints.

The browser uses that information to pick the image that makes the most sense. Since it's still just an `img` tag, we also include a `src` attribute that serves a fallback for browseres that don't support `srcset`.

### Browser Support

Currently, support for srcset isn't great^[[http://caniuse.com/#feat=picture](http://caniuse.com/#feat=picture)], and support for the <picture> element is even worse^[[http://caniuse.com/#feat=picture](http://caniuse.com/#feat=picture)], but support will get better in the near future.

However, the Picturefill polyfill from Filament Group^[[http://scottjehl.github.io/picturefill/](http://scottjehl.github.io/picturefill/)] let's you use both methods today. When support improves in the future, you can simply remove the polyfill.

To learn more about these approaches and how to use them, check out "Srcset and sizes" by Eric Portis^[[https://ericportis.com/posts/2014/srcset-sizes/](https://ericportis.com/posts/2014/srcset-sizes/)].

### The markup kind of sucks

The biggest issue with these responsive image solutions is how cumbersome the markup is. We went from the simple image tag:

```html
<img src="path/to/image-small.jpg">
```

To this:

```html
<img src="path/to/image-small.jpg"
     srcset="path/to/image-large.jpg 1024w,
             path/to/image-medium.jpg 640w,
             path/to/image-small.jpg 320w"
     sizes="(min-width: 40em) 33.3vw,
            100vw"
>
```

Here's the good news: WordPress makes this insanely easy.

### The WordPress Way

WordPress creates responsive images automatically since version 4.4. Any image you add to the content editor gets converted into a responsive image using `srcset`. WordPress also loads the Picturefill polyfill.

If you're using an older version of WordPress, install the RICG Responsive Images Plugin^[[https://wordpress.org/plugins/ricg-responsive-images/](https://wordpress.org/plugins/ricg-responsive-images/)]. This plugin was forked into the WordPress core in version 4.4, and does the exact same thing that new version of WordPress do today.