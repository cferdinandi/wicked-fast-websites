
# Measuring performance

When it comes to web performance, how fast is fast enough? **One second.**

One second is roughly the limit to keep a visitor's flow of thought on the task at hand uninterrupted^[[http://www.nngroup.com/articles/response-times-3-important-limits/](http://www.nngroup.com/articles/response-times-3-important-limits/)].

Sound impossible? It's not, but that's because there's a catch.

## Total load time and page weight aren’t the right metrics

Historically, developers have looked at things like total load time and page weight as the key metrics for performance. While those are important, there are two that matter a lot more:

1. **Time to First Byte**, which tells you how quickly your server is sending data back to the browser.
2. **Time to Start Render**, which is how quickly the browser begins displaying content to your visitor.

Your whole site doesn't need to load in one second for it to feel fast. It just needs to start displaying content in that amount of time.

*Perceived performance* is more important than *actual performance*.

## Tools for measuring performance

My favorite tool for measuring web performance is WebPagetest.org^[[http://www.webpagetest.org/](http://www.webpagetest.org/)]. Originally built by AOL, it was opened source in 2008 and is now maintained by Google.

WebPagetest provides a simple, web-based GUI that measures a ton of performance metrics about your site, but the two you care about the most are, of course, *Time to First Byte* and *Start Render Time*.

![A report from WebPagetest.org](img/web-page-test.jpg)

Let's use WebPagetest to get some baselines for your site. Visit WebPagetest, pop in your URL, and run it with all of the default settings. When that's done, run it again, but expand out the Advanced Settings and change the Connection to 3g. When that's done, run it a third time with an EDGE connection.

We want to get a baseline for your site on a variety of different connections. Sites can sometimes do quite well on cable but fail horribly on slower connections.

## Target baselines

### Time to First Byte

You ideally want this to be 100ms - 300ms, but if you're running on inexpensive, shared hosting, it can closer to 500ms or 600ms.

If that's that case, don't freak out. You may want to consider switching web hosts. There may also be some simple changes you can make to your WordPress setup to improve this number. We're actually going to talk about some of this in a later lesson.

### Start Render Time

- On a cable connection, you want this number to be 1,000ms (1 second) or lower.
- On a 3g connection, you want this number to be 3,000ms (3 seconds) or lower.
- On an EDGE network, you want this number to be 5,000ms (5 seconds) or lower.

If your site doesn't hit these numbers today, that's ok. We're just getting a baseline so we can measure our improvement. These are the targets we'd like to hit after we're done making all of our changes.

## Google PageSpeed Insights

Another often recommended tool is Google PageSpeed Insights^[[https://developers.google.com/speed/pagespeed/insights/](https://developers.google.com/speed/pagespeed/insights/)]. It provides a simple 0-100 ranking of how performant your site is.

![A report from Google PageSpeed Insights](img/google-pagespeed-insights.jpg)

I'm actually not a big fan of this tool for measuring performance. My site today is faster than it's ever been. It has a start render time of just over a second on initial load, and under a second on subsequent visits. My Google PageSpeed score is also the worst it's ever been.

Google PageSpeed Insights measures indicators of performance rather than actual performance. That makes a great tool for identifying potential bottlenecks, but not a great way to measure your actual performance.

## What does my site cost?

Another really interesting tool for measuring performance is What does my site cost?^[[https://whatdoesmysitecost.com/](https://whatdoesmysitecost.com/)].

![A report from "What does my site cost?"](img/what-does-my-site-cost.jpg)

Built by Tim Kadlec^[[http://timkadlec.com/](http://timkadlec.com/)], it mashes up data about your site from WebPageTest.org with average data costs around the globe^[[http://www.itu.int/en/Pages/default.aspx](http://www.itu.int/en/Pages/default.aspx)] and financial metrics from the World Bank^[[http://data.worldbank.org/](http://data.worldbank.org/)] to provide you with a few different views of what your site costs to visitors around the world.

When lobbying for performance, this is nice, tangible metric to share with clients.