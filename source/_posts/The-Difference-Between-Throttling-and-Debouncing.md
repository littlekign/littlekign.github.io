---
title: The-Difference-Between-Throttling-and-Debouncing
date: 2019-11-17 15:42:22
tags: js
---

I got these confused the other day and someone corrected me. So I tossed it on the ol' list of blog post ideas and here we are. Both of them are ways to limit the amount of JavaScript you are executing based on DOM events for performance reasons. But they are, you guessed it, different.

## # Throttling enforces a maximum number of times a function can be called over time. As in "execute this function at most once every 100 milliseconds."

Say under normal circumstances you would call this function 1,000 times over 10 seconds. If you throttle it to only once per 100 milliseconds, it would only execute that function at most 100 times

(10s \* 1,000) = 10,000ms
10,000ms / 100ms throttling = 100 maximum calls

## # Debouncing enforces that a function not be called again until a certain amount of time has passed without it being called. As in "execute this function only if 100 milliseconds have passed without it being called."

Perhaps a function is called 1,000 times in a quick burst, dispersed over 3 seconds, then stops being called. If you have debounced it at 100 milliseconds, the function will only fire once, at 3.1 seconds, once the burst is over. Each time the function is called during the burst it resets the debouncing timer.

## # What's the point?

One major use case for these concepts is certain DOM events, like scrolling and resizing. For instance, if you attach a scroll handler to an element, and scroll that element down say 5000px, you're likely to see 100+ events be fired. If your event handler does a bunch of work (like heavy calculations and other DOM manipulation), you may see performance issues (jank). If you can get away with executing that handler less times, without much interruption in experience, it's probably worth it.

Quick hit examples:

- Wait until the user stops resizing the window
- Don't fire an ajax event until the user stops typing
- Measure the scroll position of the page and respond at most every 50ms
- Ensure good performance as you drag elements around in an app

## # How to do it

Functions for both are built into [Underscore](http://underscorejs.org/) and [Lodash](https://lodash.com/). Even if you don't use those libraries wholesale, you could always go extract the functions out of them for your own use.

Simple throttled scroll:

```javascript
Jquery;
$("body").on(
  "scroll",
  _.throttle(function() {
    // Do expensive things
  }, 100)
);
```

Simple debounced resize:

```javascript
Jquery;
$(window).on(
  "resize",
  _.debounce(function() {
    // Do expensive things
  }, 100)
);
```

Leveling up from here, you would work in the use of requestAnimationFrame, so even when the functions are executed the browser does it on it's own ideal timing. That's covered in [this Paul Lewis tutorial.](http://www.html5rocks.com/en/tutorials/speed/animations/)

## Demo

Simple demo so you can experience the difference:

<p class="codepen" data-height="865" data-theme-id="default" data-default-tab="js,result" data-user="chriscoyier" data-slug-hash="vOZNQV" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="The Difference Between Throttling, Debouncing, and Neither">
  <span>See the Pen <a href="https://codepen.io/chriscoyier/pen/vOZNQV">
  The Difference Between Throttling, Debouncing, and Neither</a> by Chris Coyier  (<a href="https://codepen.io/chriscoyier">@chriscoyier</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
