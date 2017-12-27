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

### 2. Stacking contexts and Z-indexes
#### (or why can't I see my element, part 1.)

Every developer doing front end work is familiar with the notion of Z-index, a property you can set to define in what order elements stack on on top of the other - in terms of depth. So, a small Z-index is at the bottom (or farthest from the user), and a large Z-index is at the top (or closer to the user).

The following examples attempt to illustrate the twist that CSS puts on this simple notion, by introducing **stacking contexts**.

...

To explain the above: inn CSS, we distinguish between any number of stacking contexts, which can each be seen as a separate container as far as Z-index is concerned. If you have three boxes in the root of your page, and want to order them in a specific way, but then again each of them have three boxes as children, which also need a specific ordering, stacking contexts help you out: you can have one for the three initial boxes (this, incidentally, will be your **root** stacking context, since it's the bottom most on your page), and you can define a separate, so called **local** stacking context for each of the three boxes, to order their content as you please, but independently of the ordering of the initial boxes.

What creates a new stacking context? (Not a complete list, for that see [this](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context) link)
 - the root element of document (HTML) - obviously
 - elements with position value "absolute" or "relative", with a specified Z-index
 - elements with a position value "fixed"
 - elements that are a child of a flex (flexbox) container, with a specified Z-index
 - elements that are transparent (opacity < 1)
 
 Any time you set the Z-index of a given element, but it fails to jump on top of whatever it's behind, even though it has a larger Z-index value, go ahead and read [this](https://philipwalton.com/articles/what-no-one-told-you-about-z-index/) article!


