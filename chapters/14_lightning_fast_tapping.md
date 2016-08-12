
# Lightning Fast Tapping

Many mobile browsers introduce a 300ms delay when a user taps on a link or button. From Google:^[[https://developers.google.com/mobile/articles/fast_buttons](https://developers.google.com/mobile/articles/fast_buttons)]

> Mobile browsers will wait approximately 300ms from the time that you tap the button to fire the click event. The reason for this is that the browser is waiting to see if you are actually performing a double tap.

This is why web apps feel *so* much slower than native apps. Don't believe me? Check out this video from Jake Archibald.^[[https://www.youtube.com/watch?v=AjUpiwvIa5A](https://www.youtube.com/watch?v=AjUpiwvIa5A)]

The delay made a lot of sense when smartphones were first introduced and most websites weren't mobile-friendly. When viewing a desktop-oriented site on a small screen, double-tapping was a fast and easy way to zoom in and out of content areas.

While there are still far too many sites today that aren't mobile-friendly, many are, and waiting for a double tap on those sites doesn't make sense.

## Set a viewport width

Setting a `<meta>` tag that defines the viewport width will remove the delay on both Chrome for Android (since version 32) and Safari on iOS (since version 9.1).

```html
<meta name="viewport" content="width=device-width">
```

Mobile Safari also provides a way to remove the delay on an element-by-element basis. Jeremy Keith from Clearleft recommends [using the follow CSS](https://adactio.com/journal/10019):

```css
/**
 * Remove the tap delay in webkit
 */
a,
button,
input,
select,
textarea,
label,
summary {
    touch-action: manipulation;
}
```