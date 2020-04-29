- [Image Basics](#image-basics)
  - [Image Format: Rastor and Vector](#image-format-rastor-and-vector)
  - [Image Size and Resolution](#image-size-and-resolution)
  - [Picture Element](#picture-element)
  - [Figure](#figure)
- [Responsive Image Techniques](#responsive-image-techniques)
  - [aspect ratio hack](#aspect-ratio-hack)
  - [height: auto, width, and max/min-width](#height-auto-width-and-maxmin-width)
  - [srcset](#srcset)
  - [object-fit and polyfill](#object-fit-and-polyfill)
  - [background-image if your image is not part of the page’s content](#background-image-if-your-image-is-not-part-of-the-pages-content)
  - [image grid using auto-fill and minmax](#image-grid-using-auto-fill-and-minmax)
- [Image Optimization](#image-optimization)
  - [Image Compression](#image-compression)
  - [Inlining Images](#inlining-images)
  - [Image Sprites](#image-sprites)
  - [Image Lazy Loading](#image-lazy-loading)
  - [CDNs](#cdns)
  - [Eliminate jack with aspect ratio](#eliminate-jack-with-aspect-ratio)
- [Tooling](#tooling)
  - [Terminology](#terminology)
  - [Resources](#resources)

# Image Basics

## Image Format: Rastor and Vector

Raster images, like photographs and other images, are represented as a grid of individual dots or pixels. Raster images typically come from a camera or scanner, or can be created in the browser with the canvas element. As the image size gets larger, so does the file size. When scaled larger than their original size, raster images become blurry because the browser needs to guess how to fill in the missing pixels.

Vector images, such as logos and line art, are defined by a set of curves, lines, shapes, and fill colors. Vector images are created with programs like Adobe Illustrator or Inkscape and saved to a vector format like SVG. Because vector images are built on simple primitives, they can be scaled without any loss in quality or change in file size.

- Use JPG for photographic images.
- Use SVG logos and icons.
- Use PNG for photographic images with transparency
- For longer animations consider using <video>, which provides better image quality and gives the user control over playback.
- WebP: Image format that supports lossy and lossless compression, as well as animation and transparency.
- Progressive JPEG

## Image Size and Resolution

Device Pixel Ratio -
width density descriptors - 

<img src="lighthouse-160.jpg" alt="lighthouse"
     sizes="80vw"
     srcset="lighthouse-160.jpg 160w, 
             lighthouse-320.jpg 320w,        
             lighthouse-640.jpg 640w,
             lighthouse-1280.jpg 1280w">

pixel density descriptors - Add support for high resolution displays using pixel density descriptors such as 1x, 1.5x, 2x, and 3x. The new srcset attribute applies to both <img> and <source> elements.

<img 
    src="images/kitten-curled.png" 
    srcset="images/kitten-curled@1.5x.png 1.5x,
            images/kitten-curled@2x.png 2x"
    alt="a cute kitten">

The sizes attribute in HTML is very directly related to CSS. In fact, it basically says: “This is how I intend to size this image in CSS, I’m just letting you know right now because you might need this information right this second and cannot wait for CSS to download first.”

Sample:

<img
  sizes="(min-width: 40em) 80vw, 100vw"
  srcset=" ... "
  alt="…">
Which assumes something like this in the CSS:

```css
img {
  width: 100%;
}
@media (min-width: 40em) {
  /* Probably some parent element that limits the img width */
  main {
    width: 80%;
  }
}
```

But sizes alone doesn’t do anything. You pair it with srcset, which provides known widths, so the browser can make a choice. Let’s assume just a pair of images like:

<img
  sizes="(min-width: 400px) 80vw, 100vw"
  srcset="examples/images/small.jpg 375w,
          examples/images/big.jpg 1500w"
  alt="…">
The information in the markup above gives the browser what it needs to figure out the best image for it. The browser knows 1) it’s own viewport size and 2) it’s own pixel density.

Perhaps the browser viewport is 320px wide and it’s a 1x display. It now also knows it will be displaying this image at 100vw. So it has to pick between the two images provided. It does some math.

375 (size of image #1) / 320 (pixels available to show image) = 1.17
1500 (size of image #2) / 320 (pixels available to show image) = 4.69

1.17 is closer to 1 (it’s a 1x display), so the 375w image wins. It’ll try to not go under, so 1.3 would beat 0.99, as far as I understand it.

Now say it was a 2x display. That doubles the amount of pixels needed to show the images, so the math is:

375 / 640 = 0.59
1500 / 640 = 2.34

2.34 wins here, and it’ll show the 1500w image. How about a 1x display with a 1200px viewport?

375 / (80% of 1200) = 0.39
1500 / (80% of 1200) = 1.56

The 1500w image wins here.

This is kinda weird and tricky to write out in CSS. If we just think about 1x displays, we end up with logic like…

If the viewport is less than 375px, use the 375w image.
If the viewport is larger than 375px but less than 400px, use the 1500w image (because otherwise we’d be scaling up).
At 400px, the image moves to 80vw wide, so it’s safe to use the 375w image for a littttttle bit ( (between 400px and 468px)
Over 468px, use the 1500w image.
Which we could write like:

```css
img {
  background-image: url(small.jpg);
}

/* Only override this if one of the conditions for the 1500w image is met */

@media (min-width: 375px) and (max-width: 400px), (min-width: 468px) {
  main {
    background-image: url(large.jpg);
  }
}
```

In this exact case, a 2x display, even at a really narrow width like 300px, still requires 600px make that 1.0 minimum quality, so we’d also add that to the logic:

```css
.img {
  background-image: url(small.jpg);
}
/* Only override this if one of the conditions for the 1500w image is met */

@media (min-width: 375px) and (max-width: 400px),
  (min-width: 468px),
  (-webkit-min-device-pixel-ratio: 2),
  (min-resolution: 192dpi) {
  .img {
    background-image: url(large.jpg);
  }
}
```

The complexity of this skyrockets the more breakpoints (sizes) and the more provided images. And it’s still not a perfect match for what responsive images (in HTML) can do, since it doesn’t allow for browser discretion (e.g. the potential for a browser to consider other factors [i.e. bandwidth] to choose an image).

By setting the CSS max-width property to 100%, an image will fill the width of it's parenting element, but won’t render larger than it's actual size, thus preserving resolution.

Setting the height property to auto maintains the aspect ratio of the image, using this technique allows static height to be overridden and enables the image to flex proportionally in all directions. For example:

```css
img,
embed,
object,
video {
  max-width: 100%;
  height: auto;
}
```

## Picture Element

The `<picture>` element offers a declarative approach towards image resource loading. Web developers will no longer need CSS or JavaScript hacks to handle images in responsive designs. The native implementation of the <picture> element means that you can declare your responsive images using only HTML. And users benefit from natively-optimized image resource loading—especially important for users on slower mobile internet connections.

Alongside the newer `srcset` and `sizes` attributes recently added to `<img>`, the `<picture>` element gives web developers more flexibility in specifying image resources. Write clear HTML markup and let the browser do the work of detecting any of the following scenarios, alone or in combination, to support responsive designs and improve web page load times:

**Art direction-based selection**Is this a mobile device held in a portrait orientation or a wide desktop monitor? [Load an image that is optimized for the given screen dimensions.](https://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-art-direction)
**Device-pixel-ratio-based selection**Does the device have a high DPI display? [Load a higher resolution image.](https://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-pixel-density-descriptors)**Viewport-based selection**Is the image meant to always fill a fixed proportion of the viewport? [Load images relative to the viewport.](https://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-width-descriptors)
**Image format-based selection**Can the browser support additional image file types that offer performance boosts such as smaller file sizes? [Load an alternative image file such as WebP.](https://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-file-type)

- [Picture Element Specification](https://html.spec.whatwg.org/multipage/embedded-content.html#embedded-content) 
- [https://scottjehl.github.io/picturefill/](https://scottjehl.github.io/picturefill/)
- [https://www.html5rocks.com/en/tutorials/responsive/picture-element/](https://www.html5rocks.com/en/tutorials/responsive/picture-element/)

## Figure

- [Responsive Image using autofill and minmax](https://codepen.io/Devvventure/pen/oNjWzbe)

# Responsive Image Techniques

## aspect ratio hack

- https://css-tricks.com/aspect-ratio-boxes/
- [aspect ratio hack](https://codepen.io/Devvventure/pen/bGVWpLB)

## height: auto, width, and max/min-width

## srcset

## object-fit and polyfill

## background-image if your image is not part of the page’s content

## image grid using auto-fill and minmax

- https://css-tricks.com/planning-for-responsive-images/

# Image Optimization

- Choose the right image format
- Include image optimization and compression tools into your workflow to reduce file sizes.
- Reduce the number of http requests by placing frequently used images into image sprites and inlining.
- To improve the initial page load time and reduce the initial page weight, consider loading images only after they’ve scrolled into view.
- Eliminate image jank by declaring a width and height properties on the img element and setting height: auto and width: 100% using css
- Github ImageBot

## Image Compression

Lossy vs Loseless Image Compression

## Inlining Images

Inline code for images can be verbose—especially Data URIs—so why would you want to use it? To reduce HTTP requests! SVGs and Data URIs can enable an entire web page, including images, CSS and JavaScript, to be retrieved with one single request.

On the downside:

- On mobile, Data URIs can be significantly slower to display on mobile than images from an external src.
- Data URIs can considerably increase the size of an HTML request.
- They add complexity to your markup and your workflow.
- The Data URI format is considerably bigger than binary (up to 30%) and therefore doesn't reduce total download size.
- Data URIs cannot be cached, so must be downloaded for every page they're used on.
- They're not supported in IE 6 and 7, incomplete support in IE8.
- With HTTP/2, reducing the number of asset requests will become less of a priority.

As with all things responsive, you need to test what works best. Use developer tools to measure download file size, the number of requests, and the total latency. Data URIs can sometimes be useful for raster images—for example, on a homepage that only has one or two photos that aren't used elsewhere. If you need to inline vector images, SVG is a much better option.

Related:

- JavaScript Image Replacement

## Image Sprites

- Spriting has the advantage of reducing the number of downloads required to get multiple images, while still enabling caching

## Image Lazy Loading

- LazyLoad Component: a lightweight and flexible script that speeds up your web application by deferring the loading of your below-the-fold images, videos and iframes to when they will enter the viewport. It's written in plain "vanilla" JavaScript, it leverages the IntersectionObserver API, it supports responsive images and enables native lazy loading. https://www.andreaverlicchi.eu/lazyload/ https://github.com/verlok/lazyload

## CDNs

- Cloudinary (Syntax promo code)
- Cloudflare

## Terminology

- Art Direction: To change images based on device characteristics
- Pixel Density Descriptor: the browser assumes that 2x images are twice as large as 1x images, and so scales the 2x image down by a factor of 2, so that the image appears to be the same size on the page.
- Spriting: is a technique whereby a number of images are combined into a single "sprite sheet" image. You can then use individual images by specifying the background image for an element (the sprite sheet) plus an offset to display the correct part.

## Resources

### Tooling

- RespImageLint
- PostCSS/Auto Prefixer
- https://github.com/andreruffert/progressive-image-element

### Links

- http://responsiveimages.org/
- [W3C Document](http://usecases.responsiveimages.org/)
- https://developers.google.com/web/fundamentals/design-and-ux/responsive/images
- [https://css-tricks.com/responsive-images-css/](https://css-tricks.com/responsive-images-css/)
- https://www.sarasoueidan.com/blog/svgo-tools/
- Responsive Images Community Group
- [Responsive Images: Use Cases and Documented Code Snippets to Get You Started](https://dev.opera.com/articles/responsive-images/)
- [Responsive Image Techniques](https://css-tricks.com/which-responsive-images-solution-should-you-use/)
