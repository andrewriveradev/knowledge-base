# Rem: (Root em)

a unit of typography equal to the **root font-size**. 1rem is always equal to the font-size defined in <html>.So, all references to rem will return the same value, regardless of ancestor font size. In effect, rem is a document-wide CSS variable. The most obvious use for rem is replacing em to protect inner element font sizes from being changed by outer elements, thus avoiding the classic nested em scaling problem.

Use rems for:

- typography
- spacing and font-sizing
- To size elements independently from all other elements

# Ems :

Sizing ems are relative to their parent's font size. historically, em was a measurement based on the width of the capital M letter. In modern web design, it is more commonly equated to roughly 16 pixels.

![https://cdn.tutsplus.com/webdesign/uploads/2013/05/demystifying-ems-1-em.png](https://cdn.tutsplus.com/webdesign/uploads/2013/05/demystifying-ems-1-em.png)

Ems are for relational design. It is based off the font-size of the parent element. Ems can be used to style children in relation to parent (container items or text) [example](https://codepen.io/dixita0607/pen/LQNoOL)

**Caveats**

One caveat to using ems is nest em scaling problem. To fix this problem just be cognizant of how you create and style HTML. If you design your HTML simple and modular ems measurements are easy to reason about.

This is where many people start to get confused with em values.

Consider the following code. What should the margin-bottom value be for both the <h1> and <p> elements? (Assume font-size of <html> is set to 100%).

h1 {font-size: 2em; /_ 1em = 16px _/margin-bottom: 1em; /_ 1em = 32px _/}p {font-size: 1em; /_ 1em = 16px _/margin-bottom: 1em; /_ 1em = 16px _/}

Are you surprised that the computed value of 1em on margin-bottom is different in these two scenarios`?

This phenomenon occurs because 1em is equal to its current font-size. **Since the font-size in <h1> is now set to 2em. Other properties computed with em in <h1> would see that 1em = 32px**.

What throws people off is that 1em can take on different values in different parts of the code. It can be confusing if you’re just starting out with ems.

Use for:

- [Border-radius property / Rounded boxes](https://codepen.io/andrewriveraj/pen/wVGGjB)

### **Benefits of REMs & EMs**

The main benefit I’ve noticed of basing a site off REMs & EMs is ease of responsiveness. If the content, structure, etc is based off the root font-size then the only media queries needed are on the HTML font-size. When that is scaled up, the entire page scales up. To have this work, it’s best to have the containers based on REMs and the font based on EMs. That is because we want the containers to scale up relative to the HTML root font size and the contents inside of them to be relative to their containers.
