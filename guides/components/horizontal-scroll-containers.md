# Creating horizontal scrolling containers the right way [CSS Grid]

## **UX considerations**

I will not go into a deep discussion about the UX aspects of horizontal scrolling; however, when resorting to a horizontal scrolling layout, it seems that there are at least two UX principles which must be fulfilled:

1. [Your design must have a visual hint that a set of content is horizontally scrollable. The best way to do it is, letting a part of the scrollable content peek out](https://uxplanet.org/horizontal-scrolling-in-mobile-643c81901af3#6133).
2. [It’s important for the user to know when the scroll ends. We have noticed users repeat the scroll operation because they think that they didn’t scroll enough in the previous attempt. A way of indicating the end of the list is use of extra space at the end](https://uxplanet.org/horizontal-scrolling-in-mobile-643c81901af3#d9a0).

![https://www.evernote.com/shard/s726/res/7cd6128b-417f-445f-a954-95a50f5d148a/1%2AWusJPjQDFBu9TPD9Oolv5g.png](https://www.evernote.com/shard/s726/res/7cd6128b-417f-445f-a954-95a50f5d148a/1%2AWusJPjQDFBu9TPD9Oolv5g.png)

![https://www.evernote.com/shard/s726/res/1c19f58d-bfca-42f8-b3f5-db52f970f314/1%2AWusJPjQDFBu9TPD9Oolv5g.png](https://www.evernote.com/shard/s726/res/1c19f58d-bfca-42f8-b3f5-db52f970f314/1%2AWusJPjQDFBu9TPD9Oolv5g.png)

Example of horizontal scrolling with extra space at the end (screenshots from Myntra app)

# **Outlining the layout**

Before we begin, let us outline the layout features we want to accomplish:

- The scrolling container must follow the overall layout of the page — i.e., respecting the margins and padding
- Part of the scrollable content must peek out from the edge
- The content of the container must slide off the edges of the screen when scrolling
- The gutter between the content must be smaller than that of the edges, so that there will be more space at either end of the container (indicating to the user that they have scrolled to the end)

So something along the lines of this:

![https://www.evernote.com/shard/s726/res/d59e928d-99d5-4481-9cd6-bbd3eaaa0590/1%2AowfJDGpP-n707h-FZuDDKg.gif](https://www.evernote.com/shard/s726/res/d59e928d-99d5-4481-9cd6-bbd3eaaa0590/1%2AowfJDGpP-n707h-FZuDDKg.gif)

![https://www.evernote.com/shard/s726/res/2d4cf88f-fa42-4a0c-b410-3de726cdbb67/1%2AowfJDGpP-n707h-FZuDDKg.gif](https://www.evernote.com/shard/s726/res/2d4cf88f-fa42-4a0c-b410-3de726cdbb67/1%2AowfJDGpP-n707h-FZuDDKg.gif)

Dribbble Shot from Nakul Dhaka — [https://dribbble.com/shots/4901594-Horizontal-Vertical-Scroll](https://dribbble.com/shots/4901594-Horizontal-Vertical-Scroll)

Notice that there is an equal amount of space at either end of the horizontal scrolling container matching the surrounding content width.

# **Overall layout**

Now that we have a fundamental understanding of the features we want our horizontal scrolling container to have, let us look into how we might come about coding it using CSS Grid. The convenient thing about CSS Grid is that we can seamlessly control the gutter between the elements without further calculations.

For the overall layout we’ll use a simple yet powerful CSS Grid technique:

```
.app {
  display: grid;
  grid-template-columns: 20px 1fr 20px;
}.app > * {
  grid-column: 2 / -2;
}.app > .full {
  grid-column: 1 / -1;
}
```

Any direct children of **.app** will be ‘containerized’ with a 20px gap on both ends keeping the content off the edges. If a child is equipped with a class of **.full**, it will span across the entire viewport without any padding on the side (aka. full bleed).

# **The scrolling container**

Let us create the horizontal scrolling container with six cards, showing two at a time. As we want the horizontal scrolling container to follow the overall layout with padding on both sides, we omit the **.full** class and might try something like this:

```
.hs {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: repeat(6, calc(50% - 40px));
  grid-template-rows: minmax(150px, 1fr);
}
```

Using **grid-template-columns** we can set up how much space we want each card should take up — in this example, the cards take up 50% of the viewport. When subtracting the gutter, we end up seeing the third card peeking out at the end.

[https://www.evernote.com/shard/s726/res/b8f64dfd-59d7-470f-b485-1265aeeb9796/display](https://www.evernote.com/shard/s726/res/b8f64dfd-59d7-470f-b485-1265aeeb9796/display)

This embedded content is from a site that does not comply with the Do Not Track (DNT) setting now enabled on your browser.

Please note, if you click through and view it anyway, you may be tracked by the website hosting the embed.

**[Learn More about Medium's DNT policy](https://medium.com/policy/how-we-handle-do-not-track-requests-on-medium-f2b4b4fb7c5e)**

**[SHOW EMBED](https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fcodepen.io%2Fdannievinther%2Fembed%2Fpreview%2FXPwyax%3Fheight%3D600%26slug-hash%3DXPwyax%26default-tabs%3Dcss%2Cresult%26host%3Dhttps%3A%2F%2Fcodepen.io%26embed-version%3D2&url=https%3A%2F%2Fcodepen.io%2Fdannievinther%2Fpen%2Ff6e2e4473194621c808ded07e346d4d1%3Feditors%3D0100&image=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fi.cdpn.io%2F3794.XPwyax.small.3fff8a55-4b2b-4bad-a794-ba7ed20513c6.png&key=a19fcc184b9711e1b4764040d3dc5c07&type=text%2Fhtml&schema=codepen#)**

Note that I use CSS variables for the gutter in this Codepen

However — as you might have noticed — the cards are cut off at both ends. Remember, we want the scrollable content to slide of the edges of the screen when we scroll.

![https://www.evernote.com/shard/s726/res/e9870ace-9599-4ccd-b10c-b58d637f1bcf/1%2AodN6Oz6trsM2Ek5cpYcs9A.jpeg](https://www.evernote.com/shard/s726/res/e9870ace-9599-4ccd-b10c-b58d637f1bcf/1%2AodN6Oz6trsM2Ek5cpYcs9A.jpeg)

![https://www.evernote.com/shard/s726/res/404b03be-8387-4022-9b3f-662114aa7a68/1%2AodN6Oz6trsM2Ek5cpYcs9A.jpeg](https://www.evernote.com/shard/s726/res/404b03be-8387-4022-9b3f-662114aa7a68/1%2AodN6Oz6trsM2Ek5cpYcs9A.jpeg)

So let’s add a class of **.full** to the container and compensate for the lack of padding:

```
.hs {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: repeat(6, calc(50% - 40px));
  grid-template-rows: minmax(150px, 1fr);
  padding: 0 20px;
}
```

[https://www.evernote.com/shard/s726/res/25829e30-9267-4553-a5cf-32d7ebdadd9f/display](https://www.evernote.com/shard/s726/res/25829e30-9267-4553-a5cf-32d7ebdadd9f/display)

This embedded content is from a site that does not comply with the Do Not Track (DNT) setting now enabled on your browser.

Please note, if you click through and view it anyway, you may be tracked by the website hosting the embed.

**[Learn More about Medium's DNT policy](https://medium.com/policy/how-we-handle-do-not-track-requests-on-medium-f2b4b4fb7c5e)**

**[SHOW EMBED](https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fcodepen.io%2Fdannievinther%2Fembed%2Fpreview%2FzJQXBO%3Fheight%3D600%26slug-hash%3DzJQXBO%26default-tabs%3Dcss%2Cresult%26host%3Dhttps%3A%2F%2Fcodepen.io%26embed-version%3D2&url=https%3A%2F%2Fcodepen.io%2Fdannievinther%2Fpen%2F57a435e650ba4a5b7c68adb1abff96a2&image=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fm.cdpn.io%2Fscreenshot-coming-soon-small.png&key=a19fcc184b9711e1b4764040d3dc5c07&type=text%2Fhtml&schema=codepen#)**

Note that I use CSS variables for the gutter in this Codepen

At first glance, it seems that we’ve achieved the desired result, but once you scroll to the end, you will notice that there isn’t any space — thus not respecting the overall layout.

![https://www.evernote.com/shard/s726/res/5d2e037c-6d7f-401e-8565-1ece841c5075/1%2A9BCD4xEqjvJOU8gBHdTiCw.jpeg](https://www.evernote.com/shard/s726/res/5d2e037c-6d7f-401e-8565-1ece841c5075/1%2A9BCD4xEqjvJOU8gBHdTiCw.jpeg)

![https://www.evernote.com/shard/s726/res/853d9953-9dd4-49af-a029-0b853571957d/1%2A9BCD4xEqjvJOU8gBHdTiCw.jpeg](https://www.evernote.com/shard/s726/res/853d9953-9dd4-49af-a029-0b853571957d/1%2A9BCD4xEqjvJOU8gBHdTiCw.jpeg)

You might want to deal with it by adding a margin-right to the last element like so:

```
.hs > li:last-child {
  margin-right: 20px;
}
```

Unfortunately, this doesn’t work either. So how might we solve it?

# **Suggested solution**

Let’s consider what we have, once we remove the padding of the container:

```
.hs {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: repeat(6, calc(50% - 40px));
  grid-template-rows: minmax(150px, 1fr);
}
```

If we add some empty spaces to either side of the grid-template-columns acting as padding, we should be able to achieve our desired layout.

Let’s add 2 x 10px empty spaces to the grid-columns at both ends. Combined with the grid-gap value of 10px, we have 20px in total, thus following the padding of the overall layout.

```
.hs {
  display: grid;
  grid-gap: 10px;
  grid-template-columns:
    10px
    repeat(6, calc(50% - 40px))
    10px;
  grid-template-rows: minmax(150px, 1fr);
}
```

In order to not having the first card take up the space of the first column of 10px, we bring in empty pseudo elements at each end like so:

```
.hs::before,
.hs::after {
  content: ‘’;
}
```

The ::before and ::after elements fits perfectly in the grid-columns, as there are automatically added to the start and the end of the horizontal scrolling container. Thankfully, pseudo elements participate in the grid.

Now we are fulfilling all of the layout features we outlined at the beginning:

[https://www.evernote.com/shard/s726/res/3a18b192-3bd9-4061-9292-56aeebc49fb1/display](https://www.evernote.com/shard/s726/res/3a18b192-3bd9-4061-9292-56aeebc49fb1/display)

This embedded content is from a site that does not comply with the Do Not Track (DNT) setting now enabled on your browser.

Please note, if you click through and view it anyway, you may be tracked by the website hosting the embed.

**[Learn More about Medium's DNT policy](https://medium.com/policy/how-we-handle-do-not-track-requests-on-medium-f2b4b4fb7c5e)**

**[SHOW EMBED](https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fcodepen.io%2Fdannievinther%2Fembed%2Fpreview%2FLJovrx%3Fheight%3D600%26slug-hash%3DLJovrx%26default-tabs%3Dcss%2Cresult%26host%3Dhttps%3A%2F%2Fcodepen.io%26embed-version%3D2&url=https%3A%2F%2Fcodepen.io%2Fdannievinther%2Fpen%2F82398fb3e5f4e01e64f08164737a8291&image=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fi.cdpn.io%2F3794.LJovrx.small.32076231-57f1-4520-9200-72dd0a432284.png&key=a19fcc184b9711e1b4764040d3dc5c07&type=text%2Fhtml&schema=codepen#)**

# **Caveats**

One caveat of this technique is the fixed number of cards you have to specify in the **grid-template-columns**:

```
grid-template-columns:
  10px
  repeat(6, calc(50% - 40px))
  10px;
```

If one of the containers only contains 4 cards, you will need to set up a new grid rule for that particular container. And that’s not very flexible.

One way of making it more flexible is by counting how many cards there are in the specific container using Javascript and then assigning this number to a CSS Variable:

```
var root = document.documentElement;
const lists = document.querySelectorAll('.hs');lists.forEach(el => {
  const listItems = el.querySelectorAll('li');
  const n = el.children.length;
  el.style.setProperty('--total', n);
});
```

Then you can use the variable inside the grid-template-columns:

```
grid-template-columns:
  10px
  repeat(var(--total), calc(50% - 40px))
  10px;
```

[https://www.evernote.com/shard/s726/res/73d31596-d986-4aa0-aa22-f98f0fb03e08/display](https://www.evernote.com/shard/s726/res/73d31596-d986-4aa0-aa22-f98f0fb03e08/display)

This embedded content is from a site that does not comply with the Do Not Track (DNT) setting now enabled on your browser.

Please note, if you click through and view it anyway, you may be tracked by the website hosting the embed.

**[Learn More about Medium's DNT policy](https://medium.com/policy/how-we-handle-do-not-track-requests-on-medium-f2b4b4fb7c5e)**

**[SHOW EMBED](https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fcodepen.io%2Fdannievinther%2Fembed%2Fpreview%2FPdvvQv%3Fheight%3D600%26slug-hash%3DPdvvQv%26default-tabs%3Dcss%2Cresult%26host%3Dhttps%3A%2F%2Fcodepen.io%26embed-version%3D2&url=https%3A%2F%2Fcodepen.io%2Fdannievinther%2Fpen%2F54d1f32aace4dc6326be83eddfb14ff9&image=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fi.cdpn.io%2F3794.PdvvQv.small.7fe24b6e-1ae1-4eb9-b1b0-cd7f0bee0dd4.png&key=a19fcc184b9711e1b4764040d3dc5c07&type=text%2Fhtml&schema=codepen#)**

**UPDATE:** As [Alex Baciu](https://uxdesign.cc/u/74861ae4a85f?source=post_page---------------------------) mentions in the comments, you could omit javascript (or a CSS variables solution) entirely by taking advantage of the implicit grid. This way, we don’t need to calculate the number of overflowing columns we need, as this is computed for us by the browser.

For this to work, we will need to set up our code a bit differently:

```
.hs {
  ...
  grid-template-columns: 10px;
  grid-auto-flow: column;
  grid-auto-columns: calc(50% - var(--gutter) * 2);
  ...
....hs:before,
.hs:after {
  content: '';
  width: 10px;
}
```

We still need our initial 10px to compensate for the padding; however, the rest of the cards are now being laid out by the auto-placement algorithm. In order for this to work though, we need to set auto-flow to ‘column’ (the default is ‘row’).

Finally, we need to make sure, that the .hs:after — which inherit the size of the other cards — doesn’t take up more space than 10 pixels. So we limit the size of the pseudo elements by applying a fixed width.

[https://www.evernote.com/shard/s726/res/25829e30-9267-4553-a5cf-32d7ebdadd9f/display](https://www.evernote.com/shard/s726/res/25829e30-9267-4553-a5cf-32d7ebdadd9f/display)

This embedded content is from a site that does not comply with the Do Not Track (DNT) setting now enabled on your browser.

Please note, if you click through and view it anyway, you may be tracked by the website hosting the embed.

**[Learn More about Medium's DNT policy](https://medium.com/policy/how-we-handle-do-not-track-requests-on-medium-f2b4b4fb7c5e)**

**[SHOW EMBED](https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fcodepen.io%2Fdannievinther%2Fembed%2Fpreview%2FvVydZJ%3Fheight%3D600%26slug-hash%3DvVydZJ%26default-tabs%3Dcss%2Cresult%26host%3Dhttps%3A%2F%2Fcodepen.io&url=https%3A%2F%2Fcodepen.io%2Fdannievinther%2Fpen%2FvVydZJ%2F&image=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fm.cdpn.io%2Fscreenshot-coming-soon-small.png&key=a19fcc184b9711e1b4764040d3dc5c07&type=text%2Fhtml&schema=codepen#)**

You could argue, that the code becomes less legible, as the values are scattered a bit more making it somewhat less obvious what is going on. However, I guess that is fine :)
