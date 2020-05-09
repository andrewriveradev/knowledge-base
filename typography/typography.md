topics:

- typography and geometric progression [https://type-scale.com/](https://type-scale.com/) [https://www.modularscale.com/](https://www.modularscale.com/)
- do something with this: Darker color variations are made by lowering brightness and increasing saturation. Brighter color variations are made by increasing brightness and lowering saturation.
- serif vs san serif
- rule: use one or two fonts but not more. Learn Font pairing

- More spacing than you think you need
- Vertical rhythm
- Letter spacing: -1px
- Consistent margin and padding
- line length 50-60 characters
- what is text lockup? https://css-tricks.com/snippets/svg/text-lock-up/

# **Importance is not by font size, but by font weight**

# “You can say, “I love you,” in Helvetica. And you can say it with Helvetica Extra Light if you want to be really fancy. Or you can say it with the Extra Bold if it’s really intensive and passionate, you know, and it might work.” — Massimo Vignelli

As what Vignelli has said, it is by font weight that we can give emphasis and importance to words. Let’s take a look at the difference of the two examples below:

![https://miro.medium.com/max/60/1*FpW7hrS3PMfQCYScOMXD8w.png?q=20](https://miro.medium.com/max/60/1*FpW7hrS3PMfQCYScOMXD8w.png?q=20)

![https://miro.medium.com/max/1372/1*FpW7hrS3PMfQCYScOMXD8w.png](https://miro.medium.com/max/1372/1*FpW7hrS3PMfQCYScOMXD8w.png)

Font used: Helvetica

From the image above, I used **_32px_** for *I love you* to show that it’s higher hierarchy than *You are my life, my everything,* which is **_23px._** But giving an emphasis to *I love you* doesn’t stop there. It still lacks the contrast we’re looking for in order to set it apart from the subtitle. And so, **font weight** comes to the rescue.

# **_Bonus Tip:_**

![https://miro.medium.com/max/60/1*Q8UF2mYDKNRf_4RjFpfEbw.png?q=20](https://miro.medium.com/max/60/1*Q8UF2mYDKNRf_4RjFpfEbw.png?q=20)

![https://miro.medium.com/max/1138/1*Q8UF2mYDKNRf_4RjFpfEbw.png](https://miro.medium.com/max/1138/1*Q8UF2mYDKNRf_4RjFpfEbw.png)

Using the same format we had before, in Design A, I used 3 different font sizes: **_32px_** (Title), **_23px_** (description), **_16px_** (published) while in Design B only 2 fonts sizes: **_32px_** (Title) and **_23px_** (description + published).

# Contrast formula = size + weight + color

In handling contrast, I go by the formula **_size+weight+color_** than size+size+size/color. In Design B, instead of adding a new font size, I opted to keep my *description’s size* and utilize it in *Published by* and added a touch of color to achieve contrast.

# The goal is not to have various font sizes and few font weights, but fewer font sizes with various weights

# Contrast

Font hierarchy is just not about small to big font sizes. It is about the **right** **_mix of size, weights, and colors that creates contrast. Bigger contrast, the better._**

![https://miro.medium.com/max/2048/1*yqVwmecQQmn6fB6yRjv94A.jpeg](https://miro.medium.com/max/2048/1*yqVwmecQQmn6fB6yRjv94A.jpeg)

Example 2

![https://miro.medium.com/max/2295/1*KYZikUrx9F02cJU9kpn_gQ.png](https://miro.medium.com/max/2295/1*KYZikUrx9F02cJU9kpn_gQ.png)

# **So how do I create better contrast?**

- Don’t use one kind of weight with different font sizes to create contrast and hierarchy.
- Instead, use bolder and darker style for primary content, or smaller and lighter for secondary content (less important).
- Create three kinds of text colors that varies from dark to light (see example below). Usually I use my base color as body text color.
- Don’t be afraid to apply big font gaps to your items (i.e. 24px header, 16px body text, 10px meta etc). Bigger gap = better contrast.
- Check out

  [Modularscale](http://www.modularscale.com/)

  an online calculator you can use to create better font hierarchy.

- Remember, contrast is = size + weight + color.
- Lastly, make sure to check its contrast ratio. You can use this [calculator](http://leaverou.github.io/contrast-ratio/) to check its accessibility.

![https://miro.medium.com/max/564/1*J9cLqeIrNQ3j3FzQrMnxzg.png](https://miro.medium.com/max/564/1*J9cLqeIrNQ3j3FzQrMnxzg.png)

from my base color, I move from darker for headlines to lighter for ancillary content.

# Fluid typography

3 approaches, using vw units

[https://medium.com/sketch-app-sources/truly-fluid-typography-257a2b434105](https://medium.com/sketch-app-sources/truly-fluid-typography-257a2b434105)

# Another way to create Responsive Typography
It appears that by using calc() and vw we can get responsive typography that scales perfectly between specific pixel values within a specific viewport range.

The problem with the common approach to responsive typography is that it is jumpy and requires a lot of media queries.

Viewport units are fluid but lack precise control over font-size.

Typically you might use a table like this to work out the range of font sizes across different resolutions.

Viewport units:	1vw	2vw	3vw	4vw	5vw
Viewport size	font-size in pixels
400px	4px	8px	12px	16px	20px
500px	5px	10px	15px	20px	25px
600px	6px	12px	18px	24px	30px
700px	7px	14px	21px	28px	35px
800px	8px	16px	24px	32px	40px
900px	9px	18px	27px	36px	45px
1000px	10px	20px	30px	40px	50px
1100px	11px	22px	33px	44px	55px
1200px	12px	24px	36px	48px	60px
Looking at the table you can see there are many limitations. There is no way to scale between 16px and 36px for example over the given viewport sizes. That is a shame because this is the type of control designers expect (and should expect).

Imagine you want the smallest font-size to be 12 pixels and then once the device width is greater than 400px you want the font-size to gradually increase to 24px and stop scaling by the time the viewport reaches 800px. That is exactly what this demo does!

This is achieved by using viewport units in combination with calc().

More details here: https://madebymike.com.au/writing/precise-control-responsive-typography/

Get the SCSS mixin: https://codepen.io/MadeByMike/pen/vNrvdZ

See all examples in the collection: https://codepen.io/collection/nLbRMZ

Read about it on Smashing: https://www.smashingmagazine.com/2016/05/fluid-typography/

# Resources on Typography

***[Thinking with Type: A Critical Guide for Designers, Writers, Editors, & Students](http://www.amazon.com/Thinking-Type-2nd-revised-expanded/dp/1568989695/)* (Ellen Lupton)**A classic introduction to type.

***[Stop Stealing Sheep & Find Out How Type Works](http://www.amazon.com/Stealing-Edition-Graphic-Communication-Courses/dp/0321934288/)* (Erik Spiekermann)**A design classic, recently updated with new info on web typography.

***[The Anatomy of Type: A Graphic Guide to 100 Typefaces](http://www.amazon.com/The-Anatomy-Type-Graphic-Typefaces/dp/0062203126/)* (Stephen Cole)**An interesting and useful visual resource.

***[On Web Typography](http://www.abookapart.com/products/on-web-typography)* (Jason Santa Maria)**On how to relate typography principles to the context of the web.

***[Why Fonts Matter](http://www.amazon.com/The-Type-Taster-Fonts-Influence/dp/0993181104)* (Sarah Hyndman)**An engaging book on the impact of type on our feelings and senses.

**_[Designing with Type: The Essential Guide to Typography](http://www.amazon.com/Designing-Type-5th-Essential-Typography/dp/0823014134/) (James Craig)_**This is considered the industry standard typography book with an [online companion site](http://www.designingwithtype.com/5/home.php).

**_[The Elements of Typographic Style](http://www.amazon.com/The-Elements-Typographic-Style-Anniversary/dp/0881792128/)_**Intro to type by one of Canada’s most highly regarded typographers.

- Media queries cause a visual jump between font-sizes.
- To achieve fluid typography combine the calc() function in css with viewport units (vw/vw/vmin/vmax). html { font-size: calc(1em + 1vw);}

## Line Height

- https://codepen.io/Devvventure/pen/dyYWjoy
- https://dev.to/lampewebdev/css-line-height-jjp

# Resources

- https://css-tricks.com/simplified-fluid-typography/
- https://www.smashingmagazine.com/2016/05/fluid-typography/

-
