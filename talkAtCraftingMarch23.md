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

```

2. Partial style sheet files
```
@import 'crafting';

body {
  font: 100% $crafting-font-stack;
  color: $crafting-primary-color;
}
```

_(also, we should probably get into the habit of using font stacks instead of a single font, it's industry best practice)_

