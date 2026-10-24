# CSS Interview Questions and Answers

There are 450 questions here: 150 easy, 150 moderate and 150 advanced. They cover modern CSS as it works in current browsers (late 2026). This includes newer features like nesting, cascade layers, container queries, `:has()`, anchor positioning and view transitions. If a feature only works in some browsers, the answer says so. Check MDN or caniuse before you use it in production.

## Table of Contents

## Easy Questions (1-150)

### 1: What is CSS?

CSS (Cascading Style Sheets) is the language used to style web pages. HTML gives the page its structure and content. CSS controls how it looks: colors, fonts, spacing and layout, and how all of that changes at different screen sizes. One stylesheet can style many pages, so a design change only has to be made in one place.

### 2: What are the ways to add CSS to a page?

There are three:

1. **External stylesheet**: a `.css` file linked with `<link>`. This is the normal choice for real projects.
2. **Internal stylesheet**: a `<style>` block inside the HTML document.
3. **Inline styles**: a `style` attribute on an element.

```html
<link rel="stylesheet" href="styles.css">

<style>
  h1 { color: navy; }
</style>

<p style="color: red;">Inline style</p>
```

Inline styles have the highest priority of the three, but they are hard to reuse and maintain.

### 3: What does a CSS rule look like?

A rule has a selector, followed by a block of declarations in curly braces. Each declaration is a property and a value, with a semicolon at the end.

```css
h1 {
  color: navy;
  font-size: 2rem;
}
```

Here `h1` is the selector, `color` and `font-size` are properties, and `navy` and `2rem` are values.

### 4: What is a selector?

A selector tells the browser which elements a rule applies to. It can match by tag name (`p`), class (`.card`), id (`#header`), attribute (`[type="email"]`), state (`:hover`), or by where the element sits in the document (`nav a`).

### 5: What is the difference between type, class and id selectors?

- **Type selector** (`p`) matches every element with that tag name.
- **Class selector** (`.button`) matches every element that has that class. One element can have many classes, and a class can be used on many elements.
- **ID selector** (`#main`) matches the single element with that id. An id should be unique on the page.

Most teams style with classes and avoid ids. Ids have very high specificity, which makes them hard to override later.

### 6: What does the universal selector `*` do?

`*` matches every element. It's mostly used in resets, for example to set `box-sizing` on everything.

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

It has zero specificity, so it doesn't make the rules that use it harder to override.

### 7: How do you apply the same styles to several selectors?

Separate the selectors with commas:

```css
h1, h2, h3 {
  font-family: Georgia, serif;
}
```

In a normal selector list, if one selector is invalid the whole rule is dropped. `:is()` and `:where()` are "forgiving": they skip the invalid selector and keep the rest.

### 8: What is the difference between the descendant and child combinators?

- `nav a` (a space) is the **descendant combinator**. It matches any `a` inside `nav`, at any depth.
- `nav > a` is the **child combinator**. It only matches `a` elements that are direct children of `nav`.

```css
ul li { color: gray; }   /* every li inside the ul, including nested lists */
ul > li { color: black; } /* only the top-level li elements */
```

### 9: What is the difference between `+` and `~` combinators?

- `h2 + p` (**next-sibling combinator**) matches a `p` that comes right after an `h2` and has the same parent.
- `h2 ~ p` (**subsequent-sibling combinator**) matches every `p` that comes after an `h2` and has the same parent. It doesn't have to come right after.

Both only look forward. To style an element based on something that comes after it, use `:has()`.

### 10: What are attribute selectors?

They match elements based on their attributes and attribute values.

```css
[disabled] { opacity: 0.5; }                /* has the attribute */
[type="email"] { border-color: blue; }      /* exact value */
a[href^="https"] { color: green; }          /* starts with */
a[href$=".pdf"]::after { content: " (PDF)"; } /* ends with */
[class*="icon-"] { display: inline-block; }  /* contains */
[lang|="en"] { quotes: "\201C" "\201D"; }    /* "en" or starts with "en-" */
```

Add ` i` before the closing bracket to make the value match case-insensitive: `[type="EMAIL" i]`.

### 11: What is a pseudo-class?

A pseudo-class selects an element based on its state or position, not on its tag, class or id. It starts with a single colon.

```css
a:hover { text-decoration: underline; }
input:focus { outline: 2px solid blue; }
li:first-child { font-weight: bold; }
input:disabled { background: #eee; }
```

### 12: What is a pseudo-element?

A pseudo-element styles part of an element, or adds generated content that isn't in the HTML. It uses two colons.

```css
p::first-line { font-weight: bold; }
.required::after { content: " *"; color: red; }
::selection { background: yellow; }
```

`::before` and `::after` are the most common ones. They don't show up unless you set `content`.

### 13: What is the difference between `:` and `::`?

A single colon is for pseudo-classes (`:hover`) and a double colon is for pseudo-elements (`::before`). Before CSS3, pseudo-elements also used one colon, so browsers still accept `:before` and `:after` for older code. In new code, use `::`.

### 14: What does "cascading" mean in CSS?

The cascade is the set of rules a browser uses when more than one declaration sets the same property on the same element. It checks these, in this order:

1. Origin and importance (browser defaults, user styles, author styles, and whether the declaration is `!important`).
2. Cascade layers.
3. Specificity.
4. Source order. If everything else is equal, the declaration that comes last wins.

### 15: What is specificity?

Specificity is a score the browser gives each selector. When two rules set the same property on the same element, the selector with the higher score wins, whatever their order in the file. An id is worth more than a class, and a class is worth more than a tag name.

### 16: How do you calculate specificity?

It's written as three numbers (A, B, C):

- **A**: number of id selectors.
- **B**: number of classes, attribute selectors and pseudo-classes.
- **C**: number of type selectors and pseudo-elements.

Compare A first, then B, then C.

```css
p                 /* (0,0,1) */
.card p           /* (0,1,1) */
#nav .link:hover  /* (1,2,0) */
ul li a::before   /* (0,0,4) */
```

`*` and combinators add nothing. Inline styles beat any selector, and `!important` beats normal declarations.

### 17: What does `!important` do and when should you use it?

`!important` makes a declaration win over normal declarations, no matter the specificity. It's hard to override. You need another `!important` with a higher priority, which leads to `!important` wars.

```css
.hidden { display: none !important; }
```

Fair uses are utility classes that must always apply, and overriding inline styles from a third-party script you can't change. Don't use it to fix specificity problems you created yourself.

### 18: What is inheritance in CSS?

Some properties pass their value from a parent element down to its children. Text properties like `color`, `font-family`, `font-size`, `line-height` and `visibility` inherit. Box properties like `margin`, `padding`, `border`, `width` and `background` don't.

```css
body {
  font-family: system-ui, sans-serif;
  color: #222;
}
/* every element inside body gets this font and color unless it sets its own */
```

### 19: What do `inherit`, `initial`, `unset` and `revert` do?

They are keywords that work on any property:

- `inherit`: use the parent's value.
- `initial`: use the property's default from the spec. For `display`, that's `inline`, even on a `div`.
- `unset`: act like `inherit` for properties that inherit and like `initial` for those that don't.
- `revert`: go back to what the previous origin would give, usually the browser's default style. For a `div`, `display: revert` gives `block`.

There is also `revert-layer`, which goes back to the value from the previous cascade layer.

### 20: What is the CSS box model?

Every element is drawn as a rectangular box made of four layers, from the inside out:

1. **Content**: the text or image.
2. **Padding**: space between the content and the border.
3. **Border**: the line around the padding.
4. **Margin**: space outside the border that pushes other elements away.

The background covers the content, the padding and the area under the border. The margin is always transparent.

### 21: What is the difference between `content-box` and `border-box`?

They change what `width` and `height` measure.

- `content-box` (the default): `width` is only the content. Padding and border are added on top.
- `border-box`: `width` includes the content, the padding and the border.

```css
.box {
  width: 200px;
  padding: 20px;
  border: 5px solid;
}
/* content-box: the box takes 250px */
/* border-box: the box takes 200px, the content area shrinks to 150px */
```

`border-box` is easier to work with, which is why most resets use it.

### 22: How do you make every element use `border-box`?

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

Some people set it on `html` and use `box-sizing: inherit` on everything else. That lets a component switch back to `content-box` for its whole subtree. Both versions are common.

### 23: What is the difference between margin and padding?

Padding is space inside the element, between its content and its border. Margin is space outside the border, between this element and others. Padding gets the element's background. Margin is always transparent. Vertical margins between blocks can collapse, but padding never does.

### 24: What is margin collapsing?

When two vertical margins of block elements touch, they merge into one margin, and its size is the larger of the two. They aren't added together.

```css
h2 { margin-bottom: 30px; }
p  { margin-top: 20px; }
/* the gap between them is 30px, not 50px */
```

This also happens between a parent and its first or last child, if no border, padding or inline content sits between them. It doesn't happen with horizontal margins, inside flex or grid containers, or for floated and absolutely positioned elements.

### 25: How do the margin and padding shorthands work with 1, 2, 3 or 4 values?

They go clockwise from the top:

```css
margin: 10px;                /* all four sides */
margin: 10px 20px;           /* top/bottom, left/right */
margin: 10px 20px 30px;      /* top, left/right, bottom */
margin: 10px 20px 30px 40px; /* top, right, bottom, left */
```

To remember the order, think of the word "TRouBLe" (Top, Right, Bottom, Left).

### 26: What is the difference between `block`, `inline` and `inline-block`?

- **block**: starts on a new line and fills the available width. `width`, `height`, margin and padding all work as you'd expect. Examples: `div`, `p`, `h1`.
- **inline**: flows inside a line of text. It ignores `width` and `height`, and its vertical margin and padding don't push other lines away. Examples: `span`, `a`, `strong`.
- **inline-block**: sits in the line like inline, but accepts `width`, `height` and vertical spacing like a block.

### 27: What is the difference between `display: none`, `visibility: hidden` and `opacity: 0`?

| | Takes up space | Can be clicked | Read by screen readers |
|---|---|---|---|
| `display: none` | No | No | No |
| `visibility: hidden` | Yes | No | No |
| `opacity: 0` | Yes | Yes | Yes |

`opacity` and `visibility` can be animated. Since 2024 you can also animate to and from `display: none` with `transition-behavior: allow-discrete` and `@starting-style`.

### 28: What is `position: static`?

It's the default for every element. The element sits in the normal document flow, and `top`, `right`, `bottom`, `left` and `z-index` have no effect on it.

### 29: What does `position: relative` do?

The element stays in the normal flow, but you can move it with `top`, `left` and the other offsets. Its original space is kept, so other elements don't move. It's mostly used to make the element the reference point for absolutely positioned children.

```css
.card { position: relative; }
.badge { position: absolute; top: 8px; right: 8px; }
```

### 30: What does `position: absolute` do?

The element is taken out of the normal flow, so other elements act like it isn't there. Its position is set by the offsets, measured from the nearest ancestor that is positioned (anything other than `static`). If there is no such ancestor, it's measured from the initial containing block, which has the size of the viewport and sits at the top of the page.

### 31: What does `position: fixed` do?

The element is taken out of the flow and positioned relative to the viewport, so it stays in place when the page scrolls. It's used for headers, chat buttons and cookie banners. If an ancestor has a `transform`, `filter` or a few similar properties, the element becomes fixed to that ancestor instead of the viewport.

### 32: What does `position: sticky` do?

The element acts like `relative` until you scroll past a threshold you set, then it sticks like `fixed`, but only inside its parent.

```css
.table-header {
  position: sticky;
  top: 0;
}
```

It needs at least one offset (`top`, `bottom`, etc.). It stops working if an ancestor between it and the scroll container has `overflow: hidden`, `auto` or `scroll`.

### 33: What is `z-index`?

`z-index` decides which elements appear on top when they overlap. A higher value is closer to the viewer. It works on positioned elements (not `static`) and on flex and grid items. The value is only compared with other elements in the same stacking context. That's why `z-index: 9999` sometimes doesn't help.

### 34: Why does `z-index` sometimes have no effect?

The usual reasons:

1. The element is `position: static` and isn't a flex or grid item.
2. The element is inside a parent that created its own stacking context with a lower level. A child can never appear above something that is above its parent's stacking context.
3. The other element comes later in the source and has the same or a higher `z-index` in the same context.

Use DevTools (the Layers panel in Chrome, or the 3D View in Edge) to see which stacking contexts exist.

### 35: What do `float` and `clear` do?

`float: left` or `float: right` moves an element to one side and lets text and inline content wrap around it. `clear` stops an element from sitting next to floats that come before it.

```css
img.avatar { float: left; margin-right: 1rem; }
.footer { clear: both; }
```

Floats were used for page layout for years. Today that job belongs to flexbox and grid. Use floats for what they were made for: wrapping text around an image.

### 36: What is a clearfix?

A parent that only has floated children collapses to zero height, because floats are out of the normal flow. The old fix was a "clearfix" hack:

```css
.clearfix::after {
  content: "";
  display: table;
  clear: both;
}
```

The modern way is `display: flow-root` on the parent. It creates a new block formatting context, so the parent contains its floats with no hack.

```css
.container { display: flow-root; }
```

### 37: What are the values of `overflow`?

`overflow` controls what happens when content is bigger than its box.

- `visible` (default): the content spills out.
- `hidden`: the extra content is cut off. It can still be scrolled with code.
- `clip`: like `hidden`, but no scrolling at all, even with code, and it doesn't create a scroll container.
- `scroll`: always shows scrollbars.
- `auto`: shows scrollbars only when needed.

You can set each axis on its own with `overflow-x` and `overflow-y`.

### 38: What is the difference between `px`, `em` and `rem`?

- `px` is a fixed unit. One CSS pixel isn't always one device pixel.
- `em` is relative to the element's own font size. When used on `font-size`, it's relative to the parent's font size.
- `rem` is relative to the root (`html`) font size, which is usually 16px.

```css
html { font-size: 100%; }  /* usually 16px */
h1 { font-size: 2rem; }    /* 32px */
.btn { padding: 0.5em 1em; } /* scales with the button's own font size */
```

Use `rem` for font sizes so they respect the user's browser settings. `em` is handy for spacing that should grow with the text, like button padding.

### 39: What is the `%` unit relative to?

It depends on the property:

- `width`, and `left`/`right`: the width of the containing block.
- `height`, and `top`/`bottom`: the height of the containing block. It only works if that height is defined.
- `padding` and `margin`: always the **width** of the containing block, even for top and bottom.
- `font-size`: the parent's font size.
- `transform: translate()`: the element's own size.

### 40: What are `vw` and `vh`?

`1vw` is 1% of the viewport width and `1vh` is 1% of the viewport height. `vmin` and `vmax` use whichever side is smaller or larger.

```css
.hero { height: 100vh; }
h1 { font-size: 5vw; }
```

On mobile, `100vh` is often taller than the area you can actually see, because the browser's address bar shows and hides as you scroll. The newer `dvh`, `svh` and `lvh` units were added to fix this.

### 41: What are `svh`, `lvh` and `dvh`?

They are viewport units that know about mobile browser toolbars:

- `svh` (small): the viewport height when the toolbars are showing. This is the smallest size.
- `lvh` (large): the height when the toolbars are hidden. This is the largest size.
- `dvh` (dynamic): changes as the toolbars show and hide.

```css
.full-screen {
  min-height: 100svh; /* never taller than the visible area */
}
```

`dvh` changes while the user scrolls, which can cause layout to jump. `svh` is usually the safer choice for full-screen sections. There are matching width units (`svw`, `lvw`, `dvw`) and inline/block versions.

### 42: What are the `ch` and `ex` units?

`ch` is the width of the "0" character in the current font. `ex` is the x-height (the height of a lowercase "x"). `ch` is a common way to limit line length so text is easy to read:

```css
article { max-width: 65ch; }
```

### 43: What color formats does CSS support?

```css
color: tomato;                    /* named color */
color: #ff6347;                   /* hex */
color: #f63;                      /* short hex */
color: rgb(255 99 71);            /* rgb */
color: hsl(9 100% 64%);           /* hue, saturation, lightness */
color: oklch(70% 0.19 35);        /* lightness, chroma, hue */
color: color(display-p3 1 0.4 0.3); /* wide gamut */
```

`hsl()` and `oklch()` are easier to tweak by hand than hex. `oklch()` also keeps brightness more consistent between different hues.

### 44: How do you add transparency to a color?

Add an alpha value. Modern syntax uses spaces and a slash. The older comma syntax and `rgba()`/`hsla()` still work.

```css
background: rgb(0 0 0 / 50%);
background: hsl(220 90% 50% / 0.3);
background: #00000080;          /* 8-digit hex, last two digits are alpha */
background: rgba(0, 0, 0, 0.5); /* older syntax */
```

### 45: What is `currentColor`?

`currentColor` is a keyword that means "the value of this element's `color` property". It's useful for borders, SVG icons and shadows that should always match the text color.

```css
.button {
  color: purple;
  border: 2px solid currentColor;
}
.icon { fill: currentColor; }
```

### 46: What is the difference between `opacity` and an alpha color?

`opacity` makes the whole element see-through, including its text and all its children. An alpha color only affects the property it's used on.

```css
.overlay { opacity: 0.5; }                  /* text gets faded too */
.overlay { background: rgb(0 0 0 / 50%); }  /* only the background is faded */
```

An `opacity` below 1 also creates a new stacking context.

### 47: How does `font-family` work?

You give a list of fonts. The browser uses the first one that's available, and the list should end with a generic family as the last fallback.

```css
body {
  font-family: "Inter", "Helvetica Neue", Arial, sans-serif;
}
```

Generic families include `serif`, `sans-serif`, `monospace`, `cursive`, `system-ui` and `ui-monospace`. Put quotes around names that contain spaces.

### 48: Which properties control font size, weight and style?

```css
p {
  font-size: 1rem;
  font-weight: 400;     /* normal; 700 is bold */
  font-style: italic;
}
```

There is also a `font` shorthand: `font: italic 700 1rem/1.5 system-ui, sans-serif;`. It resets every font property you leave out, so be careful with it.

### 49: Why should `line-height` usually have no unit?

A unitless value like `1.5` is passed down to children as a multiplier, so each child works out its own line height from its own font size. With a unit like `24px` or `1.5em`, children inherit the final computed number, which may be too small for larger headings.

```css
body { line-height: 1.5; } /* good */
body { line-height: 1.5em; } /* children get a fixed px value */
```

### 50: Which properties are used to style text?

```css
h1 {
  text-align: center;          /* left, right, center, justify, start, end */
  text-decoration: underline;  /* none, underline, line-through */
  text-transform: uppercase;   /* lowercase, capitalize */
  text-indent: 2em;
}
```

`text-decoration` is a shorthand. You can set the line, style, color and thickness: `text-decoration: underline wavy red 2px;`. `text-underline-offset` moves the underline away from the text.

### 51: What do `letter-spacing` and `word-spacing` do?

`letter-spacing` adds or removes space between characters, and `word-spacing` does the same between words. Both accept negative values.

```css
.label {
  text-transform: uppercase;
  letter-spacing: 0.05em;
}
```

Use `em` so the spacing grows and shrinks with the font size.

### 52: What does the `white-space` property do?

It controls how spaces and line breaks in the source are handled.

- `normal`: spaces collapse into one and lines wrap.
- `nowrap`: spaces collapse, but the text never wraps.
- `pre`: keeps spaces and line breaks, no wrapping (like `<pre>`).
- `pre-wrap`: keeps spaces and line breaks, and still wraps.
- `pre-line`: collapses spaces, keeps line breaks, and wraps.

In newer browsers, `white-space` is a shorthand for `white-space-collapse` and `text-wrap-mode`.

### 53: How do you cut off long text with "..."?

You need all three of these on a block with a set width:

```css
.title {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

This only works for one line. For several lines, use `line-clamp`.

### 54: What is `@font-face`?

`@font-face` loads a custom font file and gives it a name you can use in `font-family`.

```css
@font-face {
  font-family: "Inter";
  src: url("/fonts/inter.woff2") format("woff2");
  font-weight: 100 900;
  font-style: normal;
  font-display: swap;
}

body { font-family: "Inter", sans-serif; }
```

WOFF2 is the format to use today. It's compressed and every modern browser supports it.

### 55: What is a system font stack?

It's a `font-family` list that uses the operating system's own UI font, so nothing has to be downloaded.

```css
body {
  font-family: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
}
```

Text shows up instantly with no layout shift. The trade-off is that the font looks a little different on each OS.

### 56: What are the main background properties?

```css
.hero {
  background-color: #222;
  background-image: url("hero.jpg");
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  background-attachment: scroll; /* or fixed, local */
  background-clip: border-box;   /* or padding-box, content-box, text */
}
```

The `background` shorthand sets all of them at once: `background: #222 url("hero.jpg") center / cover no-repeat;`.

### 57: Can an element have more than one background?

Yes. Separate the layers with commas. The first one is drawn on top.

```css
.banner {
  background:
    linear-gradient(rgb(0 0 0 / 60%), transparent),
    url("photo.jpg") center / cover;
}
```

This is the usual way to darken an image so text on top of it is easier to read.

### 58: What is the difference between `background-size: cover` and `contain`?

- `cover` scales the image so it fills the whole box. Parts of the image may get cut off.
- `contain` scales the image so the whole image fits inside the box. There may be empty space around it.

Both keep the image's aspect ratio.

### 59: How do you create a gradient?

Gradients are images, so they go in `background-image`.

```css
.a { background: linear-gradient(to right, #4f46e5, #ec4899); }
.b { background: linear-gradient(45deg, red 0%, orange 50%, yellow 100%); }
.c { background: radial-gradient(circle at top left, white, steelblue); }
.d { background: conic-gradient(red, yellow, green, red); }
.e { background: repeating-linear-gradient(45deg, #000 0 10px, #fff 10px 20px); }
```

### 60: How do `border` and `border-radius` work?

```css
.card {
  border: 1px solid #ddd;   /* width, style, color */
  border-radius: 8px;       /* rounded corners */
}
.tab {
  border-radius: 8px 8px 0 0; /* top-left, top-right, bottom-right, bottom-left */
}
```

A border won't show without a style (`solid`, `dashed`, `dotted` and so on). The default style is `none`.

### 61: How do you make a circle?

Give the element the same width and height and set `border-radius: 50%`.

```css
.avatar {
  width: 48px;
  aspect-ratio: 1;
  border-radius: 50%;
  object-fit: cover;
}
```

For a pill shape on a rectangle, use a very large radius like `border-radius: 999px`.

### 62: What is the difference between `outline` and `border`?

A border is part of the box model, so it takes up space and can move other elements. An outline is drawn outside the border and takes up no space. Outlines are used for focus rings. You can move an outline away from the element with `outline-offset`. Modern browsers make outlines follow `border-radius`.

Never remove focus outlines without putting a visible focus style in their place.

### 63: How does `box-shadow` work?

```css
.card {
  /* x-offset y-offset blur spread color */
  box-shadow: 0 4px 12px 0 rgb(0 0 0 / 15%);
}
.input:focus {
  box-shadow: inset 0 0 0 2px royalblue; /* inner shadow */
}
.layered {
  box-shadow:
    0 1px 2px rgb(0 0 0 / 10%),
    0 8px 24px rgb(0 0 0 / 10%);
}
```

Several shadows stacked on top of each other usually look more natural than one big shadow.

### 64: How do you add a shadow to text?

```css
h1 {
  text-shadow: 1px 1px 2px rgb(0 0 0 / 40%); /* x y blur color */
}
```

There is no spread value for text shadows. You can add several shadows separated by commas.

### 65: What do `min-width`, `max-width`, `min-height` and `max-height` do?

They set limits on an element's size. `max-width` stops an element from growing past a size, and `min-width` stops it from shrinking below one. If they conflict, `min-*` wins over `max-*`, and `max-*` wins over `width`.

```css
.container {
  width: 100%;
  max-width: 1200px;
  margin-inline: auto;
}
```

### 66: What does `auto` mean for `width` and `margin`?

For a block element, `width: auto` fills the available space, minus margins, borders and padding. This is different from `width: 100%`, which ignores margins and can cause overflow.

`margin: auto` takes the leftover space. On a block with a set width, left and right `auto` margins split that space evenly, which centers the block.

### 67: How do you center a block element horizontally?

Give it a width (or `max-width`) and set its left and right margins to `auto`.

```css
.container {
  max-width: 800px;
  margin-inline: auto; /* same as margin-left: auto; margin-right: auto; */
}
```

### 68: How do you center text?

`text-align: center` centers inline content, like text, images and inline-blocks, inside a block. It doesn't center the block itself.

```css
.hero { text-align: center; }
```

### 69: What is Flexbox?

Flexbox (`display: flex`) is a layout model for laying out items in one direction, either a row or a column. It makes it easy to share space between items, align them, change their order and handle wrapping, without floats or hacks.

```css
.toolbar {
  display: flex;
  align-items: center;
  gap: 8px;
}
```

### 70: What are the main axis and the cross axis in Flexbox?

The **main axis** is the direction set by `flex-direction`: horizontal for `row`, vertical for `column`. The **cross axis** is the other direction.

`justify-content` aligns items along the main axis, and `align-items` aligns them along the cross axis. When you switch to `flex-direction: column`, the two swap roles.

### 71: What does `flex-direction` do?

It sets the direction of the main axis:

- `row` (default): left to right in left-to-right languages.
- `row-reverse`: right to left.
- `column`: top to bottom.
- `column-reverse`: bottom to top.

The reverse values only change the visual order. Keyboard tab order and screen readers still follow the HTML order.

### 72: What does `justify-content` do?

It spreads items along the main axis.

```css
.nav {
  display: flex;
  justify-content: space-between;
}
```

Values: `flex-start`, `flex-end`, `center`, `space-between` (no space at the ends), `space-around` (half-size space at the ends) and `space-evenly` (equal space everywhere).

### 73: What does `align-items` do?

It aligns items on the cross axis.

- `stretch` (default): items fill the container's cross size.
- `flex-start` / `flex-end` / `center`.
- `baseline`: lines up the first line of text in each item.

### 74: What does `flex-wrap` do?

By default (`nowrap`), flex items stay on one line and shrink to fit. `flex-wrap: wrap` lets them move to new lines when there isn't enough room.

```css
.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}
```

`flex-flow` is a shorthand for `flex-direction` and `flex-wrap`: `flex-flow: row wrap;`.

### 75: What does the `gap` property do?

`gap` adds space between flex items, grid tracks or multi-column columns, but not around the outside edges. It replaced the old trick of putting margins on children and cancelling them with negative margins.

```css
.grid {
  display: grid;
  gap: 1rem 2rem; /* row-gap column-gap */
}
```

### 76: What does `flex: 1` mean?

`flex` is a shorthand for `flex-grow`, `flex-shrink` and `flex-basis`. `flex: 1` means `flex: 1 1 0%`: the item can grow and shrink, and it starts from a size of zero. So all items with `flex: 1` end up the same size.

```css
.sidebar { flex: 0 0 250px; } /* fixed 250px, no growing or shrinking */
.content { flex: 1; }         /* takes the rest */
```

Other common values: `flex: auto` is `1 1 auto` and `flex: none` is `0 0 auto`.

### 77: What does the `order` property do?

It changes the visual order of flex or grid items. Items with lower values come first, and the default is 0.

```css
.featured { order: -1; } /* moves to the front */
```

It only changes what you see, not the tab order or what screen readers read. Use it carefully, or keyboard users will jump around the page in a confusing way.

### 78: What does `align-self` do?

It overrides `align-items` for one item.

```css
.container { display: flex; align-items: flex-start; }
.special { align-self: center; }
```

### 79: How do you center an element both ways with Flexbox?

```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
```

Another option is `margin: auto` on the child, which also centers it inside a flex container.

### 80: What is CSS Grid?

Grid (`display: grid`) is a two-dimensional layout system. You define rows and columns, then place items into them. Flexbox works in one direction at a time, while grid controls both directions together.

```css
.page {
  display: grid;
  grid-template-columns: 200px 1fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}
```

### 81: What is the `fr` unit?

`fr` means a fraction of the free space in the grid container. Free space is what's left after fixed tracks and gaps are taken out.

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr; /* the middle column is twice as wide */
}
```

### 82: What does `repeat()` do in grid?

It saves you from writing the same track size many times.

```css
grid-template-columns: repeat(4, 1fr);
grid-template-columns: repeat(2, 100px 1fr); /* 100px 1fr 100px 1fr */
grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
```

The last line is a responsive grid with no media queries. It fits as many 200px-or-wider columns as it can.

### 83: How do you make a grid item span several columns or rows?

Use `grid-column` and `grid-row` with line numbers or `span`.

```css
.hero { grid-column: 1 / -1; }   /* from the first line to the last line */
.wide { grid-column: span 2; }   /* two columns wide */
.tall { grid-row: 2 / 4; }       /* from row line 2 to row line 4 */
```

Negative numbers count from the end, but only for lines of the explicit grid.

### 84: What is `grid-template-areas`?

It lets you name areas of the grid and draw the layout with text.

```css
.page {
  display: grid;
  grid-template-columns: 200px 1fr;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
}
.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }
.footer  { grid-area: footer; }
```

Each area has to be a rectangle. A `.` marks an empty cell.

### 85: When should you use Flexbox and when should you use Grid?

- **Flexbox**: one-direction layouts where the content decides the sizes. Examples are navbars, toolbars, button groups and centering one thing.
- **Grid**: two-direction layouts where you want rows and columns to line up. Examples are page layouts, card grids and forms.

They work well together. It's common to use grid for the page and flexbox inside each component.

### 86: What is the shortest way to center something with Grid?

```css
.parent {
  display: grid;
  place-items: center;
}
```

`place-items` is a shorthand for `align-items` and `justify-items`.

### 87: What are media queries?

Media queries apply styles only when a condition is true, usually about the viewport size or a user setting.

```css
.grid { grid-template-columns: 1fr; }

@media (min-width: 768px) {
  .grid { grid-template-columns: repeat(2, 1fr); }
}

@media (width >= 1024px) {   /* newer range syntax */
  .grid { grid-template-columns: repeat(3, 1fr); }
}
```

### 88: What is mobile-first CSS?

You write the base styles for small screens first, then add `min-width` media queries for bigger screens. Small screens get the simplest CSS, and you build up from there. This is usually easier than starting with the desktop layout and undoing it for mobile with `max-width` queries.

### 89: Why do you need the viewport meta tag?

Without it, mobile browsers pretend the screen is about 980px wide and zoom the page out, and your media queries don't fire as you'd expect.

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

This tag is HTML, but responsive CSS doesn't work properly on phones without it.

### 90: How do you make images responsive with CSS?

```css
img {
  max-width: 100%;
  height: auto;
  display: block;
}
```

`max-width: 100%` stops images from growing wider than their container, and `height: auto` keeps the aspect ratio. `display: block` removes the small gap under inline images.

### 91: What is a CSS transition?

A transition animates a property smoothly from one value to another when the value changes, for example on hover.

```css
.button {
  background: royalblue;
  transition: background-color 200ms ease;
}
.button:hover {
  background: darkblue;
}
```

### 92: What are the parts of the `transition` shorthand?

`transition: property duration timing-function delay;`

```css
.card {
  transition:
    transform 300ms ease-out,
    box-shadow 300ms ease-out 50ms;
}
```

Timing functions include `ease` (default), `linear`, `ease-in`, `ease-out`, `ease-in-out`, `cubic-bezier()`, `steps()` and `linear()`. Avoid `transition: all`. It can animate things you didn't mean to and costs more.

### 93: What does `transform` do?

It moves, rotates, scales or skews an element visually without affecting the layout around it.

```css
.a { transform: translate(10px, 20px); }
.b { transform: rotate(45deg); }
.c { transform: scale(1.1); }
.d { transform: translateX(-50%) rotate(10deg); }
```

There are also separate properties for each one: `translate`, `rotate` and `scale`.

### 94: How do you create a CSS animation?

Define the steps with `@keyframes`, then apply them with `animation`.

```css
@keyframes fade-in {
  from { opacity: 0; transform: translateY(10px); }
  to   { opacity: 1; transform: translateY(0); }
}

.toast {
  animation: fade-in 300ms ease-out;
}
```

You can use percentages for more steps: `0%`, `50%`, `100%`.

### 95: What are the animation properties?

```css
.spinner {
  animation-name: spin;
  animation-duration: 1s;
  animation-timing-function: linear;
  animation-delay: 0s;
  animation-iteration-count: infinite;
  animation-direction: normal;   /* reverse, alternate, alternate-reverse */
  animation-fill-mode: none;     /* forwards, backwards, both */
  animation-play-state: running; /* paused */
}

/* shorthand */
.spinner { animation: spin 1s linear infinite; }
```

### 96: What is the difference between `:hover`, `:focus` and `:active`?

- `:hover`: the pointer is over the element.
- `:focus`: the element has keyboard focus, or was clicked if it's focusable.
- `:active`: the element is being pressed (mouse button held down).

On touch screens `:hover` acts strangely, so don't hide important content behind it.

### 97: What is `:focus-visible`?

`:focus-visible` matches when the browser decides a focus ring should be shown. That's usually when the user is using the keyboard, not when they click with a mouse. You get clear focus styles for keyboard users without rings showing on every mouse click.

```css
button:focus-visible {
  outline: 3px solid orange;
  outline-offset: 2px;
}
```

### 98: How do `:first-child`, `:last-child` and `:nth-child()` work?

```css
li:first-child { font-weight: bold; }
li:last-child  { border-bottom: none; }
li:nth-child(2) { color: red; }          /* the 2nd one */
tr:nth-child(odd) { background: #f5f5f5; } /* 1st, 3rd, 5th... */
li:nth-child(3n) { color: blue; }        /* every 3rd one */
li:nth-child(n + 4) { opacity: 0.5; }    /* 4th and after */
li:nth-last-child(-n + 2) { color: gray; } /* the last two */
```

They count from 1, not 0. They count all sibling elements, whatever their type.

### 99: What is the difference between `:nth-child()` and `:nth-of-type()`?

`:nth-child()` counts all sibling elements. `:nth-of-type()` only counts siblings with the same tag.

```html
<div>
  <h2>Title</h2>
  <p>First</p>
  <p>Second</p>
</div>
```

`p:nth-child(1)` matches nothing, because the first child is the `h2`. `p:nth-of-type(1)` matches "First".

### 100: What does `:not()` do?

It matches elements that don't match the selector inside it. It accepts a list of selectors.

```css
li:not(:last-child) { border-bottom: 1px solid #ddd; }
button:not(.primary, .danger) { background: gray; }
```

Its specificity is the specificity of the most specific selector inside it.

### 101: How do `::before` and `::after` work?

They add a pseudo-element as the first or last child of an element. They only appear if you set `content`, even if it's empty.

```css
.quote::before {
  content: "\201C"; /* left double quote */
  font-size: 2em;
}
.link-external::after {
  content: "";
  display: inline-block;
  width: 0.8em;
  height: 0.8em;
  background: url("external.svg") no-repeat center / contain;
}
```

They don't work on replaced elements like `<img>` and `<input>`, because those can't have children.

### 102: What do `::placeholder` and `::selection` do?

- `::placeholder` styles the placeholder text in an input.
- `::selection` styles the text the user has highlighted.

```css
input::placeholder { color: #999; }
::selection { background: #ffe066; color: #000; }
```

`::selection` only supports a few properties, such as `color`, `background-color`, `text-decoration` and `text-shadow`.

### 103: What are `::first-letter` and `::first-line`?

They style the first letter or the first rendered line of a block of text. They're used for drop caps and lead-in styles.

```css
article p:first-of-type::first-letter {
  float: left;
  font-size: 3em;
  line-height: 1;
  margin-right: 0.1em;
}
```

The `initial-letter` property is made for drop caps, but browser support is still uneven.

### 104: What are CSS custom properties?

Custom properties (often called CSS variables) are values you define once and reuse. Their names start with `--`, and you read them with `var()`.

```css
:root {
  --brand: #4f46e5;
  --radius: 8px;
}
.button {
  background: var(--brand);
  border-radius: var(--radius);
}
```

Unlike Sass variables, they exist while the page runs. They inherit, they can be changed in media queries or by JavaScript, and they follow the cascade.

### 105: How do you give `var()` a fallback value?

Put it after a comma:

```css
.card {
  padding: var(--card-padding, 1rem);
  color: var(--text, var(--fallback-text, black));
}
```

The fallback is used when the variable isn't defined. It isn't used when the variable is defined with a value that's wrong for the property.

### 106: What does `calc()` do?

`calc()` does math with values, and it can mix units.

```css
.sidebar { width: calc(100% - 250px); }
.title { font-size: calc(1rem + 1vw); }
.item { margin: calc(var(--space) * 2); }
```

You need spaces around `+` and `-`. `calc(100%-10px)` is invalid.

### 107: What are `min()`, `max()` and `clamp()`?

- `min(a, b)` uses the smaller value.
- `max(a, b)` uses the larger value.
- `clamp(min, preferred, max)` uses the preferred value but keeps it between the two limits.

```css
.container { width: min(100% - 2rem, 1200px); }
h1 { font-size: clamp(1.75rem, 4vw + 1rem, 3rem); }
.button { padding-inline: max(1rem, 2vw); }
```

You can do math inside them without writing `calc()`.

### 108: How do you write comments in CSS?

Only with `/* ... */`. They can span several lines. `//` is not a comment in plain CSS (it is in Sass and Less), and using it can break the next rule.

```css
/* Main navigation */
.nav { display: flex; }
```

### 109: What is the difference between a CSS reset and Normalize.css?

- A **reset** (like the old Eric Meyer reset) removes most browser default styles, such as margins, heading sizes and list bullets, so you start from almost nothing.
- **Normalize.css** keeps useful defaults and only fixes the differences between browsers.

Many projects today use a small "modern reset" that does a bit of both: sets `box-sizing`, removes default margins, makes images block-level, and makes form controls inherit fonts.

### 110: What is the user agent stylesheet?

It's the browser's built-in default CSS. It's why `h1` is big and bold, `ul` has bullets and padding, and `body` has an 8px margin. Your styles override it because author styles come later in the cascade. You can see these defaults in DevTools, marked as "user agent stylesheet".

### 111: How do you style lists?

```css
ul.clean {
  list-style: none;
  margin: 0;
  padding: 0;
}
ol.roman { list-style-type: upper-roman; }
ul.check { list-style-image: url("check.svg"); }
li::marker { color: tomato; }
```

Safari doesn't announce lists to VoiceOver as lists when you remove `list-style`. If that matters, add `role="list"` to the `ul`.

### 112: In what order should you write link states?

Use the "LoVe HAte" order: `:link`, `:visited`, `:hover`, `:active`. These selectors all have the same specificity, so the later one wins. If `:hover` came before `:visited`, visited links wouldn't change on hover.

```css
a:link    { color: blue; }
a:visited { color: purple; }
a:hover   { color: darkblue; }
a:active  { color: red; }
```

Add `:focus-visible` next to `:hover` for keyboard users.

### 113: What does the `cursor` property do?

It changes the mouse pointer shown over an element.

```css
button { cursor: pointer; }
.disabled { cursor: not-allowed; }
.draggable { cursor: grab; }
.resizer { cursor: col-resize; }
```

### 114: What does `object-fit` do?

It controls how an `<img>` or `<video>` fills its box when the box has a different shape from the media.

```css
.thumb {
  width: 200px;
  height: 200px;
  object-fit: cover;          /* fill and crop */
  object-position: top center; /* which part to keep */
}
```

The values are `fill` (default, stretches), `contain`, `cover`, `none` and `scale-down`.

### 115: What does `aspect-ratio` do?

It keeps an element at a set width-to-height ratio.

```css
.video { width: 100%; aspect-ratio: 16 / 9; }
.square { aspect-ratio: 1; }
```

Before this property existed, people used the "padding-top: 56.25%" hack. Browsers also use the `width` and `height` attributes on `<img>` to set an aspect ratio before the image loads, which stops layout from jumping.

### 116: What does `vertical-align` do?

It aligns inline, inline-block and table-cell elements vertically. It does nothing on block elements or flex items.

```css
img.icon { vertical-align: middle; }
td { vertical-align: top; }
```

Common values: `baseline` (default), `middle`, `top`, `bottom`, `text-top`, `text-bottom`, or a length.

### 117: What does `pointer-events: none` do?

The element stops reacting to the mouse and touch. Clicks go through it to whatever is underneath. It's useful for decorative overlays.

```css
.overlay-decoration { pointer-events: none; }
```

It doesn't stop keyboard focus. A button with `pointer-events: none` can still be reached with Tab and pressed with Enter. To really disable something, use the `disabled` attribute or `inert`.

### 118: What does `display: inline-flex` do?

The element is a flex container for its children, but on the outside it acts like an inline element and sits in a line of text. It's common for buttons with an icon and a label.

```css
.button {
  display: inline-flex;
  align-items: center;
  gap: 0.5em;
}
```

`inline-grid` works the same way for grid.

### 119: How do you style tables?

```css
table {
  border-collapse: collapse; /* single borders between cells */
  width: 100%;
}
th, td {
  border: 1px solid #ddd;
  padding: 0.5rem 0.75rem;
  text-align: left;
}
tbody tr:nth-child(even) { background: #f9f9f9; }
caption { caption-side: bottom; }
```

With `border-collapse: separate` (the default), each cell has its own borders, and `border-spacing` sets the gap between them.

### 120: Why does `opacity` on a parent affect its children?

`opacity` applies to the element after it has been drawn, together with everything inside it. A child can't be less transparent than its parent, even with `opacity: 1`. If you only want a see-through background, use an alpha color on the background.

### 121: What is a CSS preprocessor?

A preprocessor is a language that compiles to CSS and adds features plain CSS used to lack. Sass (SCSS syntax) is the most used one, and Less is the other well-known one. Common features: variables, nesting, mixins, functions, loops and splitting code into partial files.

Native CSS now has custom properties, nesting and math functions, so many projects need a preprocessor less than before. Sass is still common in existing codebases.

### 122: What are vendor prefixes?

Vendor prefixes like `-webkit-`, `-moz-` and `-ms-` were how browsers shipped experimental features before they were finished.

```css
.text {
  -webkit-background-clip: text;
  background-clip: text;
}
```

Browsers now ship new features behind flags instead. You rarely write prefixes by hand. A tool like Autoprefixer (a PostCSS plugin) adds them based on the browsers you support.

### 123: What is PostCSS?

PostCSS is a tool that runs CSS through JavaScript plugins. Common plugins:

- **Autoprefixer**: adds vendor prefixes.
- **postcss-preset-env**: turns newer CSS syntax into code older browsers understand.
- **cssnano**: minifies CSS.

Many frameworks and build tools (Vite, Next.js, Tailwind) use PostCSS or a similar tool such as Lightning CSS.

### 124: What is a CSS framework?

A CSS framework is a ready-made set of styles and components. Examples:

- **Bootstrap**: prebuilt components like buttons, navbars and modals.
- **Tailwind CSS**: small utility classes that you combine in your HTML.
- **Bulma**, **Foundation**: other component frameworks.

They speed up development and keep things consistent, but you add their size and their opinions to your project.

### 125: Why should you avoid `@import` in plain CSS?

`@import` inside a CSS file makes the browser download that file, find the import, and only then start downloading the imported file. Each level adds another round trip, and CSS blocks rendering, so the page appears later.

Use several `<link>` tags, or let a bundler join the files at build time. `@import` is fine when a build tool processes it, and `@import ... layer()` is useful for cascade layers.

### 126: What is the difference between `<link>` and `@import`?

`<link>` lives in the HTML, and the browser can find it and start downloading it right away, in parallel with other files. `@import` lives inside a CSS file, so the browser only finds it after downloading and parsing that file. `@import` rules must also come before all other rules, apart from `@charset` and `@layer` statements.

### 127: What is a common mistake with shorthand properties?

A shorthand resets every sub-property you leave out back to its initial value.

```css
.box {
  background-color: red;
  background: url("bg.png"); /* background-color is reset to transparent */
}
```

The same happens with `font`, `border`, `margin`, `animation` and others. If you only want to change one part, use the longhand property, like `background-image`.

### 128: What is the "62.5%" trick, and should you use it?

Setting `html { font-size: 62.5%; }` makes `1rem` equal 10px, which makes the math easy (`1.6rem` = 16px). The downside is that every component must set its font size again, and third-party components may come out tiny. Many teams now leave the root at 100% and just do the math, or use a design token scale. Never set the root size in `px`. That ignores the user's browser font setting.

### 129: What does the `inset` property do?

`inset` is a shorthand for `top`, `right`, `bottom` and `left`. It uses the same 1-4 value pattern as margin.

```css
.overlay {
  position: absolute;
  inset: 0; /* fills the positioned parent */
}
```

### 130: What are logical properties?

Logical properties describe sides by text direction instead of by screen direction. They switch automatically for right-to-left languages and vertical writing.

| Physical | Logical |
|---|---|
| `margin-left` / `margin-right` | `margin-inline-start` / `margin-inline-end` |
| `margin-top` / `margin-bottom` | `margin-block-start` / `margin-block-end` |
| `width` / `height` | `inline-size` / `block-size` |
| `text-align: left` | `text-align: start` |

```css
.card { padding-inline: 1rem; margin-block: 2rem; }
```

### 131: How do you detect dark mode in CSS?

Use the `prefers-color-scheme` media feature.

```css
:root {
  --bg: white;
  --text: #111;
}
@media (prefers-color-scheme: dark) {
  :root {
    --bg: #111;
    --text: #eee;
  }
}
body { background: var(--bg); color: var(--text); }
```

Also set `color-scheme: light dark;` on `:root`, so scrollbars and form controls switch too.

### 132: What is `prefers-reduced-motion`?

It's a media feature that tells you the user asked the OS for less motion. Motion can make some people dizzy or sick.

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

Reducing motion doesn't mean removing every effect. A fade is usually fine. Big slides, zooms and parallax are what cause problems.

### 133: How do you write print styles?

Use `@media print`, or a stylesheet with `media="print"`.

```css
@media print {
  nav, .ads, button { display: none; }
  body { color: black; background: white; }
  a[href]::after { content: " (" attr(href) ")"; }
  h2 { break-after: avoid; }
}
```

You can test it in DevTools by emulating print media in the Rendering panel.

### 134: What is a CSS sprite?

A sprite is one image file that holds many small icons. Each icon is shown with `background-position`. It was used to cut down HTTP requests in the HTTP/1.1 days.

```css
.icon { background: url("sprite.png") no-repeat; width: 16px; height: 16px; }
.icon-home { background-position: 0 0; }
.icon-user { background-position: -16px 0; }
```

Today it's mostly legacy. People use inline SVG, SVG sprites with `<use>`, or icon components, and HTTP/2 makes many small requests cheap.

### 135: What does the `resize` property do?

It lets the user resize an element by dragging its corner. It only works when `overflow` is not `visible`.

```css
textarea { resize: vertical; }   /* none, both, horizontal, vertical */
.panel { overflow: auto; resize: both; }
```

### 136: What does `scroll-behavior: smooth` do?

It makes the page scroll smoothly instead of jumping when you click an anchor link or call `scrollTo()` from JavaScript.

```css
html { scroll-behavior: smooth; }

@media (prefers-reduced-motion: reduce) {
  html { scroll-behavior: auto; }
}
```

### 137: What does `accent-color` do?

It sets the main color of built-in form controls like checkboxes, radio buttons, range sliders and progress bars, with no custom markup.

```css
:root { accent-color: #4f46e5; }
```

The browser picks a check mark color that has enough contrast with your accent.

### 138: Which pseudo-classes help style form states?

```css
input:checked + label { font-weight: bold; }
input:disabled { opacity: 0.5; }
input:enabled { background: white; }
input:required { border-left: 3px solid orange; }
input:optional { border-left: none; }
input:read-only { background: #f3f3f3; }
input:indeterminate { outline: 2px dashed gray; }
```

`:indeterminate` matches a checkbox whose `indeterminate` property was set by JavaScript. It also matches a radio group with nothing selected.

### 139: What is the difference between `:invalid` and `:user-invalid`?

`:invalid` matches as soon as the field fails validation, even before the user types anything, so a required field shows red on page load. `:user-invalid` only matches after the user has interacted with the field (or tried to submit the form), which is what you usually want.

```css
input:user-invalid { border-color: crimson; }
input:user-valid { border-color: seagreen; }
```

### 140: What is `::marker`?

It styles the bullet or number of a list item, and also the triangle on a `<summary>`.

```css
li::marker {
  color: #4f46e5;
  font-weight: bold;
}
ol li::marker { content: counter(list-item) ") "; }
```

Only a few properties work on it, like `color`, `content` and font properties.

### 141: What does the `filter` property do?

It applies image effects to an element and everything inside it.

```css
img.gray { filter: grayscale(100%); }
.blurred { filter: blur(4px); }
.icon:hover { filter: brightness(1.2) drop-shadow(0 2px 4px rgb(0 0 0 / 30%)); }
```

Other functions: `contrast()`, `saturate()`, `sepia()`, `invert()`, `hue-rotate()` and `opacity()`. Any filter other than `none` creates a stacking context.

### 142: What does `backdrop-filter` do?

It applies filters to whatever is behind the element. It's used for the "frosted glass" look. The element needs a background that is at least partly transparent, or you won't see anything behind it.

```css
.glass {
  background: rgb(255 255 255 / 30%);
  backdrop-filter: blur(12px);
}
```

It can be slow on large areas or weak devices.

### 143: What is the difference between `overflow-wrap` and `word-break`?

- `overflow-wrap: break-word` (old name `word-wrap`) breaks a long word only when it wouldn't fit on a line by itself.
- `overflow-wrap: anywhere` is the same, but the possible breaks also count when the browser works out min-content size. It helps inside flex items and tables.
- `word-break: break-all` breaks between any two characters, even when the word would fit on the next line.

For long URLs or ids in user content, `overflow-wrap: anywhere` is usually the right choice.

### 144: What does `text-wrap: balance` do?

It makes the browser spread text evenly across lines, so a heading doesn't end with one lonely word on the last line.

```css
h1, h2, h3 { text-wrap: balance; }
p { text-wrap: pretty; }
```

`balance` only works on short blocks (a few lines), because it's costly. `pretty` mainly avoids a single word on the last line of a paragraph. Browsers that don't support these values just ignore them.

### 145: How do you limit text to a set number of lines?

```css
.excerpt {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
  overflow: hidden;
}
```

The prefixed version is the one that works everywhere today. The spec also defines an unprefixed `line-clamp` property, but support for it is still incomplete, so keep the prefixed form.

### 146: Why doesn't `height: 100%` work?

A percentage height only works if the parent has a defined height. If the parent's height is `auto` (decided by its content), the percentage can't be worked out and acts like `auto`. You'd have to set heights all the way up to `html` and `body`.

Better options are viewport units (`min-height: 100dvh`), flexbox or grid with stretching, or `height: 100%` inside a grid or flex item that already has a definite size.

### 147: What does `display: contents` do?

It makes the element's own box disappear, and its children act as if they were children of its parent. It's useful when a wrapper element gets in the way of a flex or grid layout.

```css
.wrapper { display: contents; }
```

Old browser versions had bugs where `display: contents` removed the element's meaning for screen readers, for example on buttons and lists. Most of these are fixed, but don't use it on interactive elements.

### 148: How do you debug CSS in the browser?

Open DevTools (F12) and use the **Elements** panel:

- **Styles** tab: every rule that matches, overridden declarations crossed out, and you can edit values live.
- **Computed** tab: the final value of each property and which rule set it.
- Box model diagram: margin, border, padding and content sizes.
- Flex and grid badges that draw overlays on the layout.
- `:hov` button to force `:hover`, `:focus` and other states.

### 149: What happens when a browser sees CSS it doesn't understand?

It ignores that declaration or rule and carries on. That's what makes fallbacks work:

```css
.box {
  height: 100vh;   /* old browsers use this */
  height: 100dvh;  /* new browsers override it */
}
```

If one selector in a normal selector list is invalid, the whole rule is dropped. If a syntax error isn't closed properly, like a missing `}`, it can break the rules that come after it.

### 150: How do you check if a CSS feature is safe to use?

- **MDN**: every property page has a browser compatibility table.
- **caniuse.com**: support tables and usage numbers.
- **Baseline**: a label on MDN and web.dev. "Newly available" means it works in all major browsers. "Widely available" means it has worked everywhere for at least 30 months.

For features that aren't Baseline yet, use `@supports` or make sure the page still works without them.

## Moderate Questions (151-300)

### 151: How does specificity work for `:is()`, `:where()`, `:not()` and `:has()`?

- `:is()`, `:not()` and `:has()` take the specificity of the most specific selector inside them.
- `:where()` always has zero specificity, whatever is inside it.

```css
:is(#main, .content) p { }   /* (1,0,1): the #main counts even when .content matched */
:where(#main, .content) p { } /* (0,0,1) */
```

The first case surprises people. Having one id in the list makes the whole `:is()` as strong as an id, even for elements that only matched the class.

### 152: When would you use `:where()` instead of `:is()`?

Use `:where()` when you want styles that are easy to override, like resets, base styles and library defaults. Any single class can beat them.

```css
:where(ul, ol)[role="list"] { list-style: none; padding: 0; }
:where(.btn) { padding: 0.5rem 1rem; }
```

Use `:is()` when you just want shorter selectors and normal specificity:

```css
:is(h1, h2, h3):hover { color: navy; }
```

### 153: What is `:has()` and what can you do with it?

`:has()` matches an element if it contains something that matches the selector inside it. People call it the "parent selector", but it can also look at following siblings.

```css
/* card that contains an image */
.card:has(img) { grid-template-rows: 200px 1fr; }

/* label before an invalid input */
label:has(+ input:user-invalid) { color: crimson; }

/* form with a checked checkbox */
form:has(input[name="agree"]:checked) button { opacity: 1; }

/* lock page scroll while a modal is open */
html:has(dialog[open]) { overflow: hidden; }
```

It's Baseline since late 2023. A lot of things that used to need JavaScript to toggle classes can now be done with it.

### 154: How does native CSS nesting work?

You can write rules inside other rules, like in Sass.

```css
.card {
  padding: 1rem;

  & h2 { font-size: 1.25rem; }
  .meta { color: gray; }          /* & is optional before a class */
  &:hover { box-shadow: 0 4px 12px rgb(0 0 0 / 10%); }
  &.featured { border-color: gold; }

  @media (width >= 768px) {
    padding: 2rem;
  }
}
```

`&` stands for the parent selector. You need it when you're attaching something directly to the parent, like `&:hover` or `&.featured`.

### 155: How is native nesting different from Sass nesting?

1. **No string joining.** Sass lets you write `&__title` to make `.card__title`. Native CSS doesn't. `&__title` doesn't work.
2. **Specificity.** Native nesting works like wrapping the parent in `:is()`. So `.a, #b { & .c {} }` becomes `:is(.a, #b) .c`, which has id-level specificity even for `.a .c`.
3. **It runs in the browser.** Nothing is compiled, so the rules stay nested in the CSSOM.

Early versions of the spec required `&` or a symbol before type selectors (`& h2`). Current browsers accept `h2` alone.

### 156: What are cascade layers (`@layer`)?

Cascade layers let you group styles into named layers and decide which layers win. Layer order beats specificity: a simple selector in a later layer wins over a very specific selector in an earlier one.

```css
@layer reset, base, components, utilities;

@layer reset {
  * { margin: 0; }
}
@layer components {
  #sidebar .nav a { color: gray; } /* high specificity */
}
@layer utilities {
  .text-red { color: red; }        /* still wins, because the layer is later */
}
```

### 157: How do layer order and unlayered styles work?

- Layers are ordered by when their names first appear. The usual practice is to list all of them at the top with one `@layer a, b, c;` statement.
- Styles that aren't in any layer go into an implicit last layer, so they **beat all named layers**.
- Nested layers are written with dots: `@layer framework.base`.

That's why you often put third-party CSS in a layer. Then your unlayered styles override it with no specificity battles.

### 158: How does `!important` behave inside cascade layers?

It flips. For normal declarations, later layers win. For `!important` declarations, **earlier layers win**, and important declarations in layers beat important unlayered ones.

This lets a reset layer at the start mark something as `!important`, and nothing later can override it by accident. It's surprising, so most teams avoid `!important` inside layers.

### 159: How do you put third-party CSS into a cascade layer?

Use `@import` with `layer()`:

```css
@layer vendor, base, components;

@import url("bootstrap.min.css") layer(vendor);
@import url("theme.css") layer(base) supports(display: grid) screen;
```

In HTML there's no `layer` attribute on `<link>` yet, so people use `@import` in a CSS file, or a bundler that wraps the file in `@layer vendor { ... }`.

### 160: What is the full order of the cascade?

From the highest priority to the lowest:

1. Transitions (while running).
2. `!important` from the browser (user agent).
3. `!important` from the user.
4. `!important` from the author (the page). Inside this, earlier layers beat later layers, and inline `!important` beats both.
5. Animations (`@keyframes`).
6. Normal author styles. Inline styles, then unlayered, then later layers before earlier layers. After that, specificity, then source order.
7. Normal user styles.
8. Normal browser styles.

`@scope` proximity sits between specificity and source order.

### 161: What is a stacking context, and what creates one?

A stacking context is a group of elements that get layered together. `z-index` values inside it only compete with each other. Then the whole group is placed as one unit inside its parent's context.

Things that create one:

- The root element.
- `position: absolute` or `relative` with a `z-index` other than `auto`.
- `position: fixed` or `sticky`.
- A flex or grid item with a `z-index` other than `auto`.
- `opacity` below 1.
- `transform`, `filter`, `backdrop-filter`, `perspective`, `clip-path`, `mask` (any value other than `none`).
- `isolation: isolate`.
- `mix-blend-mode` other than `normal`.
- `will-change` naming any of the above.
- `contain: layout` or `paint`, and `container-type: size` or `inline-size`.
- Elements in the top layer (open modal dialogs, popovers).

### 162: A dropdown shows up behind another section even with `z-index: 9999`. How do you fix it?

The dropdown is probably inside a parent that creates a stacking context, and that parent sits below the other section.

1. In DevTools, walk up from the dropdown and look for ancestors with `transform`, `opacity`, `filter`, `z-index` + `position`, and so on.
2. Raise the `z-index` of that ancestor in its own context, or remove the property that creates the context if you don't need it.
3. Or render the dropdown outside that tree. Today the best fix is often the `popover` attribute, which puts the element in the top layer, above everything, with no `z-index` needed.

### 163: What is a block formatting context (BFC)?

A BFC is an area where block boxes are laid out on their own, separate from the outside. Inside a BFC:

- Floats are contained, so the parent wraps around its floated children.
- Margins of children don't collapse with margins outside the BFC.
- The BFC box doesn't overlap floats next to it.

### 164: How do you create a block formatting context?

The cleanest way is `display: flow-root`. It exists only for this purpose and has no side effects. Other things that create a BFC: `overflow` other than `visible` or `clip`, `float`, `position: absolute` or `fixed`, `display: inline-block`, flex and grid items, table cells, `contain: layout` and multi-column containers.

```css
.media { display: flow-root; } /* contains floated image, no clearfix */
```

### 165: What is the containing block of an absolutely positioned element?

It's the padding box of the nearest ancestor that:

- has `position` other than `static`, **or**
- has `transform`, `translate`, `rotate`, `scale`, `perspective`, `filter`, `backdrop-filter`, `contain: layout` or `paint`, or `will-change` for one of those.

If none exists, it's the initial containing block (viewport sized, at the top of the document). For `position: fixed`, it's the viewport, unless one of those transform-like properties is on an ancestor.

### 166: Why might `position: sticky` not work?

Common reasons:

1. No offset is set (`top`, `bottom`, etc.).
2. An ancestor has `overflow: hidden`, `auto` or `scroll`, so the element sticks inside that ancestor instead of the page. Often that ancestor doesn't scroll, so nothing happens. `overflow: clip` doesn't cause this.
3. The parent is the same height as the sticky element, so there's no room to stick.
4. In a flex or grid container, the item is stretched to full height. Add `align-self: start`.
5. It's inside a `<table>` part that doesn't support it in some older browsers.

### 167: Why does a `position: fixed` modal scroll with the page?

An ancestor probably has `transform`, `filter`, `backdrop-filter`, `perspective`, `contain: paint`, or `will-change: transform`. Any of these makes that ancestor the containing block for fixed children, so "fixed" now means fixed to that ancestor.

Fix: move the modal to be a direct child of `body`, remove the property from the ancestor, or use `<dialog>` with `showModal()`. That puts it in the top layer, which isn't affected.

### 168: What is the difference between `flex-basis` and `width`?

`flex-basis` sets the starting size of a flex item along the main axis, before growing and shrinking. In a row it acts like width, and in a column like height. If both are set, `flex-basis` wins (unless it's `auto`, which then uses `width`).

`min-width` and `max-width` still limit the final size. `flex-basis: 0` with `flex-grow` gives equal sizes, while `flex-basis: auto` starts from the content size.

### 169: How do `flex-grow` and `flex-shrink` split the space?

**Growing:** the free space is shared in proportion to `flex-grow`. Free space is the container size minus the bases of all items.

```css
/* container 600px, items with basis 100px each = 300px free */
.a { flex: 1 1 100px; } /* gets 100 + 100 = 200px */
.b { flex: 2 1 100px; } /* gets 100 + 200 = 300px */
.c { flex: 0 1 100px; } /* stays 100px */
```

**Shrinking:** the overflow is shared in proportion to `flex-shrink × flex-basis`. That way bigger items shrink more in pixels. No item shrinks below its minimum size (see `min-width: auto`).

### 170: Why does a flex item overflow instead of shrinking?

Flex items have `min-width: auto` by default. That means they can't get smaller than their content, like a long word, a wide table, or a `pre` block. Set `min-width: 0` (or `overflow: hidden`) on the item so it can shrink.

```css
.content {
  flex: 1;
  min-width: 0; /* now text-overflow: ellipsis inside it works */
}
```

In a column flex container, the same thing happens with `min-height`.

### 171: What is the difference between `align-items` and `align-content`?

- `align-items` aligns items **inside each line** on the cross axis.
- `align-content` aligns **the lines themselves** in the container. It only does something when there's more than one line (`flex-wrap: wrap`) and extra space on the cross axis.

In grid, `align-content` moves the whole grid inside the container when the tracks are smaller than the container. In newer browsers, `align-content` also works on normal block elements, so you can center content vertically with no flex or grid.

### 172: How does `margin: auto` work in a flex container?

Auto margins take all the free space on their side, before `justify-content` runs. It's a simple way to push one item away from the others.

```css
.nav { display: flex; gap: 1rem; }
.nav .logout { margin-left: auto; } /* pushed to the far right */
```

`margin: auto` on a single child centers it both ways.

### 173: How do you make a sticky footer layout?

The footer stays at the bottom when the content is short, and is pushed down when it's long.

```css
body {
  min-height: 100svh;
  display: grid;
  grid-template-rows: auto 1fr auto; /* header, main, footer */
}
```

Flexbox version:

```css
body { min-height: 100svh; display: flex; flex-direction: column; }
main { flex: 1; }
```

### 174: How do you build the "holy grail" layout?

A header, a footer, and three columns (left sidebar, main, right sidebar) that turn into one column on mobile.

```css
.layout {
  display: grid;
  min-height: 100svh;
  grid-template:
    "header" auto
    "main" 1fr
    "left" auto
    "right" auto
    "footer" auto / 1fr;
}

@media (width >= 900px) {
  .layout {
    grid-template:
      "header header header" auto
      "left   main   right" 1fr
      "footer footer footer" auto
      / 200px 1fr 200px;
  }
}

header { grid-area: header; }
main   { grid-area: main; }
.left  { grid-area: left; }
.right { grid-area: right; }
footer { grid-area: footer; }
```

### 175: What is the difference between `auto-fill` and `auto-fit`?

Both create as many columns as fit. The difference shows when there are fewer items than columns:

- `auto-fill` keeps the empty columns, so items stay at their minimum size.
- `auto-fit` collapses the empty columns to zero, so the items stretch to fill the row.

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(250px, 100%), 1fr));
  gap: 1rem;
}
```

`min(250px, 100%)` stops overflow when the container is narrower than 250px.

### 176: What is the implicit grid?

The explicit grid is the tracks you define with `grid-template-*`. When items are placed outside it, or there are more items than cells, the browser adds implicit tracks. Their size comes from `grid-auto-rows` and `grid-auto-columns`, which default to `auto`.

```css
.gallery {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 200px; /* every new row is 200px */
}
```

### 177: What does `grid-auto-flow: dense` do?

By default, grid places items in order and leaves holes when a big item doesn't fit. `dense` goes back and fills those holes with smaller items that come later.

```css
.gallery {
  grid-auto-flow: row dense;
}
```

The visual order no longer matches the HTML order, which can confuse keyboard users. Use it for image galleries, not for content where order matters.

### 178: How do named grid lines work?

You can name lines in the track list and place items by name.

```css
.page {
  display: grid;
  grid-template-columns:
    [full-start] 1fr
    [content-start] minmax(0, 60rem) [content-end]
    1fr [full-end];
}
.page > * { grid-column: content; }
.page > .full-bleed { grid-column: full; }
```

Lines named `x-start` and `x-end` create an implicit area called `x`. That's why `grid-column: content` works.

### 179: What is subgrid?

`subgrid` lets a nested grid use its parent's tracks instead of making its own. Items in different cards can then line up with each other.

```css
.cards {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
.card {
  display: grid;
  grid-row: span 3;
  grid-template-rows: subgrid; /* title, body and footer line up across cards */
}
```

It has been Baseline since 2023. The child also inherits the parent's line names and gaps, but it can set its own `gap`.

### 180: Why use `minmax(0, 1fr)` instead of `1fr`?

`1fr` really means `minmax(auto, 1fr)`. The `auto` minimum means a column can't shrink below its content's min-content width. A long URL or a wide code block can then make a "1fr" column wider than the others and cause overflow.

```css
grid-template-columns: repeat(3, minmax(0, 1fr)); /* truly equal columns */
```

### 181: What is the difference between `justify-items` and `justify-content` in grid?

- `justify-items` aligns each item **inside its own cell** along the inline axis.
- `justify-content` aligns **the whole grid** inside the container when the tracks don't fill it.

The same goes for `align-items` and `align-content` on the block axis. `justify-self` and `align-self` override alignment for one item.

### 182: How do you stack items on top of each other with grid?

Put them in the same cell. You don't need absolute positioning, and the container still grows to fit the bigger item.

```css
.hero {
  display: grid;
}
.hero > * {
  grid-area: 1 / 1;
}
.hero img { z-index: 0; }
.hero .text { z-index: 1; align-self: end; }
```

### 183: What are container queries?

Container queries change styles based on the size of a parent container instead of the viewport. A component can then adapt to wherever it's placed, like a sidebar or a main column.

```css
.card-wrapper {
  container-type: inline-size;
}

@container (width > 400px) {
  .card {
    display: grid;
    grid-template-columns: 150px 1fr;
  }
}
```

Size container queries have been Baseline since 2023.

### 184: What do the `container-type` values mean?

- `inline-size`: you can query the width (inline size). This is the one you'll use most.
- `size`: you can query width and height. The element must then have a height that doesn't depend on its children, or it collapses.
- `normal` (default): no size queries, but style queries (and scroll-state queries in Chromium) still work.

Setting `inline-size` or `size` adds size containment. The container's size can't depend on its children in that axis, which is what stops infinite loops.

### 185: What are container query units?

They are like viewport units, but relative to the nearest size container:

- `cqw` / `cqh`: 1% of the container's width / height.
- `cqi` / `cqb`: 1% of the inline / block size.
- `cqmin` / `cqmax`: the smaller / larger one.

```css
.card-wrapper { container-type: inline-size; }
.card h2 { font-size: clamp(1rem, 5cqi, 2rem); }
```

If there's no container, they fall back to the small viewport units.

### 186: How do you name a container and query a specific one?

```css
.sidebar {
  container-type: inline-size;
  container-name: sidebar;
  /* shorthand: container: sidebar / inline-size; */
}

@container sidebar (width < 300px) {
  .widget { font-size: 0.875rem; }
}
```

Without a name, `@container` queries the nearest ancestor container of any name.

### 187: What are container style queries?

They check the computed value of a custom property on the container, not its size.

```css
.card { --variant: default; }
.card.featured { --variant: featured; }

@container style(--variant: featured) {
  .card-title { color: gold; }
}
```

Every element is a style container by default, so no `container-type` is needed. Querying custom properties works in Chromium and Safari, and Firefox support has been in progress. Check current support before relying on it. Querying normal properties like `style(display: flex)` isn't supported anywhere yet.

### 188: What is the media query range syntax?

It lets you write comparisons instead of `min-` and `max-`.

```css
/* old */
@media (min-width: 600px) and (max-width: 899.98px) { }

/* new */
@media (600px <= width < 900px) { }
```

It's clearer and avoids the `899.98px` trick that prevented overlap at the boundary. It's Baseline since 2023 and also works in `@container`.

### 189: How do you make font sizes scale with the screen?

Use `clamp()` with a `rem` + viewport mix as the middle value.

```css
h1 {
  font-size: clamp(2rem, 1.5rem + 2.5vw, 3.5rem);
}
```

The `rem` part makes sure browser zoom still works. A value that only uses `vw` doesn't grow when the user zooms, which is an accessibility problem (WCAG 1.4.4). Tools like Utopia generate these values for a whole type scale.

### 190: How do you serve different background images for different screen densities?

Use `image-set()`:

```css
.hero {
  background-image: image-set(
    url("hero.avif") type("image/avif") 1x,
    url("hero@2x.avif") type("image/avif") 2x,
    url("hero.jpg") 1x
  );
}
```

The browser picks the best image for the screen's pixel density and skips formats it can't decode. For `<img>` elements, use `srcset` and `<picture>` in HTML.

### 191: How do you choose breakpoints?

Pick breakpoints where **your content** starts to look wrong, not at popular device widths. Start narrow, widen the browser, and add a breakpoint where the layout breaks. Most projects end up with 3-5 breakpoints. Keep them in one place, like Sass variables, design tokens or `@custom-media` if your build tool supports it. Plain custom properties can't be used inside media query conditions.

For components, container queries often replace page-level breakpoints.

### 192: What are the `hover` and `pointer` media features?

They describe the user's input device:

- `hover: hover`: the main input can hover (like a mouse).
- `hover: none`: it can't (like a touchscreen).
- `pointer: fine`: precise pointer (mouse). `pointer: coarse`: imprecise (finger).
- `any-hover` / `any-pointer`: whether **any** input device has that ability.

```css
@media (hover: hover) {
  .card:hover { transform: translateY(-4px); }
}
@media (pointer: coarse) {
  .button { min-height: 44px; }
}
```

This stops "sticky hover" on phones, where a tapped element stays in its hover state.

### 193: How do logical properties help with right-to-left languages?

When `dir="rtl"` is set, `inline-start` becomes the right side. If you write `margin-inline-start` instead of `margin-left`, the layout flips with no extra CSS.

```css
.icon { margin-inline-end: 0.5rem; }
.quote { border-inline-start: 4px solid; padding-inline-start: 1rem; }
.menu { inset-inline-end: 0; }
```

Flexbox and grid already follow text direction. Mostly, you need to update spacing, borders, positioning and `text-align`.

### 194: What is the `:dir()` pseudo-class?

`:dir(rtl)` and `:dir(ltr)` match elements based on their text direction, including direction inherited from parents. `[dir="rtl"]` only matches the element that has the attribute.

```css
.arrow:dir(rtl) { transform: scaleX(-1); }
```

It's Baseline since 2023.

### 195: What does `writing-mode` do?

It sets whether lines of text run horizontally or vertically, and which way blocks stack.

```css
.vertical-label {
  writing-mode: vertical-rl;  /* top to bottom, lines go right to left */
}
.sideways { writing-mode: sideways-lr; } /* newer, rotates the text */
```

It's used for Chinese and Japanese text, and for vertical labels on tables or charts. Logical properties follow `writing-mode`, so `inline-size` becomes the height in vertical mode.

### 196: How do custom properties work with the cascade and inheritance?

Custom properties are normal properties that inherit by default. You can set one on `:root` for global use, or on any element to override it for that part of the page.

```css
:root { --gap: 1rem; }
.compact { --gap: 0.5rem; }
.stack > * + * { margin-top: var(--gap); }
```

Everything inside `.compact` gets the smaller gap. This makes them good for component variants and themes.

### 197: What is the difference between custom properties and Sass variables?

| Sass variables | Custom properties |
|---|---|
| Replaced at build time and gone from the output | Live in the browser |
| Can't change with media queries or JS | Can change in media queries, on hover, or from JS |
| No cascade or inheritance | Follow the cascade and inherit |
| Work in selectors and media query conditions | Only work in property values |

Many teams use both: Sass for build-time values like breakpoints, and custom properties for theme values that change at runtime.

### 198: How do you read and set custom properties from JavaScript?

```js
const root = document.documentElement;

// set
root.style.setProperty("--brand", "#e11d48");

// read the value that applies
const brand = getComputedStyle(root).getPropertyValue("--brand").trim();

// set on one element
card.style.setProperty("--progress", "75%");
```

A common pattern is passing mouse position or scroll progress to CSS with variables, and doing the visual work in CSS.

### 199: What is `@property`?

`@property` registers a custom property with a type, an initial value and an inheritance setting.

```css
@property --angle {
  syntax: "<angle>";
  inherits: false;
  initial-value: 0deg;
}
```

Because the browser knows its type, it can **animate** it, and invalid values fall back to `initial-value`. Without registration, custom properties are just strings and can't be animated smoothly. It's Baseline since mid-2024.

### 200: How do you animate a gradient?

Gradients can't be animated directly. You can animate a registered custom property that the gradient uses:

```css
@property --angle {
  syntax: "<angle>";
  inherits: false;
  initial-value: 0deg;
}

.border-glow {
  background: conic-gradient(from var(--angle), #4f46e5, #ec4899, #4f46e5);
  animation: rotate 3s linear infinite;
}

@keyframes rotate {
  to { --angle: 360deg; }
}
```

### 201: What does "invalid at computed-value time" mean?

When a `var()` gives a value that doesn't work for the property, the browser has already thrown away other declarations for that property during the cascade. So it can't fall back to an earlier rule. Instead, the property becomes `unset`: it inherits if it's an inherited property, otherwise it uses its initial value.

```css
:root { --size: red; }
p {
  font-size: 20px;
  font-size: var(--size); /* invalid, so font-size becomes unset (inherited), not 20px */
}
```

That's why fallbacks for custom properties work differently from normal fallbacks. Registering the property with `@property` makes it use its `initial-value` instead.

### 202: How do you build a theme system with custom properties?

Define semantic tokens once, then change their values for each theme.

```css
:root {
  --color-bg: #fff;
  --color-text: #111;
  --color-primary: #4f46e5;
}

[data-theme="dark"] {
  --color-bg: #0b0b0f;
  --color-text: #e5e5e5;
  --color-primary: #818cf8;
}

body {
  background: var(--color-bg);
  color: var(--color-text);
}
```

Components only use semantic names (`--color-primary`), never raw colors. Adding a new theme then means adding one block of overrides.

### 203: What do `color-scheme` and `light-dark()` do?

`color-scheme` tells the browser which color schemes the page supports. Browser UI like scrollbars, form controls and the default background then match. `light-dark()` picks one of two values based on the active scheme.

```css
:root {
  color-scheme: light dark;
}
body {
  background: light-dark(#ffffff, #121212);
  color: light-dark(#111111, #eeeeee);
}

/* force one theme for part of the page */
.always-dark { color-scheme: dark; }
```

`light-dark()` is Baseline since 2024. It only works with colors (newer specs extend it to images). It follows the `color-scheme` of the element, so you can let users switch themes by changing `color-scheme` instead of copying all your variables.

### 204: What does `color-mix()` do?

It mixes two colors in a color space you choose.

```css
.button {
  --base: #4f46e5;
  background: var(--base);
}
.button:hover {
  background: color-mix(in oklch, var(--base), black 15%);
}
.subtle {
  background: color-mix(in srgb, var(--base) 20%, transparent);
}
```

It's an easy way to make hover states, tints and see-through versions from one base color, with no preprocessor.

### 205: What is relative color syntax?

It lets you take an existing color and change its channels.

```css
:root { --brand: #4f46e5; }

.light  { background: oklch(from var(--brand) calc(l + 0.2) c h); }
.faded  { background: rgb(from var(--brand) r g b / 50%); }
.shift  { background: hsl(from var(--brand) calc(h + 30) s l); }
```

The `from` keyword breaks the color into channels (`l`, `c`, `h` or `r`, `g`, `b`), and you can use `calc()` on each one. It's in all major browsers since 2024.

### 206: Why is OKLCH popular for colors?

OKLCH (Lightness, Chroma, Hue) is "perceptually uniform". If two colors have the same `L` value, they look equally bright to people. In HSL that isn't true: yellow at 50% lightness looks much brighter than blue at 50%.

This makes it easier to build color scales, keep contrast predictable, and create shades by only changing `L`. OKLCH can also describe colors outside sRGB for wide-gamut screens. Tailwind v4 and many design systems now define their palettes in OKLCH.

### 207: How do you use wide-gamut colors?

Modern screens can show colors outside sRGB (Display P3). You can use them with `color(display-p3 ...)` or `oklch()`, and keep a fallback for older screens.

```css
.cta {
  background: #ff3366;
}
@media (color-gamut: p3) {
  .cta { background: color(display-p3 1 0.15 0.4); }
}
```

If a screen can't show a color, the browser maps it to the nearest color it can show.

### 208: How do you animate an element to and from `display: none`?

Use `transition-behavior: allow-discrete` (or `allow-discrete` in the `transition` shorthand) and `@starting-style` for the starting values when the element appears.

```css
.toast {
  opacity: 1;
  translate: 0 0;
  transition:
    opacity 300ms,
    translate 300ms,
    display 300ms allow-discrete;
}

.toast[hidden] {
  display: none;
  opacity: 0;
  translate: 0 1rem;
}

@starting-style {
  .toast {
    opacity: 0;
    translate: 0 1rem;
  }
}
```

`allow-discrete` keeps the element visible (`display: block`) until the exit transition ends. `@starting-style` gives the "before" values for the first frame after it appears. For popovers and dialogs, also transition `overlay` with `allow-discrete` so they stay in the top layer while they animate out.

### 209: How do you animate to `height: auto`?

The traditional way is a grid trick, which works everywhere:

```css
.panel {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows 300ms;
}
.panel.open { grid-template-rows: 1fr; }
.panel > .inner { overflow: hidden; }
```

The newer way is `interpolate-size: allow-keywords`, which lets the browser animate between a length and `auto`. Right now it only works in Chromium browsers. Others just jump with no animation, which is a fine fallback.

```css
:root { interpolate-size: allow-keywords; }
details::details-content {
  height: 0;
  overflow: clip;
  transition: height 300ms, content-visibility 300ms allow-discrete;
}
details[open]::details-content { height: auto; }
```

### 210: What does `animation-fill-mode` do?

It decides what styles apply before the animation starts and after it ends.

- `none`: the keyframe styles only apply while the animation runs.
- `forwards`: keeps the last keyframe after it ends.
- `backwards`: applies the first keyframe during `animation-delay`.
- `both`: does both.

```css
.fade-in {
  opacity: 0;
  animation: fade 500ms 200ms forwards;
}
@keyframes fade { to { opacity: 1; } }
```

### 211: How do you pause or reverse an animation with CSS?

```css
.spinner { animation: spin 1s linear infinite; }
.spinner:hover { animation-play-state: paused; }

.pulse { animation: pulse 1s ease-in-out infinite alternate; }
```

`animation-direction: alternate` plays forward, then backward. `reverse` always plays backward. To restart an animation from JavaScript, remove the animation, force a reflow, and set it again. Or use the Web Animations API, which has `play()`, `pause()` and `reverse()`.

### 212: What are `cubic-bezier()`, `steps()` and `linear()`?

They are timing functions.

```css
.a { transition-timing-function: cubic-bezier(0.34, 1.56, 0.64, 1); } /* overshoot */
.b { animation-timing-function: steps(8); }  /* sprite sheet, 8 frames */
.c { transition-timing-function: linear(0, 0.6 20%, 1.1 50%, 0.95 70%, 1); } /* bounce-ish */
```

- `cubic-bezier()` sets a curve with two control points. Values above 1 overshoot.
- `steps()` jumps between values with no smoothing.
- `linear()` (Baseline 2023) draws a curve from many points, which makes spring and bounce effects possible in pure CSS.

### 213: Which properties are cheap to animate?

`transform` (and `translate`, `rotate`, `scale`) and `opacity`. Browsers can animate them on the compositor, often on the GPU, without doing layout or paint again for each frame. `filter` is often fine too.

Animating `width`, `height`, `top`, `left`, `margin` or `padding` triggers layout on every frame. `background-color`, `box-shadow` and `color` trigger repaint. Those can drop frames, especially on weak phones.

```css
/* bad */
.panel { transition: left 300ms; }
/* good */
.panel { transition: transform 300ms; }
```

### 214: What are the individual transform properties?

`translate`, `rotate` and `scale` are separate properties. You can change one without overwriting the others.

```css
.card {
  translate: 0 0;
  scale: 1;
  transition: translate 200ms, scale 200ms;
}
.card:hover { translate: 0 -4px; }
.card:active { scale: 0.98; } /* hover translate stays */
```

They are applied in a fixed order: `translate`, then `rotate`, then `scale`, then the `transform` property.

### 215: Why does the order of transform functions matter?

Each function changes the coordinate system for the ones after it.

```css
.a { transform: translateX(100px) rotate(45deg); } /* moves right, then rotates in place */
.b { transform: rotate(45deg) translateX(100px); } /* rotates, then moves along the rotated axis (diagonally) */
```

`transform-origin` sets the point that rotate and scale use. The default is the center: `transform-origin: top left;` rotates around the corner.

### 216: How do 3D transforms work?

You need `perspective` for depth, either on the parent or as a function.

```css
.scene { perspective: 800px; }
.card {
  transform-style: preserve-3d;
  transition: transform 600ms;
}
.scene:hover .card { transform: rotateY(180deg); }
.card .back {
  transform: rotateY(180deg);
  backface-visibility: hidden;
}
.card .front { backface-visibility: hidden; }
```

A smaller `perspective` value gives a stronger 3D effect. `preserve-3d` lets children keep their 3D position instead of being flattened.

### 217: What is `will-change` and when should you use it?

`will-change` tells the browser that a property is going to change, so it can prepare, usually by giving the element its own compositor layer.

```css
.drawer { will-change: transform; }
```

Use it only on a few elements that you know will animate, and ideally add it just before the animation and remove it after. Putting it on many elements uses a lot of GPU memory and can make things slower. It also creates a stacking context and a containing block for fixed children.

### 218: How does scroll snapping work?

```css
.carousel {
  display: flex;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  scroll-padding-inline: 1rem;
}
.carousel > .slide {
  flex: 0 0 80%;
  scroll-snap-align: center;
  scroll-snap-stop: always; /* don't skip past slides on a fast swipe */
}
```

`mandatory` always snaps to a point. `proximity` only snaps when the scroll stops close to one. It gives you a carousel with no JavaScript.

### 219: What does `overscroll-behavior` do?

It controls what happens when you reach the end of a scroll area. By default, scrolling "chains" to the parent, so scrolling past the end of a modal starts scrolling the page behind it.

```css
.modal-body { overscroll-behavior: contain; } /* no chaining */
body { overscroll-behavior-y: none; }         /* also no pull-to-refresh or bounce */
```

### 220: How do you stop a fixed header from covering anchor targets?

Use `scroll-margin-top` on the targets or `scroll-padding-top` on the scroll container.

```css
html { scroll-padding-top: 80px; } /* height of the sticky header */

/* or per element */
h2[id] { scroll-margin-top: 80px; }
```

These offsets also work for focus scrolling and `scrollIntoView()`.

### 221: How do you style scrollbars?

The standard properties:

```css
.panel {
  scrollbar-width: thin;              /* auto, thin, none */
  scrollbar-color: #888 transparent;  /* thumb track */
  scrollbar-gutter: stable;           /* reserve space so layout doesn't jump */
}
```

The old `::-webkit-scrollbar` pseudo-elements still work in Safari and in Chromium when the standard properties aren't set. They give more control, but they're non-standard. `scrollbar-width: none` hides the scrollbar but keeps scrolling, so make sure users can still tell the area scrolls.

### 222: How do you make a table header stick when you scroll?

```css
.table-wrapper {
  max-height: 400px;
  overflow: auto;
}
thead th {
  position: sticky;
  top: 0;
  background: white; /* otherwise rows show through */
  z-index: 1;
}
/* sticky first column */
tbody th {
  position: sticky;
  left: 0;
  background: white;
}
```

Put `position: sticky` on the `th` cells, not on `thead` or `tr`, for the best browser support.

### 223: How do you hide something visually but keep it for screen readers?

Use a "visually hidden" class:

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip-path: inset(50%);
  white-space: nowrap;
  border: 0;
}
```

`display: none` and `visibility: hidden` hide it from screen readers too. Use this for things like labels on icon buttons or a "skip to content" link. That link usually becomes visible on `:focus`.

### 224: How do you write good focus styles?

```css
:focus-visible {
  outline: 3px solid #4f46e5;
  outline-offset: 2px;
}
:focus:not(:focus-visible) { outline: none; } /* rarely needed now */
```

Rules of thumb:

- Never remove outlines without putting something visible in their place.
- Make sure the focus ring has enough contrast with the background (WCAG asks for 3:1).
- An outline is better than `box-shadow`. Windows High Contrast (forced colors) mode removes shadows but keeps outlines.
- `:focus-within` can highlight a whole form group when any field inside it has focus.

### 225: What is forced colors mode?

It's an accessibility mode (like Windows High Contrast) where the OS replaces your colors with a small user-chosen palette. Backgrounds, borders and text get system colors. Shadows and background images may be removed.

```css
@media (forced-colors: active) {
  .button {
    border: 1px solid ButtonText; /* make sure the button has an edge */
  }
  .icon { forced-color-adjust: auto; }
}
```

Test it in DevTools by emulating forced colors. Common bugs are buttons with no visible border and focus states made only with `box-shadow`.

### 226: How do you handle `prefers-reduced-motion` properly?

There are two approaches:

```css
/* opt-out: animate by default, tone down when asked */
@media (prefers-reduced-motion: reduce) {
  .hero { animation: none; }
}

/* opt-in: only animate when the user hasn't asked for less motion */
@media (prefers-reduced-motion: no-preference) {
  .hero { animation: slide-in 600ms ease-out; }
}
```

The opt-in version is safer, because new animations don't run by accident. Swap big movements for fades instead of removing all feedback. In JavaScript, check `matchMedia("(prefers-reduced-motion: reduce)")`.

### 227: Is content from `::before` and `::after` read by screen readers?

Yes, most screen readers read generated `content`. So don't put important information only in CSS content, and don't use decorative characters without care. You can give alternative text after a slash:

```css
.new::before {
  content: "★" / "New";       /* read as "New" */
}
.decoration::after {
  content: "→" / "";          /* not read at all */
}
```

Generated content can't be selected or copied, and it disappears if CSS fails to load.

### 228: What is BEM?

BEM (Block, Element, Modifier) is a class naming convention:

```css
.card { }                 /* block */
.card__title { }          /* element: part of the block */
.card__button { }
.card--featured { }       /* modifier: a variant */
.card__button--disabled { }
```

Everything is a single class, so specificity stays flat and low. Class names show where the styles live and what they belong to, which helps on big teams. The downsides are long class names, and nested elements can get awkward.

### 229: What are OOCSS, SMACSS and ITCSS?

They are older but still used ways to organise CSS:

- **OOCSS** (Object-Oriented CSS): keep structure separate from skin, and container separate from content. Build reusable "objects" like `.media`.
- **SMACSS**: sort rules into base, layout, module, state and theme.
- **ITCSS** (Inverted Triangle): order styles from generic and low specificity (settings, tools, generic, elements) to specific (objects, components, utilities).

Cascade layers now give native support for the ITCSS idea of ordering styles by how specific they are.

### 230: What is utility-first CSS?

You style with many small single-purpose classes (`flex`, `p-4`, `text-sm`) instead of writing a component class for each thing. Tailwind CSS is the main example.

Pros: no naming, CSS stops growing after a point, changes stay local, and design constraints come from the scale. Cons: long class lists in markup, a learning curve, and you need components (in React, Vue, etc.) to avoid repeating the same class lists.

### 231: What are CSS Modules?

CSS Modules is a build-time feature where every class name in a `.module.css` file is made unique per file. Styles can't leak or clash.

```css
/* Button.module.css */
.button { padding: 0.5rem 1rem; }
.primary { background: royalblue; }
```

```jsx
import styles from "./Button.module.css";

export function Button({ children }) {
  return <button className={`${styles.button} ${styles.primary}`}>{children}</button>;
}
```

It's plain CSS with local scope, and it's supported out of the box by Vite, Next.js and most bundlers.

### 232: What are the pros and cons of CSS-in-JS?

CSS-in-JS (styled-components, Emotion) writes styles in JavaScript next to components.

**Pros:** scoped styles, dynamic styles based on props, styles live next to the component, and unused styles go away with the component.

**Cons (runtime libraries):** style work at runtime adds JavaScript and slows rendering. It also plays badly with React Server Components and streaming, which is why many teams have moved away from it.

**Zero-runtime** options (vanilla-extract, Linaria, Panda CSS, StyleX) keep the authoring style but output static CSS at build time.

### 233: How does Shadow DOM isolate styles?

Styles inside a shadow root only apply inside it, and outside styles don't reach in. There are a few exceptions:

- **Inherited properties** (like `color` and `font-family`) still inherit through the shadow boundary.
- **Custom properties** also inherit, which makes them the main way to theme web components.
- `::part()` and `::slotted()` let outside and inside styles reach specific nodes.

This is why web components are usually themed with CSS variables.

### 234: What do `:host`, `::slotted()` and `::part()` do?

```css
/* inside the component's shadow root */
:host { display: block; border: 1px solid; }
:host([variant="primary"]) { border-color: blue; }
:host(:hover) { background: #f5f5f5; }
::slotted(p) { margin: 0; }   /* only top-level slotted elements */
```

```css
/* outside, in page CSS */
my-card::part(title) { font-size: 1.5rem; }
```

```html
<!-- in the shadow template -->
<h2 part="title"><slot name="title"></slot></h2>
```

`::part()` is how a component decides which internal parts the page is allowed to style.

### 235: What is `@scope`?

`@scope` limits styles to a part of the DOM, with an optional lower limit.

```css
@scope (.card) {
  img { border-radius: 8px; }
  :scope { padding: 1rem; } /* the .card itself */
}

/* "donut" scope: stop at .content */
@scope (.tabs) to (.tab-content) {
  a { color: inherit; }
}
```

It also adds **scoping proximity** to the cascade: when two scoped rules have the same specificity, the one whose scope root is closer to the element wins. It's in Chromium and Safari, and Firefox shipped it more recently. Check support before you depend on it.

### 236: Which Sass features do teams use most?

```scss
@use "sass:math";
@use "tokens" as t;

$breakpoint-md: 768px;

@mixin respond($bp) {
  @media (min-width: $bp) { @content; }
}

.card {
  padding: t.$space-4;
  width: math.div(100%, 3);

  &__title { font-size: 1.25rem; }  // .card__title (string joining)

  @include respond($breakpoint-md) {
    padding: t.$space-6;
  }
}

%visually-hidden { position: absolute; clip-path: inset(50%); }
.sr-only { @extend %visually-hidden; }
```

Variables, mixins, partials with `@use`, BEM string joining with `&__`, functions and loops are the main reasons people still use Sass.

### 237: What is the difference between `@use` and `@import` in Sass?

`@import` in Sass is deprecated and will be removed in Dart Sass 3.0. Its problems: every import put all variables into one global scope, the same file could be included many times, and it was hard to tell where a variable came from.

`@use` loads a file once, gives it a namespace, and keeps private members (names that start with `-` or `_`) private.

```scss
@use "colors";            // colors.$primary
@use "spacing" as s;      // s.$md
@forward "buttons";       // re-export from an index file
```

Sass ships a migrator tool (`sass-migrator module`) for moving old code over.

### 238: What do PostCSS and Lightning CSS do in a build?

They take your CSS and transform it:

- Add vendor prefixes based on your `browserslist`.
- Turn modern syntax (nesting, `oklch()`, media range syntax) into code older browsers understand.
- Bundle `@import`s and minify.
- Handle CSS Modules.

Lightning CSS is written in Rust and does most of this in one fast tool. Vite can use it, and Tailwind v4 uses it internally. PostCSS is the plugin-based JavaScript option with a large ecosystem.

### 239: What is `@supports`?

`@supports` applies styles only if the browser supports a property and value, or a selector.

```css
.layout { display: flex; flex-wrap: wrap; }

@supports (display: grid) {
  .layout { display: grid; grid-template-columns: repeat(3, 1fr); }
}

@supports not (backdrop-filter: blur(1px)) {
  .glass { background: rgb(255 255 255 / 90%); }
}

@supports selector(:has(a)) {
  .card:has(img) { padding: 0; }
}
```

### 240: What is progressive enhancement in CSS?

You write a baseline that works everywhere, then add features for browsers that support them. CSS makes this easy because browsers ignore what they don't understand.

```css
.card {
  background: #4f46e5;                                  /* everyone */
  background: oklch(55% 0.2 270);                       /* modern browsers */
}
.cards { display: flex; flex-wrap: wrap; }
@supports (grid-template-rows: subgrid) {
  .cards { display: grid; /* ... */ }
}
```

The page doesn't need to look the same in every browser. It needs to work in every browser.

### 241: What does `font-display` do?

It controls what happens while a web font is loading.

- `swap`: show the fallback font right away, then swap when the web font loads. Text is never hidden.
- `block`: hide text for up to about 3 seconds, then show the fallback.
- `fallback`: hide text for about 100ms, then use the fallback. Swap only if the font loads within about 3 seconds.
- `optional`: about 100ms of hidden text. If the font isn't ready by then, keep the fallback for this page view. This means no late swaps and no layout shift.
- `auto`: the browser decides, usually like `block`.

`swap` is common for body text and `optional` is great for avoiding layout shift.

### 242: What are FOUT and FOIT, and how do you reduce them?

- **FOIT** (Flash Of Invisible Text): text is hidden until the font loads.
- **FOUT** (Flash Of Unstyled Text): text shows in the fallback font, then switches.

To reduce both:

- Preload important fonts: `<link rel="preload" href="/fonts/inter.woff2" as="font" type="font/woff2" crossorigin>`.
- Self-host WOFF2 files and subset them (fewer characters, smaller files).
- Use `font-display: swap` or `optional`.
- Adjust the fallback font's metrics to match the web font, so the swap doesn't move text around.

### 243: What are variable fonts?

A variable font is one font file that contains a whole range of styles along "axes" like weight, width and slant, instead of a separate file for each style.

```css
@font-face {
  font-family: "Inter";
  src: url("Inter-Variable.woff2") format("woff2");
  font-weight: 100 900;
}

h1 { font-weight: 750; }   /* any value in the range */
.custom { font-variation-settings: "wght" 650, "opsz" 32; }
```

You get fewer files, smooth animation between weights, and in-between values like 550.

### 244: How do you stop layout shift when a web font loads?

Make the fallback font take up the same space as the web font, using `@font-face` descriptors:

```css
@font-face {
  font-family: "Inter Fallback";
  src: local("Arial");
  size-adjust: 107%;
  ascent-override: 90%;
  descent-override: 22%;
  line-gap-override: 0%;
}

body { font-family: "Inter", "Inter Fallback", sans-serif; }
```

Next.js (`next/font`), Fontaine and Capsize can work out these values for you. `font-size-adjust` is another option. It scales fallback fonts so their x-height matches.

### 245: How do you crop an image to a shape?

Use `clip-path`. Only the part inside the shape is shown, and clicks only register inside it.

```css
.avatar  { clip-path: circle(50%); }
.slanted { clip-path: polygon(0 0, 100% 0, 100% 85%, 0 100%); }
.inset   { clip-path: inset(10px round 12px); }
.custom  { clip-path: path("M0,0 L100,0 L100,80 Q50,100 0,80 Z"); }
```

`clip-path` can be animated between shapes that have the same number of points. Newer browsers also support `shape()`, which works like `path()` but accepts CSS units and percentages, so it can be responsive.

### 246: What does `mask-image` do?

It uses an image or gradient as a mask. Opaque parts of the mask show the element, and transparent parts hide it.

```css
.fade-bottom {
  mask-image: linear-gradient(to bottom, black 70%, transparent);
}
.icon {
  background: currentColor;
  mask: url("icon.svg") center / contain no-repeat;
}
```

The second example is a common trick to color an SVG icon with `currentColor` when you can't inline the SVG. Unprefixed `mask` is Baseline since late 2023.

### 247: What does `shape-outside` do?

It makes text wrap around a shape instead of the float's rectangle.

```css
.portrait {
  float: left;
  width: 200px;
  height: 200px;
  shape-outside: circle(50%);
  clip-path: circle(50%);
  margin-right: 1rem;
}
```

It only works on floats. You can also pass an image, and its alpha channel sets the shape.

### 248: What is the difference between `filter: drop-shadow()` and `box-shadow`?

`box-shadow` follows the element's box, including its `border-radius`. `drop-shadow()` follows the real visible shape, including transparent PNG and SVG edges and `clip-path` shapes. It doesn't support `spread` or `inset`.

```css
.logo { filter: drop-shadow(0 2px 4px rgb(0 0 0 / 30%)); } /* follows the logo outline */
```

### 249: What do blend modes do?

`mix-blend-mode` blends an element with whatever is behind it. `background-blend-mode` blends an element's background layers with each other.

```css
.title-on-image { mix-blend-mode: difference; color: white; }
.duotone {
  background: url("photo.jpg"), #4f46e5;
  background-blend-mode: luminosity;
}
```

The modes match the ones in Photoshop: `multiply`, `screen`, `overlay`, `difference`, `luminosity` and more.

### 250: What does `isolation: isolate` do?

It creates a new stacking context and does nothing else visually. It's used to:

- Stop `mix-blend-mode` from blending with things outside a group.
- Contain `z-index` values inside a component, so a child with `z-index: 10` can't jump above other parts of the page.

```css
.card { isolation: isolate; }
.card::before { content: ""; position: absolute; inset: 0; z-index: -1; } /* stays behind the card's content, not behind the page */
```

### 251: How do CSS counters work?

Counters let CSS number things automatically.

```css
article { counter-reset: section; }
article h2::before {
  counter-increment: section;
  content: counter(section) ". ";
}

/* nested numbering like 1.1, 1.2 */
ol { counter-reset: item; list-style: none; }
ol li::before {
  counter-increment: item;
  content: counters(item, ".") " ";
}
```

`counters()` (with an "s") joins nested counters of the same name. They're used for headings, figure numbers and custom list styles.

### 252: What does `attr()` do in `content`?

It reads an HTML attribute and inserts its value as text.

```css
a[href^="http"]::after { content: " (" attr(href) ")"; } /* print styles */
[data-tooltip]:hover::after { content: attr(data-tooltip); }
```

Traditionally it only works in `content` and only returns a string. Chromium now supports a typed `attr()` that works in any property, like `width: attr(data-width type(<length>))`, but it's not in all browsers yet.

### 253: How do you style form controls that look different in every browser?

Use `appearance: none` to remove the native look, then style them yourself.

```css
select {
  appearance: none;
  padding: 0.5rem 2rem 0.5rem 0.75rem;
  border: 1px solid #ccc;
  border-radius: 6px;
  background: url("chevron.svg") no-repeat right 0.5rem center / 1rem;
  font: inherit;
}
```

Also set `font: inherit` on buttons, inputs, selects and textareas. They don't inherit fonts by default.

### 254: How do you build a custom checkbox with CSS?

Keep the real `<input>` so keyboard and screen reader support stays. Style it with `appearance: none`.

```css
input[type="checkbox"] {
  appearance: none;
  width: 1.25rem;
  height: 1.25rem;
  border: 2px solid #555;
  border-radius: 4px;
  display: inline-grid;
  place-content: center;
}
input[type="checkbox"]::before {
  content: "";
  width: 0.65rem;
  height: 0.65rem;
  clip-path: polygon(14% 44%, 0 65%, 50% 100%, 100% 16%, 80% 0%, 43% 62%);
  background: white;
  scale: 0;
  transition: scale 120ms;
}
input[type="checkbox"]:checked {
  background: #4f46e5;
  border-color: #4f46e5;
}
input[type="checkbox"]:checked::before { scale: 1; }
input[type="checkbox"]:focus-visible { outline: 2px solid #4f46e5; outline-offset: 2px; }
```

If you only need a different color, `accent-color` is enough.

### 255: What does `field-sizing: content` do?

It makes inputs, textareas and selects grow and shrink to fit their content, with no JavaScript.

```css
textarea {
  field-sizing: content;
  min-height: 3lh;
  max-height: 12lh;
}
```

It's supported in Chromium and was added to Safari more recently. In other browsers the field just keeps its normal fixed size, which is a fine fallback.

### 256: What does `:focus-within` do?

It matches an element if the element itself or anything inside it has focus.

```css
.search:focus-within {
  border-color: #4f46e5;
  box-shadow: 0 0 0 3px rgb(79 70 229 / 25%);
}

.dropdown:focus-within .menu { display: block; } /* keeps the menu open for keyboard users */
```

### 257: How do you build a floating label with CSS only?

Use `:placeholder-shown`. The input needs a placeholder, which can be a single space.

```html
<div class="field">
  <input id="email" placeholder=" ">
  <label for="email">Email</label>
</div>
```

```css
.field { position: relative; }
.field label {
  position: absolute;
  left: 0.75rem;
  top: 0.75rem;
  transition: translate 150ms, scale 150ms;
  transform-origin: left top;
  pointer-events: none;
}
.field input:focus + label,
.field input:not(:placeholder-shown) + label {
  translate: 0 -1.25rem;
  scale: 0.8;
}
```

### 258: What do `:empty` and `:target` do?

- `:empty` matches elements with no children and no text. Whitespace still counts as content in browsers today, so `<p> </p>` doesn't match. Comments don't count.
- `:target` matches the element whose `id` is in the URL hash.

```css
.alert:empty { display: none; }

section:target {
  outline: 2px solid orange;
  scroll-margin-top: 80px;
}
```

### 259: What does `:nth-child(An+B of S)` do?

It counts only the siblings that match selector `S`.

```css
/* zebra stripes for visible rows only */
tr:nth-child(odd of :not(.hidden)) { background: #f5f5f5; }

/* the first 3 items with .featured */
li:nth-child(-n + 3 of .featured) { font-weight: bold; }
```

This is different from `.featured:nth-child(-n + 3)`, which matches `.featured` items that are among the first three of all siblings. It's Baseline since 2023.

### 260: What is `::backdrop`?

It's the layer drawn behind elements in the top layer, like a modal `<dialog>` opened with `showModal()`, a popover, or an element in fullscreen.

```css
dialog::backdrop {
  background: rgb(0 0 0 / 50%);
  backdrop-filter: blur(2px);
}
```

Before 2024, `::backdrop` didn't inherit from anything, so custom properties defined on `:root` didn't work in it. It now inherits from its element, and variables work.

### 261: How do you style popovers?

The `popover` attribute puts an element in the top layer when it opens. `:popover-open` matches it while it's showing.

```html
<button popovertarget="menu">Menu</button>
<div id="menu" popover>...</div>
```

```css
[popover] {
  margin: 0;
  padding: 1rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  opacity: 0;
  transition: opacity 200ms, overlay 200ms allow-discrete, display 200ms allow-discrete;
}
[popover]:popover-open { opacity: 1; }
@starting-style {
  [popover]:popover-open { opacity: 0; }
}
```

By default a popover is centered with `position: fixed` and `inset: 0; margin: auto`. Use anchor positioning to place it next to its button.

### 262: How do you style a `<dialog>`?

```css
dialog {
  border: none;
  border-radius: 12px;
  padding: 1.5rem;
  max-width: min(90vw, 32rem);
  box-shadow: 0 20px 50px rgb(0 0 0 / 30%);
}
dialog::backdrop { background: rgb(0 0 0 / 40%); }
dialog[open] { animation: pop 200ms ease-out; }

@keyframes pop {
  from { opacity: 0; scale: 0.95; }
}

/* stop the page behind from scrolling */
html:has(dialog[open]:modal) { overflow: hidden; }
```

`:modal` only matches dialogs opened with `showModal()`, not with `show()`.

### 263: How do you style `<details>` and `<summary>`?

```css
summary {
  cursor: pointer;
  list-style: none;            /* hide the default triangle */
}
summary::-webkit-details-marker { display: none; } /* older Safari */

summary::after { content: "+"; float: right; }
details[open] summary::after { content: "−"; }

details::details-content {   /* the content area, newer browsers */
  padding-block: 0.5rem;
}
```

`::details-content` is newer (Chromium 131+ and Safari 18.4+, check Firefox support before relying on it). It lets you style and animate the content part. Adding the same `name` attribute to several `<details>` makes them work like an accordion, where only one is open at a time.

### 264: What does `table-layout: fixed` do?

With `table-layout: fixed`, column widths come from the table's width and the first row (or `<col>` elements), not from all the content. The browser can lay out the table without reading every cell, which is faster for big tables. Column widths are also predictable and long content doesn't push columns wider.

```css
table { table-layout: fixed; width: 100%; }
th:nth-child(1) { width: 40%; }
td { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
```

### 265: How do you make text truncate inside a flex child?

The truncating element needs a width it can shrink to, and the flex item needs `min-width: 0`.

```css
.row { display: flex; gap: 0.5rem; }
.row .name {
  flex: 1;
  min-width: 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.row .actions { flex-shrink: 0; }
```

Without `min-width: 0`, the text keeps the item at its full width and the ellipsis never shows.

### 266: How does multi-column layout work?

```css
.article {
  columns: 3 250px; /* up to 3 columns, each at least 250px */
  column-gap: 2rem;
  column-rule: 1px solid #ddd;
}
.article h2 { column-span: all; }
.article figure { break-inside: avoid; }
```

It flows content like a newspaper, top to bottom and then into the next column. It's good for text and simple masonry-style card lists. The reading order goes down each column, which can feel odd on long pages.

### 267: Why is there a gap between `inline-block` elements?

The whitespace (line breaks and spaces) between the elements in the HTML is shown as a space character, just like spaces between words.

Fixes: use flexbox or grid for the parent (the best fix), remove the whitespace in the HTML, or set `font-size: 0` on the parent (a hack, and it breaks `em` units).

### 268: Why is there a small gap under an image?

Images are inline by default, so they sit on the text baseline. The space under them is room left for letters that hang below the line, like "g" and "p".

```css
img { display: block; }        /* most common fix */
img { vertical-align: middle; } /* if it must stay inline */
```

### 269: Do margins collapse inside flex and grid containers?

No. Margins of flex items and grid items never collapse, either with each other or with the container. Two items with `margin: 20px` in a column flexbox have 40px between them. This is one reason people prefer `gap` for spacing inside layouts.

### 270: What can you do with negative margins?

- Pull an element outside its container: `margin-inline: -1rem` for a full-bleed image inside a padded card.
- Overlap elements, like avatars in a stack.
- Shift an element up or left. A negative top or left margin moves the element and the content after it.
- A negative bottom or right margin pulls the following content closer.

```css
.avatars img + img { margin-left: -12px; }
```

They're valid CSS, but `gap`, grid and `translate` are often clearer.

### 271: Why is percentage padding based on width?

Both vertical and horizontal percentage padding are based on the **width** of the containing block. This makes the same percentage give the same space on all sides. It's also behind the old aspect-ratio hack:

```css
.ratio-16-9 {
  height: 0;
  padding-top: 56.25%; /* 9 / 16 */
  position: relative;
}
```

Today you'd use `aspect-ratio: 16 / 9` instead.

### 272: What do `min-content`, `max-content` and `fit-content` mean?

- `min-content`: the smallest size the content can have without overflowing. For text, that's the longest word.
- `max-content`: the size the content wants with no wrapping at all.
- `fit-content`: `max-content`, but no bigger than the available space. It's `min(max-content, max(min-content, available))`.

```css
.tag { width: fit-content; }               /* block that shrinks to its text */
.grid { grid-template-columns: max-content 1fr; } /* label column as wide as the longest label */
figure { width: min-content; }             /* caption wraps to the image width */
```

### 273: What is the difference between `overflow: hidden` and `overflow: clip`?

Both cut off overflowing content, but:

- `hidden` makes the element a scroll container. It can still be scrolled from code, it creates a block formatting context, and it breaks `position: sticky` for descendants.
- `clip` doesn't create a scroll container, can't be scrolled at all, and doesn't break sticky.

`clip` also works with `overflow-clip-margin` to let content draw a little outside the box before it's clipped. Use `clip` when you just want to hide overflow, like decorative shapes poking out.

### 274: What is the difference between `overflow-wrap: anywhere` and `break-word`?

Both break long words that don't fit. The difference is in **min-content sizing**:

- `break-word`: the browser doesn't count these possible break points when working out min-content width. A long URL can still make a flex item or table cell as wide as the URL.
- `anywhere`: the possible breaks do count, so the container can shrink and the word breaks.

```css
.comment { overflow-wrap: anywhere; }
```

### 275: How do you style SVG icons with CSS?

Inline SVG (or SVG used with `<use>`) can be styled like HTML.

```css
.icon {
  width: 1.25em;
  height: 1.25em;
  fill: currentColor;   /* matches the text color */
  stroke: none;
  vertical-align: -0.2em;
}
.button:hover .icon { fill: #4f46e5; }
```

SVG used as `<img>` or `background-image` can't be styled from the page. Use a CSS `mask` with `currentColor` if you need to color those.

### 276: How do you add a dark mode toggle that respects the OS setting?

Default to the OS setting, and let a class or attribute override it. With `light-dark()`, you only need to change `color-scheme`:

```css
:root {
  color-scheme: light dark;   /* follow the OS by default */
  --bg: light-dark(#fff, #111);
  --text: light-dark(#111, #eee);
}
:root[data-theme="light"] { color-scheme: light; }
:root[data-theme="dark"] { color-scheme: dark; }

body { background: var(--bg); color: var(--text); }
```

```js
document.documentElement.dataset.theme = "dark";
localStorage.setItem("theme", "dark");
```

Set the attribute with a small inline script in `<head>` before the page renders, or the page will flash the wrong theme.

### 277: What is `prefers-contrast`?

It tells you whether the user asked the OS for more (or less) contrast.

```css
@media (prefers-contrast: more) {
  :root {
    --text-muted: #333;   /* instead of a light gray */
    --border: #000;
  }
  .button { border: 2px solid; }
}
```

It's different from `forced-colors`. With `prefers-contrast`, your colors are still used, so it's up to you to make them stronger.

### 278: How do you control page breaks when printing?

```css
@page {
  size: A4;
  margin: 2cm;
}
h2, h3 { break-after: avoid; }       /* don't leave a heading alone at the bottom */
figure, table, pre { break-inside: avoid; }
.chapter { break-before: page; }
p { orphans: 3; widows: 3; }
```

The older names `page-break-before`, `page-break-after` and `page-break-inside` still work as aliases.

### 279: How do you avoid specificity wars?

- Style with classes only. Avoid ids and long descendant chains.
- Keep selectors short and flat, as in BEM (`.card__title`, not `.sidebar .card h2`).
- Use `:where()` for base and library styles so they're easy to override.
- Use cascade layers so order, not specificity, decides which group wins.
- Keep `!important` for utilities and override cases you can't avoid.
- Scope component styles (CSS Modules, `@scope`, Shadow DOM) so they don't need to fight global ones.

### 280: What does `all: unset` do, and how is it different from `all: revert`?

`all` resets every property (except `direction` and `unicode-bidi`) at once.

- `all: unset`: inherited properties inherit, everything else goes to its initial value. A `button` becomes `display: inline` with no padding or border, and loses its focus ring.
- `all: revert`: goes back to the browser's default styles, so a `div` stays `display: block`.
- `all: revert-layer`: goes back to the value from the previous cascade layer.

```css
.reset-button {
  all: unset;
  cursor: pointer;
}
.reset-button:focus-visible { outline: 2px solid; } /* add focus back */
```

### 281: What does `revert-layer` do?

It rolls a property back to the value it would have from the previous cascade layer, or from earlier origins if there is no previous layer.

```css
@layer base, theme;

@layer base {
  a { color: blue; }
}
@layer theme {
  a { color: purple; }
  .nav a { color: revert-layer; } /* back to blue from base */
}
```

It's useful in theme or component layers when you want "whatever the layer below says" for some cases.

### 282: How do you find out why a style isn't applied?

1. Inspect the element in DevTools. Make sure the selector matches it.
2. In the **Styles** pane, look for your rule. If it's crossed out, something else wins. Hover the specificity tooltip, or check the layer, to see why.
3. A yellow warning triangle means the value is invalid. An "inactive" hint (Chrome and Firefox) means the property does nothing in this context, like `width` on an inline element or `justify-content` without flex.
4. Check the **Computed** tab to see the final value and click through to the rule that set it.
5. Check that the stylesheet loaded (Network tab) and that a media or container query isn't excluding it.

### 283: How do you debug layout problems?

- Add a temporary outline to everything to see boxes without changing layout:

```css
* { outline: 1px solid rgb(255 0 0 / 30%); }
```

- Use the flexbox and grid overlays in DevTools, which show tracks, gaps and line numbers.
- Look at the box model diagram to spot unexpected margins.
- Find overflow by checking which element is wider than the viewport, or use the "scroll" badge in Chrome's Elements panel.
- Turn properties on and off with their checkboxes to see which one causes the problem.

### 284: How do you chain fallbacks for custom properties?

`var()` can have another `var()` as its fallback, so you can build component-level defaults:

```css
.button {
  background: var(--button-bg, var(--color-primary, #4f46e5));
  padding: var(--button-padding, 0.5rem 1rem);
}

.hero .button { --button-bg: white; }
```

The component exposes `--button-*` variables. Then it falls back to theme tokens, then to a hard-coded value. Note that the fallback can contain commas: everything after the first comma is the fallback.

### 285: What goes into a modern CSS reset?

```css
*, *::before, *::after { box-sizing: border-box; }
* { margin: 0; }
html { -webkit-text-size-adjust: none; text-size-adjust: none; }
body { line-height: 1.5; -webkit-font-smoothing: antialiased; }
img, picture, video, canvas, svg { display: block; max-width: 100%; }
input, button, textarea, select { font: inherit; }
p, h1, h2, h3, h4, h5, h6 { overflow-wrap: break-word; }
h1, h2, h3 { text-wrap: balance; }
#root, #__next { isolation: isolate; }
```

Each line fixes a real annoyance: the box model, default margins, images overflowing, form controls ignoring fonts, long words overflowing, and app root stacking.

### 286: How do you manage `z-index` values in a big project?

Keep a small named scale in one place instead of random numbers:

```css
:root {
  --z-dropdown: 100;
  --z-sticky: 200;
  --z-overlay: 300;
  --z-modal: 400;
  --z-toast: 500;
}
.header { position: sticky; z-index: var(--z-sticky); }
```

Use `isolation: isolate` on components so their inner `z-index` values stay local. Use the top layer (`<dialog>`, `popover`) for modals and popups, which removes most of the need for high `z-index` values.

### 287: How do you center an absolutely positioned element?

```css
.parent { position: relative; }

/* modern */
.child {
  position: absolute;
  inset: 0;
  margin: auto;
  width: fit-content;
  height: fit-content;
}

/* classic */
.child {
  position: absolute;
  top: 50%;
  left: 50%;
  translate: -50% -50%;
}
```

The `translate` version can blur text on non-integer pixel positions. The `inset: 0; margin: auto` version doesn't have that problem.

### 288: How do you make a responsive video embed?

```css
.video-embed {
  width: 100%;
  aspect-ratio: 16 / 9;
  border: 0;
}
```

Apply this to the `<iframe>` directly. For `<video>`, use `width: 100%; height: auto;`. The browser already knows its aspect ratio from the file. If you also set `width` and `height` attributes, it knows the ratio before loading.

### 289: What is `env()` and the safe area?

`env()` reads values from the browser or device. The main use is `safe-area-inset-*` for phones with notches and home indicators.

```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
```

```css
.bottom-bar {
  position: fixed;
  bottom: 0;
  padding-bottom: max(1rem, env(safe-area-inset-bottom));
}
```

Without `viewport-fit=cover`, the page doesn't go into the unsafe areas, and the values are 0.

### 290: What does `touch-action` do?

It tells the browser which touch gestures it should handle itself, like scrolling and pinch-zoom, so your JavaScript can handle the rest.

```css
.map { touch-action: none; }           /* JS handles all gestures */
.carousel { touch-action: pan-y; }     /* browser scrolls vertically, JS handles horizontal swipe */
button { touch-action: manipulation; } /* no double-tap zoom */
```

### 291: What does `user-select` do?

It controls whether the user can select text.

```css
.button, .tab { user-select: none; }  /* no accidental selection on double-click */
.code-snippet { user-select: all; }   /* one click selects everything */
```

Don't disable selection on content people may want to copy. Safari still needs `-webkit-user-select` in some versions.

### 292: What is the difference between `:root` and `html`?

In an HTML document they match the same element. The difference is specificity: `:root` is a pseudo-class (0,1,0) and `html` is a type selector (0,0,1). `:root` also works in other documents like SVG and XML. By convention, global custom properties go on `:root`.

### 293: When should you use a container query and when a media query?

- **Media queries** are for things about the page or the device: page layout, user settings (`prefers-*`), print, and input type (`hover`, `pointer`).
- **Container queries** are for reusable components that may appear in spaces of different widths, like a card that shows in a sidebar and in a main column.

A good rule: page layout uses media queries, and components use container queries.

### 294: How do you make a responsive grid of cards without media queries?

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(min(16rem, 100%), 1fr));
  gap: 1.5rem;
}
```

Columns appear and disappear on their own as space allows. `min(16rem, 100%)` stops overflow on very small screens. Use `auto-fit` if you want a few items to stretch across the full row.

### 295: How do you build a full-bleed section inside a centered layout?

The grid version, with named lines:

```css
.layout {
  display: grid;
  grid-template-columns:
    [full-start] minmax(1rem, 1fr)
    [content-start] minmax(0, 65ch) [content-end]
    minmax(1rem, 1fr) [full-end];
}
.layout > * { grid-column: content; }
.layout > .full-bleed { grid-column: full; }
```

A quick version with no grid:

```css
.full-bleed {
  width: 100vw;
  margin-inline: calc(50% - 50vw);
}
```

The second version can cause a horizontal scrollbar on desktop, because `100vw` includes the vertical scrollbar.

### 296: How do you write a "stack" layout that adds space between children?

```css
/* modern: flex with gap */
.stack {
  display: flex;
  flex-direction: column;
  gap: var(--space, 1rem);
}

/* "lobotomized owl", which also works in normal flow */
.flow > * + * {
  margin-block-start: var(--flow-space, 1em);
}
```

`* + *` matches every child except the first one, so you never get extra space at the top or bottom.

### 297: How do you make equal-height columns?

This comes for free in flexbox and grid. Items stretch to the height of the row by default (`align-items: stretch`).

```css
.columns { display: flex; gap: 1rem; }
.columns > * { flex: 1; }
```

To make the parts inside each card line up too (like titles and buttons), use `subgrid` or make each card a column flexbox with `margin-top: auto` on the footer.

### 298: How do you add a skip link?

```css
.skip-link {
  position: absolute;
  left: 1rem;
  top: -100px;
  padding: 0.5rem 1rem;
  background: #000;
  color: #fff;
  z-index: 1000;
}
.skip-link:focus { top: 1rem; }
```

```html
<a class="skip-link" href="#main">Skip to content</a>
```

It's hidden until a keyboard user tabs to it. Then it appears and lets them jump past the navigation.

### 299: How do you style based on the number of children?

Combine `:nth-child` and `:nth-last-child` (quantity queries), or use `:has()`.

```css
/* exactly 3 items */
li:first-child:nth-last-child(3),
li:first-child:nth-last-child(3) ~ li { width: 33%; }

/* grid with 5 or more items gets more columns */
.grid:has(> :nth-child(5)) {
  grid-template-columns: repeat(3, 1fr);
}
```

The `:has()` version is much easier to read.

### 300: What do `text-underline-offset` and `text-decoration-thickness` do?

They give you control over underlines without using `border-bottom`.

```css
a {
  text-decoration: underline;
  text-decoration-thickness: 2px;
  text-underline-offset: 0.2em;
  text-decoration-color: rgb(79 70 229 / 40%);
  transition: text-decoration-color 150ms;
}
a:hover { text-decoration-color: currentColor; }
```

`text-decoration-skip-ink: auto` (the default) breaks the line around letters that go below it, like "g" and "y".

## Advanced Questions (301-450)

### 301: Why is CSS render-blocking?

The browser won't paint anything until it has downloaded and parsed all the CSS in `<head>`. If it painted earlier, users would see unstyled content that then jumps around (FOUC). CSS also blocks scripts that come after it: a script might read styles with `getComputedStyle`, so the browser waits for the CSS before running it.

This is why CSS size and the number of CSS requests matter so much for First Contentful Paint and Largest Contentful Paint. Stylesheets with a `media` attribute that doesn't match (like `media="print"`) are still downloaded, but they don't block rendering.

### 302: What is the CSSOM?

The CSS Object Model is the browser's tree of parsed style rules, built from every stylesheet and `<style>` block. The browser combines the DOM and the CSSOM to work out the computed style of each element, then builds the render tree used for layout and paint.

JavaScript can read and change the CSSOM through `document.styleSheets`, `CSSStyleSheet.insertRule()`, `element.style` and `getComputedStyle()`.

### 303: Why do browsers match selectors from right to left?

The browser starts with an element and asks "which rules match this element?". It checks the rightmost part (the key selector) first, because most rules fail right there, and only walks up the tree if needed.

For `.sidebar nav ul li a`, the browser checks "is this an `a`?". Only for `a` elements does it walk up looking for `li`, `ul`, `nav` and `.sidebar`. Browsers also use rule hashing by id, class and tag, plus Bloom filters for ancestors, so most rules are skipped without being checked at all.

### 304: Does selector performance matter in practice?

Rarely, for normal pages. Modern engines are very fast at matching. Real style performance problems usually come from:

- A very large DOM (tens of thousands of elements).
- Style changes that make the browser recalculate styles for big parts of the page, like toggling a class on `body` that many selectors depend on.
- Expensive selectors that are checked again on every change, like broad `:has()` on the root, or `:nth-child` in huge lists.

Use the Performance panel to measure "Recalculate Style" time before you optimise selectors. Chrome's "selector stats" option shows which selectors cost the most.

### 305: What are the steps of the rendering pipeline?

1. **Style**: work out the computed style of every element from the DOM and the CSSOM.
2. **Layout**: work out the size and position of every box.
3. **Paint**: record draw commands for text, colors, borders, shadows and images.
4. **Composite**: split the page into layers, rasterize them (often on the GPU) and put them together on screen.

A change to a property starts the pipeline at some step and runs everything after it. Changing `width` causes layout, paint and composite. Changing `color` causes paint and composite. Changing `transform` or `opacity` on an element with its own layer can skip straight to composite.

### 306: What is layout thrashing?

Layout thrashing happens when JavaScript writes styles and then reads layout values again and again in a loop. Each read forces the browser to do layout right away (a forced synchronous layout).

```js
// bad: read and write in turns, layout runs on every loop
items.forEach((el) => {
  el.style.width = el.parentElement.offsetWidth / 2 + "px";
});

// good: read everything first, then write
const widths = items.map((el) => el.parentElement.offsetWidth);
items.forEach((el, i) => {
  el.style.width = widths[i] / 2 + "px";
});
```

Properties that force layout include `offsetWidth`, `offsetTop`, `getBoundingClientRect()`, `scrollTop` and `getComputedStyle()` for layout values. Libraries like FastDOM batch reads and writes for you. In the Performance panel, look for purple "Layout" blocks with a "Forced reflow" warning.

### 307: Which property changes trigger layout, paint or only composite?

- **Layout** (most expensive): `width`, `height`, `margin`, `padding`, `top`/`left`, `font-size`, `display`, `position`, `border-width`, and changing text content.
- **Paint only**: `color`, `background-color`, `background-image`, `box-shadow`, `border-radius`, `outline`, `visibility`.
- **Composite only** (cheapest, when the element has its own layer): `transform`, `opacity`, and often `filter`.

The exact behavior differs between engines. Test with DevTools paint flashing and the Performance panel instead of trusting a list.

### 308: What are compositor layers and what does promoting an element cost?

The browser can draw some elements into their own layer (a bitmap held in GPU memory), so moving or fading them doesn't repaint anything else. Elements get their own layer when they have 3D transforms, `will-change: transform`, active `transform` or `opacity` animations, `<video>`, `<canvas>`, and so on.

The cost: each layer uses memory (width × height × 4 bytes, times the device pixel ratio squared). Too many layers ("layer explosion") can use hundreds of MB on a phone and make compositing slow. Check the Layers panel in DevTools for how many layers there are and why each one was created.

### 309: What does the `contain` property do?

`contain` tells the browser that an element's inside is independent from the rest of the page, so it can skip work.

- `layout`: inside layout doesn't affect outside layout. It also creates a stacking context and a containing block.
- `paint`: children aren't painted outside the box (like `overflow: clip`).
- `size`: the element's size doesn't depend on its children. You must give it a size.
- `inline-size`: the same, only for the inline axis.
- `style`: counters and quotes don't leak out.
- `content` = `layout paint style`. `strict` = `size layout paint style`.

```css
.widget { contain: content; }
```

It's most useful for widgets that update often, like chat lists and dashboards, because a change inside doesn't make the browser recalculate the whole page.

### 310: What does `content-visibility: auto` do?

It lets the browser skip style, layout and paint for an element's contents while the element is off-screen. It renders them when they come close to the viewport.

```css
.article-section {
  content-visibility: auto;
  contain-intrinsic-size: auto 800px;
}
```

`contain-intrinsic-size` gives a placeholder size so the scrollbar doesn't jump. The `auto` keyword makes the browser remember the real size after it renders the section once. On long pages this can cut initial render time a lot.

Things to watch: find-in-page and anchor links still work. Off-screen content is still in the accessibility tree. Scroll position can jump if the placeholder size is far from the real size. It's Baseline since 2024.

### 311: What is critical CSS?

Critical CSS is the small part of your styles needed to render the above-the-fold content. You inline it in a `<style>` tag in `<head>` and load the rest without blocking.

```html
<head>
  <style>/* critical rules: header, hero, layout */</style>
  <link rel="preload" href="/main.css" as="style">
  <link rel="stylesheet" href="/main.css" media="print" onload="this.media='all'">
  <noscript><link rel="stylesheet" href="/main.css"></noscript>
</head>
```

The first paint happens with no extra request. The cost is the build complexity of picking out the critical rules (tools: Critters/Beasties, Penthouse) and the fact that inlined CSS isn't cached. It helps most on slow networks and when the full CSS is big.

### 312: How does the `media="print" onload` trick work?

A stylesheet with a media query that doesn't match is downloaded at low priority and doesn't block rendering. When it has loaded, the `onload` handler switches `media` to `all`, and the styles apply.

```html
<link rel="stylesheet" href="non-critical.css" media="print" onload="this.media='all'">
```

The `<noscript>` fallback is needed for users with JavaScript turned off. A strict Content Security Policy that blocks inline event handlers will block this `onload`, so you may need a small external script instead.

### 313: How do you find and remove unused CSS?

- **Chrome DevTools Coverage tab** shows which bytes of each stylesheet were used during a session. Click through the main flows first, because a rule unused on the home page may be used elsewhere.
- **Build tools**: PurgeCSS scans your templates for class names. Tailwind only generates classes it finds in your files. CSS Modules and component-scoped styles let unused CSS disappear when the component is deleted.
- **Lint and code search**: Stylelint, plus searching the codebase for class names.

Be careful with class names built dynamically (`"btn-" + variant`), which scanners can't see. Safelist them.

### 314: How can CSS cause layout shift (CLS)?

- Images and embeds without a set size. Use `width`/`height` attributes or `aspect-ratio`.
- Web fonts swapping in with different metrics. Use `size-adjust` and the override descriptors, or `font-display: optional`.
- Late-loading CSS that changes layout after the first paint.
- Content inserted above existing content (banners, ads) without reserved space.
- Animating layout properties like `top` or `height`. Use `transform` instead, which doesn't count toward CLS.
- Scrollbars appearing and pushing content. Use `scrollbar-gutter: stable`.

### 315: How can CSS slow down LCP?

- Large render-blocking stylesheets, or `@import` chains, delay the first paint.
- If the LCP element is a CSS `background-image`, the browser only finds the image after it parses the CSS and matches the rule. That's later than an `<img>` in the HTML. Fix it with `<link rel="preload" as="image" fetchpriority="high">` or by using an `<img>`.
- Fonts with `font-display: block` hide LCP text until they load.
- Hiding content until JavaScript runs (like `opacity: 0` with an animation on load) pushes LCP back.

### 316: How do you load fonts as fast as possible?

1. Self-host WOFF2 files on your own domain. This avoids an extra DNS and TLS connection to a font CDN.
2. Subset fonts to the characters you need, with `unicode-range` for the different language sets.
3. Preload one or two critical font files (always with `crossorigin`).
4. Use a variable font if you need many weights.
5. Use `font-display: swap` or `optional`, and adjust the fallback font's metrics.
6. Cache font files for a long time with a hashed filename.

```css
@font-face {
  font-family: "Inter";
  src: url("/fonts/inter-latin.woff2") format("woff2");
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+2000-206F;
  font-display: swap;
}
```

The browser only downloads a `unicode-range` file if the page uses characters in that range.

### 317: Can `:has()` hurt performance?

It can, depending on how you use it. When the DOM changes, the browser has to work out which elements' `:has()` result might have changed. Engines optimise common cases, but some patterns are expensive:

- Very broad anchors, like `body:has(...)`, `:root:has(...)` or `*:has(...)`, on pages that change the DOM often.
- `:has()` with descendant selectors deep inside a large subtree, like `.list:has(.item:hover)` on a list of thousands.
- Nested complexity like `:has(~ .x:has(.y))`.

Keep the anchor element as specific and close to the target as you can, prefer the child (`>`) or sibling combinators inside `:has()`, and measure "Recalculate Style" in the Performance panel.

### 318: Why do some CSS animations jank even when they use `transform`?

- The element isn't on its own layer yet. The browser promotes it when the animation starts, which can cause a hitch on the first frame.
- Other main-thread work is blocking. A transform animation on the compositor keeps running, but if the browser can't run it on the compositor (for example, because of some filters, or animating a property together with a layout property), it falls back to the main thread.
- The layer is huge, so raster or upload is slow.
- Too many layers, or memory pressure on mobile.
- Animating `transform` from JavaScript with `requestAnimationFrame` puts it back on the main thread. Use CSS animations or the Web Animations API instead.

Use the Performance panel and look for frames that took too long and "Animation" tracks marked as not composited.

### 319: What are scroll-driven animations?

They connect a CSS animation to scroll position instead of time, with no JavaScript.

```css
/* reading progress bar */
.progress {
  position: fixed;
  top: 0;
  left: 0;
  height: 4px;
  width: 100%;
  background: #4f46e5;
  transform-origin: left;
  animation: grow linear;
  animation-timeline: scroll(root block);
}
@keyframes grow {
  from { scale: 0 1; }
  to   { scale: 1 1; }
}
```

`scroll()` tracks how far a scroll container has been scrolled. `view()` tracks an element's progress through the visible area. They work in Chromium and Safari 26. Firefox support has been in progress, and it's part of Interop 2026. Wrap them in `@supports (animation-timeline: scroll())`.

### 320: How do view timelines and animation ranges work?

`view()` follows an element as it crosses the visible part of its scroll container. `animation-range` picks which part of that crossing runs the animation.

```css
.reveal {
  animation: fade-up linear both;
  animation-timeline: view();
  animation-range: entry 0% cover 30%;
}
@keyframes fade-up {
  from { opacity: 0; translate: 0 2rem; }
  to   { opacity: 1; translate: 0 0; }
}
```

Range names: `cover` (from first entering to fully leaving), `contain`, `entry`, `exit`, `entry-crossing` and `exit-crossing`. Named timelines (`view-timeline-name`, `timeline-scope`) let one element's scroll progress drive an animation on another element.

### 321: What is the View Transitions API?

It animates between two states of the page. The browser takes a snapshot of the old state, you update the DOM, and the browser animates from the snapshot to the new state. The default animation is a crossfade.

```js
function navigate(update) {
  if (!document.startViewTransition) return update();
  document.startViewTransition(() => update());
}
```

```css
::view-transition-old(root),
::view-transition-new(root) {
  animation-duration: 250ms;
}
```

Same-document view transitions are Baseline since late 2025. React, Next.js, Astro and other frameworks have built-in support.

### 322: How does `view-transition-name` work, and what does the pseudo-element tree look like?

Elements with a unique `view-transition-name` are captured on their own, and the browser animates their position and size between the old and new states. That's how you get "shared element" transitions, like a thumbnail growing into a hero image.

```css
.product-thumb { view-transition-name: product-42; }
/* on the next page or state: */
.product-hero { view-transition-name: product-42; }
```

During the transition the browser builds this tree in the top layer:

```
::view-transition
└─ ::view-transition-group(product-42)
   └─ ::view-transition-image-pair(product-42)
      ├─ ::view-transition-old(product-42)
      └─ ::view-transition-new(product-42)
```

Names must be unique on the page at capture time. `view-transition-name: match-element` (newer) gives each element an automatic name, which helps with lists. `view-transition-class` lets you style many groups with one rule.

### 323: How do cross-document view transitions work?

For normal multi-page navigation on the same origin, both pages opt in with CSS:

```css
@view-transition {
  navigation: auto;
}
```

The browser captures the old page, loads the new one, and animates between them. No JavaScript or SPA router is needed. The `pageswap` and `pagereveal` events let you change the transition, for example to set names based on which link was clicked. It works in Chromium and Safari 18.2+, and Firefox is working on it as part of Interop 2026.

### 324: What are view transition types?

Types let you run different animations for the same transition depending on context, like "forward" and "back" navigation.

```js
document.startViewTransition({
  update: () => renderNextPage(),
  types: ["slide-left"],
});
```

```css
html:active-view-transition-type(slide-left) {
  &::view-transition-old(root) { animation: slide-out-left 300ms; }
  &::view-transition-new(root) { animation: slide-in-right 300ms; }
}
```

For cross-document transitions, types are set in `@view-transition { types: ... }` or from the `pagereveal` event.

### 325: What is CSS anchor positioning?

It lets you position an element relative to another element (the anchor) anywhere on the page, with no JavaScript like Popper or Floating UI.

```css
.trigger {
  anchor-name: --menu-btn;
}
.menu {
  position: absolute;
  position-anchor: --menu-btn;
  top: anchor(bottom);
  left: anchor(left);
  margin-top: 4px;
}
```

`anchor()` returns the position of an edge of the anchor. It works in Chromium (since 125) and Safari 26, and Firefox shipped it more recently. It's an Interop focus area, so cross-browser support is growing.

### 326: How do anchor positioning fallbacks work?

If the positioned element would overflow the viewport, the browser can try other positions.

```css
.tooltip {
  position: absolute;
  position-anchor: --btn;
  position-area: top;
  position-try-fallbacks: flip-block, flip-inline, bottom;
}

/* custom fallback */
@position-try --right-side {
  position-area: right;
  margin-left: 8px;
}
.tooltip { position-try-fallbacks: flip-block, --right-side; }
```

`flip-block` and `flip-inline` mirror the position. `position-try-order: most-height` picks the option with the most room. `position-visibility: anchors-hidden` hides the element when its anchor scrolls out of view.

### 327: What do `position-area` and `anchor-size()` do?

`position-area` places an element in a 3×3 grid around its anchor, using keywords instead of `anchor()` math.

```css
.menu {
  position-anchor: --btn;
  position-area: bottom span-right; /* below, starting at the anchor's left edge */
}
.dropdown {
  width: anchor-size(width);        /* as wide as the button */
  min-width: max-content;
}
```

`anchor-size()` reads the anchor's width or height. It's how you make a select-style dropdown match its trigger's width.

### 328: How do you build a tooltip with popover and anchor positioning, without JavaScript?

```html
<button popovertarget="tip" style="anchor-name: --tip-anchor">?</button>
<div id="tip" popover="hint">Helpful text</div>
```

```css
#tip {
  position-anchor: --tip-anchor;
  position-area: top;
  position-try-fallbacks: flip-block;
  margin: 0 0 6px;
  inset: auto;   /* clear the default popover centering */
}
```

The popover handles showing and hiding, the top layer, light dismiss (closing on outside click or Escape) and keyboard support. Anchor positioning places it. `popover="hint"` (newer, Chromium) lets a hint open without closing other open popovers. In browsers where the popover invoker is the anchor, newer Chromium sets an implicit anchor, so you may not need `anchor-name` at all.

### 329: What is the customizable `<select>`?

Chromium (since 135) lets you fully style a native `<select>` by opting in with `appearance: base-select`. You can then style the button, the dropdown (`::picker(select)`), options, the check mark (`::checkmark`) and the arrow (`::picker-icon`), and even put rich HTML inside `<option>`.

```css
select,
::picker(select) {
  appearance: base-select;
}
::picker(select) {
  border-radius: 8px;
  box-shadow: 0 8px 24px rgb(0 0 0 / 15%);
}
option:checked { font-weight: bold; }
```

Browsers that don't support it show the normal native select, so it's safe to add now. The dropdown uses the top layer and anchor positioning internally.

### 330: What is the CSS `if()` function?

`if()` picks a value inline based on a condition: `style()`, `media()` or `supports()`.

```css
.button {
  background: if(
    style(--variant: danger): crimson;
    style(--variant: ghost): transparent;
    else: royalblue
  );
  padding: if(media(width < 600px): 0.5rem; else: 1rem);
}
```

It shipped in Chromium 137 and isn't in Firefox or Safari yet (late 2026). Always provide a normal fallback declaration before it.

### 331: What are `sibling-index()` and `sibling-count()`?

They return an element's position among its siblings and the total number of siblings, as numbers you can use in `calc()`.

```css
.list > li {
  transition-delay: calc((sibling-index() - 1) * 50ms); /* staggered animation */
}
.fan > .card {
  rotate: calc((sibling-index() - (sibling-count() + 1) / 2) * 5deg);
}
```

Before these existed, you had to set `--i` on each item from HTML or JavaScript. They shipped in Chromium 138, and the other engines have been adding them. Check current support.

### 332: What is the typed `attr()` function?

The new `attr()` can read an attribute as a specific type and use it in any property, not only in `content`.

```css
.bar {
  width: attr(data-value type(<percentage>), 0%);
}
.swatch {
  background: attr(data-color type(<color>), gray);
}
```

```html
<div class="bar" data-value="72%"></div>
```

The second argument is a fallback. It's in Chromium (since 133) and is an Interop 2026 focus area, so support elsewhere is still on the way.

### 333: What are CSS custom functions (`@function`)?

`@function` lets you define reusable functions in plain CSS that return a value.

```css
@function --fluid(--min, --max) {
  result: clamp(var(--min), calc(var(--min) + 2vw), var(--max));
}

h1 { font-size: --fluid(2rem, 3.5rem); }
```

It's very new: only in Chromium so far. For production code, Sass functions or plain `clamp()` are still the safe choice.

### 334: What do `calc-size()` and `interpolate-size` do?

They let you do math with and animate to intrinsic sizes like `auto`, `min-content` and `fit-content`.

```css
:root { interpolate-size: allow-keywords; } /* opt in once for the page */

.panel { height: 0; overflow: clip; transition: height 300ms; }
.panel.open { height: auto; }

.drawer.open { height: calc-size(auto, size + 2rem); }
```

`interpolate-size` is opt-in because it could change the behavior of existing sites. Both are Chromium-only for now. Other browsers just jump to the final size.

### 335: What math functions does CSS have besides `calc()`?

- `round()`, `mod()`, `rem()`: rounding and remainders.
- `abs()`, `sign()`.
- `sin()`, `cos()`, `tan()`, `asin()`, `acos()`, `atan()`, `atan2()`: trigonometry.
- `pow()`, `sqrt()`, `hypot()`, `log()`, `exp()`.

```css
/* place 8 items on a circle */
.item {
  --angle: calc(360deg / 8 * var(--i));
  translate: calc(cos(var(--angle)) * 120px) calc(sin(var(--angle)) * 120px);
}

/* snap a fluid size to a 4px grid */
.box { width: round(nearest, 33vw, 4px); }
```

Trig and rounding functions are Baseline (2023-2024).

### 336: When should a registered custom property use `inherits: false`?

When the value is only meant for the element itself, like a per-element animation value (`--angle`, `--progress`). There are two reasons:

1. **Correctness**: children don't pick up the value by accident.
2. **Performance**: when an inherited custom property changes on a high-level element (like `:root`), the browser has to recalculate styles for the whole subtree. If it doesn't inherit, only that element is updated. This matters most when you animate a variable or update it from JavaScript every frame.

### 337: What happens with circular custom property references?

If custom properties depend on each other in a loop, every property in the loop becomes "guaranteed-invalid" at computed-value time. Properties that use them fall back to their `var()` fallback, or become `unset`.

```css
.box {
  --a: var(--b);
  --b: var(--a);
  color: var(--a, red); /* red */
}
```

Also, `var()` substitution is done on tokens, not values. `--x: 10px; width: calc(var(--x) * 2)` works, but `--x: 10; width: var(--x)px` doesn't, because it becomes the two tokens `10` and `px`. Use `calc(var(--x) * 1px)` instead.

### 338: How would you design a cascade layer setup for a large app?

```css
@layer reset, tokens, base, vendor, layouts, components, variants, utilities, overrides;
```

- **reset**: box-sizing and margin resets.
- **tokens**: custom properties for colors, spacing and type.
- **base**: element styles (`body`, `a`, `h1`).
- **vendor**: third-party CSS, loaded with `@import ... layer(vendor)`.
- **layouts**: page grids, stack and cluster patterns.
- **components**: buttons, cards. Each component can have a sub-layer.
- **variants** and **utilities**: should always win over components.
- **overrides**: rare, page-level fixes.

Declare the order in one statement that loads first. Keep unlayered CSS to a minimum, because it beats every layer, and use it only for intentional emergency fixes. This removes most specificity fights and makes the order of CSS files in bundles matter much less.

### 339: How does scoping proximity work in `@scope`?

When two declarations come from scoped rules with the same specificity and the same layer, the one whose scope root is **closer** in the DOM wins, no matter the source order.

```css
@scope (.light) { p { color: black; } }
@scope (.dark)  { p { color: white; } }
```

```html
<div class="dark">
  <div class="light">
    <p>Black, because .light is the closer scope root</p>
  </div>
</div>
```

Without `@scope`, the rule that comes later (`.dark`) would win. Proximity is checked after specificity, so a more specific selector still wins.

### 340: What is a "donut scope" and when do you need it?

A donut scope styles a component but stops at certain inner parts, like slots where other components or user content go.

```css
@scope (.card) to (.card__content) {
  a { color: var(--card-link); }
  img { border-radius: 8px; }
}
```

Links and images inside `.card__content` aren't affected. Before `@scope`, you needed long `:not()` chains or strict BEM to keep a component's styles from leaking into nested content.

### 341: How do styles cross the Shadow DOM boundary?

- **Inherited properties** (`color`, `font`, `line-height`, `direction` and so on) pass down into the shadow tree from the host.
- **Custom properties** inherit too, which is the main way to theme components.
- **Non-inherited properties** don't cross, and page selectors can't match shadow content.
- `::part()` lets outside CSS style parts the component exposes. `exportparts` passes parts from nested components up.
- `:host-context()` (limited support) lets a component react to an ancestor outside.
- `all: initial` on `:host` cuts off inherited values if the component wants full isolation.

Global resets like `* { box-sizing: border-box }` don't apply inside shadow roots. Each component has to include its own.

### 342: What are constructable stylesheets?

They are `CSSStyleSheet` objects you create in JavaScript and share between documents and shadow roots with `adoptedStyleSheets`.

```js
const sheet = new CSSStyleSheet();
sheet.replaceSync(`:host { display: block; } .title { font-weight: 600; }`);

class MyCard extends HTMLElement {
  constructor() {
    super();
    const root = this.attachShadow({ mode: "open" });
    root.adoptedStyleSheets = [sheet];
  }
}
```

The browser parses the stylesheet once and shares it, instead of parsing a `<style>` block in every instance. With CSS module scripts (`import sheet from "./card.css" with { type: "css" }`), you can import a CSS file straight as a `CSSStyleSheet`. That's supported in Chromium, Safari and recent Firefox.

### 343: What is CSS Houdini?

Houdini is a set of low-level APIs that open up parts of the CSS engine to developers:

- **Properties and Values API**: `@property` / `CSS.registerProperty()`. Widely supported.
- **Typed OM**: `element.attributeStyleMap` and `CSS.px(10)`. Works with typed values instead of strings. Support isn't complete.
- **Paint API**: `paint()` worklets that draw backgrounds with canvas-like code. Chromium only.
- **Layout API** and **Animation Worklet**: experimental and mostly stalled.

In practice, `@property` is the part that made it into everyday use. The rest has limited support.

### 344: Why is `getComputedStyle` sometimes slow?

It returns up-to-date values. If styles or layout are dirty (something changed since the last calculation), calling it forces the browser to recalculate styles, and for layout values like `width` it forces layout too. Calling it in a loop, mixed with style writes, causes layout thrashing.

Read all the values you need in one batch before any writes. Cache values that don't change. Use `ResizeObserver` or `IntersectionObserver` instead of measuring in scroll or resize handlers.

### 345: Why do runtime CSS-in-JS libraries hurt performance?

On render they serialize styles, hash them, and insert rules into a stylesheet. Inserting rules makes the browser recalculate styles. In React concurrent rendering, inserting during render can happen many times and cause extra style recalculations. That's why React added `useInsertionEffect` for these libraries. They also add bundle size, hydration cost, and don't work well with Server Components, where there is no client runtime.

This is why newer projects pick CSS Modules, Tailwind, or zero-runtime libraries that output static CSS at build time.

### 346: How do zero-runtime CSS-in-JS libraries work?

They let you write styles in TypeScript or JavaScript, but a compiler pulls them out into static `.css` files at build time and swaps the style objects for class names.

```ts
// styles.css.ts (vanilla-extract)
import { style } from "@vanilla-extract/css";

export const button = style({
  padding: "0.5rem 1rem",
  borderRadius: 8,
  ":hover": { opacity: 0.9 },
});
```

You keep type safety, co-location and scoping, with no runtime cost. The trade-off is that truly dynamic values must go through CSS custom properties. Examples: vanilla-extract, Linaria, Panda CSS and StyleX (Meta's atomic CSS compiler).

### 347: What are the trade-offs of atomic CSS?

Atomic CSS means one class per declaration (`.p-4 { padding: 1rem }`), as in Tailwind and StyleX.

**Pros:** CSS size stops growing as the app grows, because classes are reused. It caches well, specificity is flat, there's no dead CSS per component, and changes stay local.

**Cons:** bigger HTML (often fixed by compression), harder-to-read markup, the need for tooling to handle conflicts (for example `tailwind-merge`, or StyleX's "last style wins" rule), and debugging in DevTools shows many tiny rules.

### 348: How would you structure design tokens in CSS?

Use layers of tokens:

```css
:root {
  /* primitive tokens: raw values */
  --blue-500: oklch(58% 0.2 260);
  --gray-900: oklch(20% 0.01 260);
  --space-4: 1rem;

  /* semantic tokens: meaning */
  --color-action: var(--blue-500);
  --color-text: var(--gray-900);
  --space-component: var(--space-4);
}

/* component tokens: optional, a public API for one component */
.button {
  --button-bg: var(--color-action);
  background: var(--button-bg);
  padding: var(--space-component);
}
```

Components only use semantic or component tokens. Themes and brands only change the semantic layer. Tokens are often stored as JSON (the W3C Design Tokens format) and turned into CSS, iOS and Android values with tools like Style Dictionary.

### 349: How would you support several brands in one codebase?

- Keep one set of components that only use semantic tokens.
- Put each brand's values in its own token file, scoped to a selector like `[data-brand="acme"]`, or loaded as a separate stylesheet.
- Combine brand with theme: `[data-brand="acme"][data-theme="dark"]`, or use `light-dark()` in each brand's tokens.
- Use `color-mix()` or relative colors to generate hover and disabled states from one brand color, so each brand only needs a few base values.
- Test visually per brand (Storybook plus screenshot tests), because token mistakes don't show up as CSS errors.

### 350: How do you prevent a flash of the wrong theme with server rendering?

The server doesn't know the user's saved theme (it's in `localStorage`), so the first HTML may render in the wrong theme.

1. Store the preference in a **cookie** as well, so the server can render the right `data-theme` on `<html>`.
2. Or run a tiny **blocking inline script** in `<head>` that reads `localStorage` and sets the attribute before the first paint:

```html
<script>
  try {
    const t = localStorage.getItem("theme");
    if (t) document.documentElement.dataset.theme = t;
  } catch {}
</script>
```

3. Use `color-scheme` and `light-dark()` so the default follows the OS with no JavaScript.
4. In React, add `suppressHydrationWarning` on `<html>` because the attribute differs between server and client.

