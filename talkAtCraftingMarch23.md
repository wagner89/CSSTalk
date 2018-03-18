## Front end styling talk @ Crafting

1. CSS, SASS, SCSS
    - what are the differences, why should we care?
      - Sassy CSS adds a preprocessor
      - codewise: CSS3 < SASS === SCSS with more CSS like syntax
      
Examples:

1. Declaring variables (actually, more like constants)

```
_crafting.scss

$crafting-font-stack: Helvetica, sans-serif;
$crafting-primary-color: #12ebff;
$menu-fontsize-normal: 1.75rem;
$menu-fontsize-large: 2.5rem;

```

2. Partial style sheet files
```
@import 'crafting';

body {
  font: 100% $crafting-font-stack;
  color: $crafting-primary-color;
}
```

3. Nesting
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

