
# Why web performance matters

I'm going to assume that if you're reading this book, you already care about web performance. But you may find yourself in a situation where you need to convince a client or a manager that this is worth spending time and money on.

With that in mind, let's take a minute to look at some data that explains why web performance matters.

## Fractions of a second

A few year’s ago, Google ran an interesting experiment^[[https://blog.kissmetrics.com/speed-is-a-killer/](https://blog.kissmetrics.com/speed-is-a-killer/)]. Based on analyst research, they increased the number of search results displayed on a single page from 10 to 30. This added about 500ms—that’s just half a second—of extra load time to the page.

The result? Search traffic, and subsequently ad revenues, decreased by 20-percent.

Amazon ran a similar experiment, and found that delays of as little as one-tenth of a second could reduce revenue by 1-percent^[[http://www.slideshare.net/stubbornella/designing-fast-websites-presentation](http://www.slideshare.net/stubbornella/designing-fast-websites-presentation)]. One-percent may not sound like a big deal, but for a company with Amazon's volume, it's huge. One-percent of their Q3 2015 sales is $254 million.

Firefox reduced their page load time by 2.2 seconds and saw downloads increase by 15.4-percent^[[https://blog.mozilla.org/metrics/2010/04/05/firefox-page-load-speed-%E2%80%93-part-ii/](https://blog.mozilla.org/metrics/2010/04/05/firefox-page-load-speed-%E2%80%93-part-ii/)].

## Sites are getting bigger

In 2010, the average webpage was 600kb in size. At the beginning of 2013, that number had doubled to 1.2mb. At the beginning of 2016, the average website had ballooned to 2.2mb in size[[http://httparchive.org/interesting.php](http://httparchive.org/interesting.php)]. The average website has doubled in size every three years for the last six years.

For a while, this exponential growth wasn't a problem. We took for granted that both computers and bandwidth got faster and more reliable every year.

## And then mobile happened

People are accessing the web from devices with varying levels of computing power and bandwidth. Desktops and laptops, phones and tablets, TVs, watches, video game consoles.

And for a growing number of people, mobile isn’t just one way they access the web—it’s the way they access the web.

More than half of all Google searches^[[http://searchengineland.com/its-official-google-says-more-searches-now-on-mobile-than-on-desktop-220369](http://searchengineland.com/its-official-google-says-more-searches-now-on-mobile-than-on-desktop-220369)] happen on a mobile device. Sixty-five percent of emails are opened on a smartphone or tablet^[[http://venturebeat.com/2014/01/22/65-of-all-email-gets-opened-first-on-a-mobile-device-and-thats-great-news-for-marketers/](http://venturebeat.com/2014/01/22/65-of-all-email-gets-opened-first-on-a-mobile-device-and-thats-great-news-for-marketers/)]. More than half of all Facebook users visit the site exclusively on their mobile device^[[http://venturebeat.com/2016/01/27/over-half-of-facebook-users-access-the-service-only-on-mobile/](http://venturebeat.com/2016/01/27/over-half-of-facebook-users-access-the-service-only-on-mobile/)].

Harvard Business Review reported^[[http://blogs.hbr.org/cs/2013/05/the_rise_of_the_mobile-only_us.html](http://blogs.hbr.org/cs/2013/05/the_rise_of_the_mobile-only_us.html)] from 2012 reported that almost a third of all Americans used a mobile device as the primary way they access the internet.

## Bandwidth varies wildly

I’ve heard people argue that it doesn’t matter. 4G LTE, they say, is as fast as broadband wifi. And they’re right. LTE is awesome!

Unless you live in a city with poor coverage, or a tall building blocks your cell signal. Or there’s a bad storm that disrupts cell signal and knocks you down to a 3g connection.

And that’s in North America, which has a good mobile infrastructure. In remote and developing nations, an EDGE network can be the norm.

## Greater Performance Expectations

Amazingly, despite the varied power and bandwidth of mobile devices, users expect websites to be faster on mobile than on desktops.

Forty-percent of visitors abandon a website that takes more than 3-seconds to load^[[https://blog.kissmetrics.com/loading-time/). After five seconds, [that number skyrockets to 74-percent](http://bradfrost.com/blog/post/performance-as-design/](https://blog.kissmetrics.com/loading-time/). After five seconds, [that number skyrockets to 74-percent](http://bradfrost.com/blog/post/performance-as-design/)].

Mobile performance is so important that Google factors it into their page rankings. Slower sites rank lower than fast ones.

## We’re in the middle of a perfect storm

1. Websites are larger.
2. Devices are more varied and less predictable.
3. Performance expectations are higher than ever.

Fast websites make more money. The rest of this book will be focused on what you can do to build WordPress sites that load insanely fast.