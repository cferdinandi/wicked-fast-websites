
# Social Sharing Buttons

As we learned in the lesson on concatenation, it's actually faster to load a single 300kb file than it is to load three 100kb files, even though the total file weight is the same.

One unexpected way you can run into trouble here is with social sharing buttons. The social sharing buttons provided by sites like Facebook, Twitter, and Google+ all load a bunch of additional files behind the scenes and you might not even realize it.

I did a quick test, and here's what I found:

- Twitter: 5 files
- Google+: 11 files
- Facebook: 10 files (and 15 seconds to load them all)

Not only are these social sharing buttons slowing your sites down, they're not even that effective. One study found^[[http://moovweb.com/blog/anyone-use-social-sharing-buttons-mobile/](http://moovweb.com/blog/anyone-use-social-sharing-buttons-mobile/)] that on both mobile and desktop visitors click these buttons less than one-percent of the time. On mobile, it's less than half a percent.

## Using old-fashioned HTML

If you can, I would avoid social sharing buttons altogether. But if you need to include them, use static HTML instead of script-powered buttons.

Instead of this:

```html
<a href="https://twitter.com/share" class="twitter-share-button" data-via="ChrisFerdinandi">Tweet</a>

<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>
```

Do this:

```html
<a target="_blank" href="https://twitter.com/intent/tweet?text=YOUR-TITLE&url=YOUR-URL&via=TWITTER-HANDLE">
	Tweet
</a>
```

This simple link opens in a new window, and the query string provides information to the site about the type of information you're trying to share.

If you're interested in this approach, my Social Sharing project on GitHub^[[https://github.com/cferdinandi/social-sharing](https://github.com/cferdinandi/social-sharing)] documents the markup you need to create sharing links for many of today's most popular sites, and also includes styles for lightweight branded buttons.