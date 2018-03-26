# Stuff I probably should've known before writing production CSS 
![Peter Griffin here](https://i.giphy.com/media/yYSSBtDgbbRzq/giphy.webp)

## Table of contents

- Vanilla CSS topics (no pre-processor involved)
  0. Reseting and normalization
  1. Specificity
  2. Stacking contexts
  3. Units of measurment (px, em, rem)
  4. Overflow vs. Z-index
  5. The box model
  6. Margin collapsing
  7. Flexbox and CSS Grid
  
- CSS, SASS, SCSS
  0. Math
  1. Declaring variables (actually, more like constants) 
  2. Partial style sheet files
  3. Nesting
  4. Mixins
  5. Inheritance

### Vanilla CSS topics (no pre-processor involved)

#### 0. Reseting and normalization

Browsers are different and opinionated, and this makes front end development a real hassle.

<img src="http://weknowmemes.com/wp-content/uploads/2012/07/testing-webpages-on-different-browsers.jpg" width="550px"/>


The lazy mans reset

```
* {
  margin: 0;
  padding; 0'
}
```

The first wildly popular solution: the Meyer reset [reset.css](https://meyerweb.com/eric/tools/css/reset/reset.css)

The shift in approach: instead of a full reset, default everything to a standard baseline, ex 
[The normalize.css library](http://nicolasgallagher.com/about-normalize-css/)

This is HTML5 ready, and used by large frameworks (like BootstrapJS)

Normalize.css vs CSS resets:

 - targets only the styles that need normalizing
 - preserves useful browser defaults rather than erasing them
 - corrects bugs and common browser inconsistencies
 - doesn't clutter the debugging tools
 - has better documentation

#### 1. Specificity

Style sheets can be, in order of priority:
- inline
- internal
- external
- browser default

When applying stlyes, more specific selectors win out:

`type/pseudo-element` (div, ::after) < `class, attribute, pseudo-class` (.popup, [type='button'], :hover) < `ids` (#thisDiv) < `inline stlyes` (<div style="..." />)

```
<div id="test">
  <span>Text</span>
</div>

span { color: black !important }
span { color: red; }
div span { color: blue; }
div#test span { color: green; }
```
For equal specificity the order of declaration is what matters -> the latter wins out over the former.

![Batman](https://memeexplorer.com/cache/846.jpg)

Avoid `!important` if possible, as it blocks others as a side effect helping you (never use it in libraries).

A nice tool for this is [this calculator](https://specificity.keegan.st/)

Also, a great place to check on selector syntax [is this page](https://code.tutsplus.com/tutorials/the-30-css-selectors-you-must-memorize--net-16048)

#### 2. Stacking contexts

Try to bring the middle square in front this [example](https://codepen.io/wagner89/pen/baqvrw?editors=1100)

![](http://s2.quickmeme.com/img/ae/ae47b29373cbcce38d4969f0a11b82f2f2332efef505133ef16a810720d72ed6.jpg)

New stacking contexts are created by:
- `root` element
- elements with `position <> static` and `z-index <> auto`
- `opacity < 1`

Inside any stacking context, the stacking order is:

- root,
- positioned elements (with negative z-order),
- non-positioned elements,
- positioned (with z-order auto, in appearance order),
- positioned (with positive z-order)] => determine the order of contexts, and of elements inside of contexts
 
_With SASS, it's a good practice to define levels within the stacking order (i.e. root, menu, popup, etc)_
 
#### 3. Units of measurment (px, em, rem)

`px` is stable across the device you are using
`em` is relative to the current local font-size
`rem` is relative to the root `html` font-size (which, unless set is inherited from the browser settings!)

Some different things to consider:
 - setting `html` font-size might make the site not respond to browser font size settings done by the user
 - `rem` is generally accepted as the proper choice for font sizing
 - `em` is good for paddings, margins, etc in specific reusable components (ex. button padding)
 - `rem` is good for media queries, unless you start zooming, in which case only `em` does not missfire (except on Safari)
 - `px` is not good for media query breakpoints, since it doesn't scale with font-size (`!`)
 - `rem` is never influenced by nesting, but `em` might be difficult to manage due to inheritance
 - SASS calculations are useful when thinking in `px` but specifying in `em` or `rem`
 - layouting, for ex. column layout proportions should always use `%` instead of other units, to keep layouts fluid
 
The rule agreed upon for our project is 

| Unit of measurment   | Usage  |
|:-:|:-:|
| rem  | fonts  |
| em  | margin, padding, etc  |
| px  | last resort  |

My conclusion: best judgment should be used not rigid guidelines

Bonus: what to use for `line-height`, to make sure it's double? Actually, the most safe way to do this, is to not pass a unit, just plain old 2. This is inherited further regardless of what font-size is applied in a given element.

Other useful units of measurment in CSS:

| Unit of measurment | Usage |
|:-:|:-:|
| vw | 1% of viewport width |
| vh | 1% of viewport height |
| vmin | 1% of smaller dimension of viewport |
| vmax | 1% of larger dimension of viewport |

#### 4. Overflow vs. Z-index

Check out this [CodePen](https://codepen.io/wagner89/pen/rpypdX?editors=1100)

Overflow works in different ways according to it's value 
  `scroll` - always scroll
  `auto` - scroll if needed,
  `hidden` - clip all overflowing content
  `visible` - show all overflowing content
  
Also works pe axis, `overflow-x` and `overflow-y` separately, but

```
The computed values of ‘overflow-x’ and ‘overflow-y’ are the same as their specified values, except that some combinations with ‘visible’ are not possible: if one is specified as ‘visible’ and the other is ‘scroll’ or ‘auto’, then ‘visible’ is set to ‘auto’. The computed value of ‘overflow’ is equal to the computed value of ‘overflow-x’ if ‘overflow-y’ is the same; otherwise it is the pair of computed values of ‘overflow-x’ and ‘overflow-y’.
```

This unfortunately means that `overflow-x: visible` and `overflow-y: hidden` used together is essentially just
`overflow-x: auto` and `overflow-y: hidden`.

#### 5. The box model

Every HTML element is comprised of four boxes:

`content` - `padding` - `border` - `margin`

![box-model](http://www.turnwall.com/wp-content/uploads/2014/06/box-model-css.png)

The `box-sizing` CSS property can be used to affect what the `width`,`height`, `min-width` and `min-height` properties actually specify:
  - `content-box` - the default, does not include padding and borders
  - `borer-box` - includes padding and borders as well
  
Many sources swear by the universal box-sizing approach: 
```
* {
    box-sizing: border-box;
}
```
which ensures that you never get in a mess because of unexpected padding/border sizing screwing up layouting.

#### 6. Margin collapsing

Vertical margins collapse to the greater of the two adjacent margins, or to just one of them if they are equal, for:

- adjacent sybling elements
- first/last children of an element and its parent element
- top and bottom margin of a heightless empty block

A positive and a negative margin get added instead of collapsing to the greater, while two negatives yield the most negative (smallest) margin.

The first/last child with a positive margin will push its own margin outside its parents block even if its parent has 0 margin.

These rules does not apply when an element is `floating` or `absolute`.

#### 7. Flexbox and CSS Grid

![](https://img.devrant.com/devrant/rant/r_26154_fwP7c.jpg)

Layout alternative already embedded in CSS3 - two axis model: main and cross axis.

Check out the examples [here](https://www.sketchingwithcss.com/samplechapter/cheatsheet.html)

Play with frogs [here](http://flexboxfroggy.com/)

![](https://i0.wp.com/theitteam.com.au/wp-content/uploads/2015/06/css-center-align-meme.jpg)

While flexbox is used to create fluid designs which work well if responsiveness is key, an alternative to flowing content across boxes is to align it to a fixed grid - and maybe not need media queries anymore?

<img src="https://daverupert.com/images/posts/2017/css-grid.jpg" width="400px" />

Nice way to compare the two is by reading [this complete guide](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) and [this complete guide](https://css-tricks.com/snippets/css/complete-guide-grid/)

##### When in doubt, check the CSS reference [here](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)

## CSS, SASS, SCSS
- what are the differences, why should we care?
  - Sassy CSS adds a preprocessor (like Stylus or PostCSS, etc.)
  - codewise: CSS3 < SASS === SCSS with more CSS like syntax

#### 0. Math
<img src="http://www.sassshop.com/images/math.jpg" width="300">

```
width: (120em/16) * 50%
```

The supported operators are `+`, `-`, `*`, `/`, and `%`. 

#### 1. Declaring variables (actually, more like constants)

```
_crafting.scss

$crafting-font-stack: Helvetica, sans-serif;

$crafting-primary-color: #12ebff;

$menu-fontsize-normal: 1.75rem;
$menu-fontsize-large: 2.5rem;

```

How we already use it: ex. `_colors.scss`

#### 2. Partial style sheet files
```
@import 'crafting';

body {
  font: 100% $crafting-font-stack;
  color: $crafting-primary-color;
}
```
_(also, we should probably get into the habit of using font stacks instead of a single font, it's industry best practice)_

#### 3. Nesting
```
side-bar {
  a {
    font-size: $menu-fontsize-normal;
    color: $crafting-secondary-color;
  }
  
  h1 {
    font-size: $menu-fontsize-large;
    color: $crafting-primary-color;
  }
}
```

vs.
```
side-bar a {
  font-size: $menu-fontsize-normal;
  color: $crafting-secondary-color;
}

side-bar h1 {
  font-size: $menu-fontsize-large;
  color: $crafting-primary-color;
}
```

#### 4. Mixins

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTtFLGOJtFEYNzAsSrMFhJcYQQ6qdlG6LG8RilsMcFNBjKVQUMw)

```
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
}

---

.teamMemberSection { @include border-radius(10px); }
```

```
@function z($name) {
    @if index($z-indexes, $name) {
        @return (length($z-indexes) - index($z-indexes, $name)) + 1;
    } @else {
        @warn 'There is no item "#{$name}" in this list; choose one of: #{$z-indexes}';
        @return null;
    }
}

$z-indexes: (
    "outdated-browser",
    "modal",
    "site-header",
    "page-wrapper",
    "site-footer"
);

---

.site-header {
    z-index: z('site-header');
}
```

How we use it already: https://github.com/lolmaus/breakpoint-slicer

#### 5. Inheritance 
```
%generic-popup {
  border: 1px solid #FFFFFF;
  padding: 1em;
  color: #E1E1E1;
  
}

.popup {
  @extend %generic-popup;
}

.popup-success {
  @extend %generic-popup;
  border-color: green;
}

.popup-error {
  @extend %generic-popup;
  border-color: red;
}
```

How we use it already: `dialogHeader` in Approval/Decline Dialogs

### You can't ever learn CSS, you can at most get used to it
What other environment would provide you with 6 different kinds of values for any property:
`initial`, `computed`, `resolved`, `specified`, `used`, and `actual`

### Miscellaneous useful links
- [CSS selector specificity calculator](https://specificity.keegan.st/)
- [CSS color picker](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Colors/Color_picker_tool)
- [Color palette designer](http://paletton.com/)
- [Visual editor to generate CSS](http://enjoycss.com)
- [Online CSS minifier](https://cssminifier.com/)

### Sources
- _[the MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)_
- _[Engage Interactive](https://engageinteractive.co.uk/blog/top-10-scss-mixins)_
- _[SASS website](https://sass-lang.com/guide)_
- _[Phillip Walton](https://philipwalton.com/articles/what-no-one-told-you-about-z-index/)_
- _[envato](https://webdesign.tutsplus.com/tutorials/comprehensive-guide-when-to-use-em-vs-rem--cms-23984)_
- _[Engage Interactive 2](https://engageinteractive.co.uk/blog/em-vs-rem-vs-px)_
- _[zellwk](https://zellwk.com/blog/media-query-units/)_
- _[sitepoint](https://www.sitepoint.com/3-things-almost-one-knows-css/)_
- _[w3schools](https://www.w3schools.com/cssref)_
- _[css-tricks](https://css-tricks.com)_
- _[meyerweb.com/](https://meyerweb.com/)_
