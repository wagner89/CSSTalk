# Quick CSS topics for beginner front-end developers
This is a (hopefully) evolving collection of CSS tidbits, explained briefly in writing, and illustrated by corresponding JSFiddle links.

---

## Target audience:
* Developers who have no particular interest in CSS, but it was thrust upon them to produce front end code by on form or another of evil managment
* Newcomers to CSS, who enjoy fiddling with code snippets
* Veterans of CSS, who are looking for concise explanations/examples to send to others (and who might find what's wrong with my examples and correct them)
* More...

---

## Topics 
#### (feel free to branch this repo and create PRs if you want to add a topic)
1. [Defining sizes in EMs / REMs / PXs](https://github.com/wagner89/CSSTalk/blob/master/README.md#defining-sizes-in-ems--rems--pxs) 
2. Stacking contexts and Z-indexes
3. Confusing hidden overflow with bad Z-index
4. Defininf colors, constants, opacity
5. Padding vs margin (vertical margin collapsing)
6. Specificity and the !important atribute
7. Basic flexbox principles
8. Responsivness principles
9. Collapsing CSS selectors
10. next topic here...

---

### 1. Defining sizes in EMs / REMs / PXs
##### (also: vh, vw, ex, ch...)
You can use a number of units of measurment to specify a size for anything in CSS (margins, paddings, widths, heights, you get the picture...)

Let's get this out of the way first: the notion of `DP` or `DDP` (and many more acronyms), or `Device Pixel` refers units of measurment which make sure that your designs look the same on different resolution/aspect ratio/pixel density screens - basically it means that the browser knows how many actual pixels on screen (`display pixels`) a `DP` corresponds to, and rendes everything by multiplying the DP value with the ratio of pixels per DP. So, if you use such `device independent` units, your lengths will remain proportional regardless of the actual device they're rendered on.

In CSS, we can use `px`, which is a `CSS pixel`, and it is always **1/96 physical inches** (the same as `user unit` in SVG). Of course, this means that depending on the screen and how many `device pixels` are on the screen, there will be a ration between one `device pixel` and one `px`.

**All CSS units of lenght translate down to `px`, so other units are definable as (X * 1px)**

`em` and `rem` are both such units, and by default they both translate to `16px`s. They both are relative values to a given font size, but different ones:

| Unit |       Equivalent in px      | 
|:----:|:---------------------------:|
|  px  |              1              |
|  em  |    `element font size` px   |
|  rem | `root element font size` px |

This means that `em`s will scale according to local font size (which might be inherited), while `rem`s onl;y care about the font-size of the `html` or root element.

Check out the following example: https://jsfiddle.net/wagner89/ejqj23g1/4/
Play around changing the font-size for the `html` and the differnet `div`s. You can see that using `em` and `rem` give you control over how to scale in different environments. 


