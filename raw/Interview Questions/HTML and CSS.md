---

---
### Why is doctype used?

The <!DOCTYPE html> declaration is required to tell the browser to render the page in **Standards Mode**.

Without this declaration, browsers fall back into **Quirks Mode**.
 In Quirks Mode, modern browsers attempt to emulate the buggy behaviors 
of browsers from the late 90s (like Internet Explorer 5) to maintain 
backward compatibility with ancient websites. This results in 
unpredictable behavior, specifically regarding the "Box Model" (how 
width and padding are calculated) and table inheritance. Always include 
it as the very first line to ensure your CSS renders consistently across
 Chrome, Firefox, and Safari.

### Best place to mount JS file (head or body)

While the traditional advice was to place scripts at the bottom of the <body> to prevent blocking the HTML parser, the modern best practice is to place them in the <head> combined with the defer attribute.

- **Bottom of Body:** Ensures HTML is parsed before scripts run, but the browser doesn't
start downloading the script until it reaches the end of the file.
- **Head with defer:** This is superior because the browser's "lookahead pre-parser" can spot
the script tag immediately and start downloading it in the background
while the HTML is still parsing. It executes only after the HTML is
fully built. This results in a faster "Time to Interactive" for the
user.

### Defer vs. Async

Both attributes allow the script to download in parallel without pausing HTML parsing, but the difference lies in **execution timing and order**:

- **Async:** The script executes the exact moment it finishes downloading. This means it will pause the HTML parser *during* execution. Crucially, async scripts are not guaranteed to run in order. If you have a small script
that depends on a large library (like jQuery), the small script might
load and run first, causing an error. Use this for independent
third-party scripts like Google Analytics or Ad trackers.
- **Defer:** The script downloads in parallel but waits to execute until the HTML document is fully parsed. defer scripts also respect the order in which they appear in the code. This
is the correct choice for your main application bundles and dependent
libraries.

### Semantic HTML

Semantic HTML means using tags that clearly describe the meaning of the content, such as <nav>, <article>, <section>, and <button>, rather than using generic <div> or <span> tags for everything.

There are two major business reasons for this:

1. **SEO (Search Engine Optimization):** Search bots use these tags to understand the hierarchy and importance of content. A page structured with <article> and <h1> ranks better than a soup of nested divs.
2. **Accessibility (a11y):** Screen readers used by the visually impaired rely on semantic tags to
navigate. For example, a screen reader can jump specifically to a <nav> element. If you use a clickable <div> instead of a real <button>, you break keyboard navigation and exclude users, which can lead to legal liability.

### Div vs. Span

- **Div (<div>):** A **block-level** element. It naturally starts on a new line and expands to fill the full width of its container. It is intended to group larger chunks of
content or define layout structure.
- **Span (<span>):** An **inline** element. It does not start a new line and only takes up as much width
as the content inside it. It is intended for styling small pieces of
text *within* a block (e.g., making one word inside a paragraph red).

### Can you apply padding and margin to span?

Yes, but it behaves differently than block elements:

- **Horizontal (Left/Right):** Padding and margin work as expected and will push adjacent text away.
- **Vertical (Top/Bottom):** Padding and margin can be applied and will render visually (e.g., the background color will expand), but they **will not affect the document flow**. The span will bleed over the lines of text above and below it rather than pushing them away.
- *Solution:* To apply effective vertical spacing, you must change the styling to display: inline-block or display: block.

### Box Sizing: Border Box

By default, CSS uses content-box. In this mode, if you set a width of 100px and a padding of 20px, the total rendered width is 140px
 (100 + 20 left + 20 right). This makes responsive layouts difficult 
because adding padding often breaks the layout (e.g., pushing content to
 the next line).

box-sizing: border-box changes the calculation so that the padding and border are included *inside* the defined width. If you set width to 100px and padding to 20px, the element stays 100px wide, and the content area shrinks to 60px. Experienced developers apply this globally (* { box-sizing: border-box; }) at the start of every project to make layout math intuitive.

### Are images inline?

Yes, technically images are **replaced elements** that behave like inline-block elements.

The
 most common side effect of this is the "magic gap": you will often see a
 3px to 4px space underneath an image when it sits inside a div. This 
happens because the browser treats the image like a letter in a 
sentence, aligning it to the **baseline**.
 That extra space is reserved for "descenders" (the tails of letters 
like j, g, y, p). To remove this, you must set the image to display: block or set its vertical-align to bottom.

### Diff between vh and vw, what is correct unit for this

- **vw (Viewport Width):** 1vw is equal to 1% of the width of the viewport.
- **vh (Viewport Height):** 1vh is equal to 1% of the height of the viewport.

**The "Correct" Unit:**

While vh works well on desktops, it is flawed on mobile browsers (like iOS 
Safari and Chrome Android). These browsers have dynamic address bars 
that appear and disappear as you scroll. Standard 100vh does not account for the address bar, causing the bottom of your content to be cut off or covered by the UI.

The modern "correct" unit for full-height layouts on mobile is **dvh (Dynamic Viewport Height)** or **svh (Small Viewport Height)**, which dynamically adjusts based on whether the browser UI is visible.

### Em vs. Rem

- **Rem (Root Em):** The size is relative to the font size of the root <html> element (usually 16px by default). If html is 16px, 1rem = 16px, 2rem = 32px.
- **Em:** The size is relative to the font size of the **parent** element.

**Detailed Application:**

Use **rem**
 for font sizes and main layout structures. It ensures consistency across the site and respects the user's browser accessibility settings.

Use **em** for properties like padding or margin on specific components (like buttons). If you define a button's padding in em, the padding will scale automatically if you increase the button's font size, keeping the proportions perfect. Avoid using em
 for font sizes in deeply nested structures, or you will trigger the 
"compounding effect" (e.g., 1.2em inside a 1.2em inside a 1.2em parent 
results in massive text).

### **Positioning in CSS**

CSS positioning determines how an element is placed in the document flow. It's controlled by the `position` property.

- `**static**`**:** The default value. The element is positioned according to the normal flow of the document. The `top`, `right`, `bottom`, `left`, and `z-index` properties have no effect.
- `**relative**`**:** The element is positioned according to the normal flow, but then you can offset it relative to its *original position* using `top`, `right`, `bottom`, and `left`. This does not affect the position of any other elements. Crucially, it creates a new **stacking context** and a **positioning context** for its descendant elements that have `position: absolute`.
- `**absolute**`**:** The element is removed from the normal document flow. It is positioned relative to its nearest *positioned ancestor* (an ancestor with a `position` value other than `static`). If no such ancestor exists, it is positioned relative to the initial containing block (usually the `<html>` element).
- `**fixed**`**:** The element is removed from the normal document flow and is positioned relative to the **viewport** (the browser window). It stays in the same place even when the page is scrolled. This is often used for sticky headers or modals.
- `**sticky**`**:** This is a hybrid of `relative` and `fixed`. The element is treated as `position: relative` until it crosses a specified threshold (defined by `top`, `right`, etc.) within its containing block, at which point it becomes `position: fixed` until it hits the boundary of its parent.

### **Transform vs. Translate**

This is a common point of confusion. They are not alternatives; one is a property, and the other is a function used by that property.

- `**transform**`**:** This is the CSS **property**. It allows you to modify the coordinate space of an element, enabling you to rotate, scale, skew, or move (translate) it.
- `**translate()**`**:** This is one of the **functions** you can use as a value for the `transform` property. It specifically moves an element along the X, Y, or Z axis from its current position.

**Example:**

```css
.box {
  /* `transform` is the property, `translate()` is the function value */
  transform: translate(50px, 100px);
  transform: rotate(45deg);
  transform: scale(1.5);
}

```

**Key Performance Consideration:** Animating the `transform` property (especially `translate` and `scale`) is significantly more performant than animating layout properties like `top`, `left`, or `margin`. This is because `transform` operations can often be offloaded to the **GPU**, which runs them on a separate layer (compositor thread) without triggering a browser reflow or repaint of the main document.

### **Flexbox and Grid basics**

Both are modern CSS layout models, but they are designed for different use cases.

- **Flexbox (Flexible Box Layout):**
    - **One-Dimensional:** It excels at distributing space and aligning items in a single dimension—either a row or a column.
    - **Use Case:** Perfect for component-level layouts, like aligning items in a navigation bar, centering a button in a div, or creating evenly spaced cards in a single row. In my `Conversex` project, I used Flexbox extensively for aligning elements within chat messages, headers, and sidebars.
    - **Key Properties:** `display: flex`, `flex-direction` (row/column), `justify-content` (main-axis alignment), `align-items` (cross-axis alignment).
- **Grid Layout:**
    - **Two-Dimensional:** It is designed for laying out items in both rows and columns simultaneously. It gives you precise control over the entire page or a large component's structure.
    - **Use Case:** Ideal for overall page layouts, like creating a header, sidebar, main content, and footer structure. In `Flowify`, the board list in the organization view was a perfect use case for Grid, where I defined columns to create a responsive grid of board previews.
    - **Key Properties:** `display: grid`, `grid-template-columns`, `grid-template-rows`, `gap`.

![[HTML and CSS synced block]]