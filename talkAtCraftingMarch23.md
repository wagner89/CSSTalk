## Front end styling talk @ Crafting

## CSS, SASS, SCSS
- what are the differences, why should we care?
  - Sassy CSS adds a preprocessor
  - codewise: CSS3 < SASS === SCSS with more CSS like syntax

#### 0. Math
```
width: (120em/16) * 50%
```
#### 1. Declaring variables (actually, more like constants)

```
_crafting.scss

$crafting-font-stack: Helvetica, sans-serif;

$crafting-primary-color: #12ebff;

$menu-fontsize-normal: 1.75rem;
$menu-fontsize-large: 2.5rem;

```

#### 2. Partial style sheet files
```
@import 'crafting';

body {
  font: 100% $crafting-font-stack;
  color: $crafting-primary-color;
}
```

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

_(also, we should probably get into the habit of using font stacks instead of a single font, it's industry best practice)_

#### 4. Mixins
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

## CSS specific topics

#### 1. Specificity

```
<div id="test">
  <span>Text</span>
</div>

span { color: black !important }
span { color: red; }
div span { color: blue; }
div#test span { color: green; }
```

#### 2. Stacking contexts

Stacking contexts appear on an element when:

[root, position != static && z-index != auto, opacity < 0] => these all form a new stacking context

Inside the same stacking context, the stacking order is:

[root,
 positioned elements (with negative z-order),
 non-positioned elements,
 positioned (with z-order auto, in appearance order),
 positioned (with positive z-order)] => determine the order of contexts, and of elements inside of contexts
 
#### 3. Units of measurment (px, em, rem)

#### 4. Overflow vs. Z-index

#### 5. The box model

Content - padding - border - margin

#### 6. Margin collapsing

Vertical margins collapse on sybling elements to the greater of the two

#### 7. Flexbox


#### 8. 


### Sources
- _[Engage Interactive](https://engageinteractive.co.uk/blog/top-10-scss-mixins)_
- _[SASS website](https://sass-lang.com/guide)_
- _[Phillip Walton](https://philipwalton.com/articles/what-no-one-told-you-about-z-index/)_
