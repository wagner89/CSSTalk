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
