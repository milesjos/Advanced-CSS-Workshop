Advanced CSS Techniques
=====================

![Spartan Hackers](http://spartanhackers.com/img/spartan-hackers-banner.png)

### Notes from [Morgan](https://github.com/memuyskens) & Josh's talk for [Spartan Hackers](http://spartanhackers.com/)

We've found these CSS techniques and resources extremely helpful through our adventures as web developers and we thought they would be helpful for anyone seeking help creating more advanced designs using CSS. Happy hacking!
## Introduction
Do you want to make your web pages beautiful? Would you like to wow your friends with your amazing transitions? Would you like to get more of a handle on positioning?

**Well you've come to the right place**

The topics we'll be covering in this talk are:
* Normalize/Reset
* !important
* Pseudo-Selectors
* Units
* Margins, Padding, and Box-Sizing oh my!
* Positioning
* Flex
* Overflow
* Transitions/Animations
* Media Queries

Let's a go!

##CSS Normalize and Reset

In short, CSS Normalize and Reset are different solutions to the same problem:

*How do we handle differences between vendor default stylesheets among commonly used web browsers?*

###[normalize.css](https://necolas.github.io/normalize.css/)

Normalize handles these differences by inserting one master stylesheet that makes html elements render with the same styles no matter what browser the page is viewed on. 

###[CSS Reset](http://cssreset.com/scripts/eric-meyer-reset-css/)

Using a CSS reset removes all default styles completely, making it up to you to apply everything. This means no bullet points for `<ul></ul>`, no `<h1></h1>` sizing, no nothing.

####Advantages

* Easy to implement
	* Link one stylesheet and you're ready to go!
* Keeps things consistent
	* All of the browsers will render your content the same

####Disadvantages

* Slows down load time
	* Adds an extra http request which can slow things down. This is notable when considering slower internet connections
* Disadvantages of Reset
	* You have to set **All** of the styles. Including setting `<strong></strong>` to bold text, `<ul></ul>` to make a bulleted list, and `<h1></h1>` to have a different font size than `<h2></h2>`.
	* Doesn't attempt to specifically target cross-browser issues
* Disadvantages of Normalize
	* Declarations that are unnecessary, such as arbitrary margins on certain elements
	* The file is fairly large for what it does, but is manageable if you remove all the comments, minify it, and include it as one line in the main stylesheet

##!important

Essentially, !important follows a property value and gives it highest priority in the stylesheet. This breaks the cascading order of stylesheets.

####Example 1

```css
p {
	color: white !important;
}
#p_tag {
	color: red;
}
```

```html
<p id='p_tag'>This will be white.</p>
```

Above is an example of !important abuse. IDs should be used for specificity in styling. In this case, either color shouldn't be used for the ID styling (since you obviously want the general style) or the !important needs to go.

####Example 2

```css
p {
	color: white !important;
}
```

```html
<p style='color: red;'>This will be white.</p>
```

This could be thought of as an abuse of !important (it disrupts the cascading order seen below), but maybe you don't want inline styles to show up anymore.

The difference here is that you have a reason, whereas most abuse of !important comes from dungbeetling your CSS and being too lazy to fix it (view Example #1).

*Note that any conflicts with the order below are solved through using the styling of the more specific selector*

####Cascading Order (no. 4 is most important)

1. Browser default
2. External style sheet (for example, a stylesheet pulled from a CDN or anything that you've created)
3. Internal style sheet (CSS styles included in your HTML file)
4. Inline style (styles inside elements)

!important would override all of these.  If there were a second !important, it would only override the first if:

* The selector were more specific (ID vs. Class == ID)
* If the the second use was defined further down in the stylesheet

It's up to you to decide when to use !important, but know that with great power comes great responsibility!

##CSS Selectors

Everyone knows the standard `class` and `id` selectors, but there are tons of other selectors that can be very useful in situations that require a bit more finesse

|Selector|Syntax|Description|
|--------------|-----------------|--------------|
|`.class`|`.selected`|Selects all elements with `class="selected"`|
|`#id`|`#selected`|Selects the element with `id="selected"`|
|`*`|`*`|Selects all elements|
|`element, element`|`.class1, .class2`|Selects all elements with `class="class1"` **and** `class="class2"`|
|`element element`|`.class1 .class2`|Selects all `class2` elements that are nested within `class1` elements|
|`element>element`|`.class1 > .class2`|Selects all `class2` elements that are direct children of a `class1` element|
|`element+element`|`.class1 + .class2`|Selects the `class2` elements that are directly after `class1` elements|
|`element~element`|`.class1 ~ .class2`|Similar to the + selector, but less strict. Selects any `class2` elements following a `class1 element`|
|`[attribute]`|`[href]`|Selects all elements with href attribute|
|`:link`|`a:link`|Selects anchor tags that have not yet been visited|
|`:visited`|`a:visited`|Selects anchor tags that have been visited|
|`:focus`|`input:focus`|Selects the input element that has focus (currently selected)|
|`:hover`|`a:hover`|Selects tags on mouse over|
|`:checked`|`input:checked`|Selects all checked inputs|
|`::before/::after`|`div::after`|Inserts content before/after every div on the page|
|`::first-letter`|`p::first-letter`|Selects first letter of every `<p>` element|
|`::selection`|`::selection`|Selects the portion that is selected/highlighted by the user|


##Units

Sometimes your standard pixels just aren't enough and you need different units for your styles. Here's a breakdown for all the different units you can use in CSS.

|Unit		|Syntax			|Description				|
|-----------|---------------|---------------------------|
|Pixels		|`width: 1000px;`|Most commonly used unit. Is actually an [angular measurement](http://inamidst.com/stuff/notes/csspx) and is not mapped to the physical pixels on the screen|
|Inches		|`width: 10in;`	|Not actual inches. Directly mapped to pixel size `1in == 96px`|
|Centimeters|`width: 100cm;`|Also mapped to pixels `1cm == 37.8px`|
|Millimeters|`width: 1000mm;`|`1mm == 0.1cm ==3.78px`|
|Ems		|`width: 10em;`	|Relative to the current font size of the parent element and will multiply upon themselves upon nesting. When not nested at all, `1em == 16px`|
|Rems		|`width: 10rem;`|Like em, but is relative to the [root element](https://en.wikipedia.org/wiki/Root_element) instead of the parent element|
|Points		|`width: 100pt;`|Physical measurement. `1pt == 1/72 of an inch`|
|Pica		|`width: 12pc;`	|`1 pc == 12pt`|
|ex			|`width: 50ex;` |Similar to ems, but based on the height of the x-character and will change size depending on the font family|
|ch			|`width: 50ch;` |Same as ex, but based on the width of the 0 character|
|Viewport Width|`width: 10vw;`|`1vw` is equal to 1% of the width of the viewport|
|Viewport Height|`width: 10vh;`|Same as vw, but based on viewport height|
|Viewport Minimum|`width: 10vmin;`|Chooses whichever is smaller at the moment, vw or vh|
|Viewport Maximum|`width: 10vmax;`|Chooses whichever is larger at the moment, vw or vh|
|Percent	|`width: 50%;`|References the parent element's size|

##Margin & Padding Tips

Margin and padding are a pretty simple concepts once you see the CSS Box Model (see [here](http://www.w3schools.com/css/css_boxmodel.asp) if you need a refresher).

It's easy as a beginner, though, to set a static width and height on an element before giving the element margin, padding, and a border.

This means your final width (and height) would look something like this:
defined width + padding + border = *Probably more than you intended*

Let's say, for example, that you want 3 boxes on a row of your page. Each would need to be 33.33...%. If you add a thick border that would throw off the width you defined. So then you might decide to subtract the border size from 33.33...%.

Or, to make it easier for yourself, let's check out the CSS property:

```css
box-sizing: content-box /* This is the default value */
```

The default property value is content-box because that's what the box model is: your set width is the width of the content. Not including the padding and border.

But...

```css
box-sizing: border-box /* This includes padding & border */
```

Allows you to set a width that includes padding and border in the final dimensions.

Neat trick, right?

##Positioning
**There are four position values you need to know:**

* Static
* Relative
* Fixed
* Absolute

####Static
This is the default value of the position property.
```css 
* {
position: static;
}
```
With this value, every element is displayed on the page in the order it was listed in the HTML.

If you have properties for top, right, bottom, and left defined they will be ignored under a value of static.

####Relative
```css 
* {
position: relative;
}
```
With this value, elements are displayed with an offset that is defined by properties top, right, bottom, and left.
```css
* {
position: relative;
top: -10px;
right: 0;
left: 5px;
bottom: 0;
}
```
This would result in an element rendered 10px closer to the top of the page and 5px to the right.

####Fixed
```css 
* {
position: fixed;
}
```
With this value, elements are placed with respect to the viewport. 
```css
* {
position: fixed;
top: 0;
right: 0;
left: 0;
}
```
This would result in an element that is always "stuck" to the top of the window. You'll often see those properties and values used for navigation bars.

####Absolute
```css 
* {
position: absolute;
}
```
With this value, elements are placed with respect to a parent element. You might consider fixed to be placement relative to the window and absolute to be placement relative to a parent.
```css
* {
position: relative;
right: 0;
bottom: 0
left: 0;
}
```
This would result in an element that is always "stuck" to the bottom of the *closest ancestor with a position value != to static*. Usually you would want that ancestor to be the parent, but not always.

##Flexbox
In the past, the spacing of children elements inside their parents required methods that sometimes felt inconsistent, like a lot of work for little gain, and overall somewhat "hacky". 

For example, centering a child vertically within their parent might have required you to take half of the child's height, subtract that from half the parent's height, and then use "position: absolute; top [calculated value];" on the child.

**Sounds like more trouble than it's worth, right?**

Luckily, we no longer have to worry about hacking out a solution. We now have flexbox.

[This page](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) is a fantastic resource. Flexbox requires a lot of memorization, and both of us still reference this article.

**Here are the basics:**

* Your parent container needs the property `display: flex;`
* You'll usually use two other properties on the parent:
  * `justify-content: [property value]; /* horizontal alignment */`
  * `align-items: [property value]; /* vertical alignment */` 
* There's a property called "flex-direction" which defaults to row.
  * If you change this to `flex-direction: column;`
     * justify-content now controls vertical alignment
     * align-items now controls horizontal alignment
* `flex-wrap: nowrap;` is default. This means children stay on one line and will shrink to fit within the parent element.
  * `flex-wrap: wrap;` makes the elements *wrap* around to the next line instead of shrinking to fit. Also, this setting allows you to use the property `align-content`.
* `align-content: stretch /* the default when flex-wrap is set to wrap */`
  * This is similar to align-items, but rather than handle the alignment of a single row, align-content handles the entire block's alignment. 

There are properties you can define for flex children, but the secret sauce is in the parent styling and that should be your main focus when using flexbox.

**So that was a lot of text.**

Flexbox is really visual, so we encourage you to check out the awesome examples located [here](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) and [here](https://scotch.io/tutorials/a-visual-guide-to-css3-flexbox-properties)

##Overflow
![CSS is Awesome](http://www.tivix.com/uploads/original_images/css_is_awesome.png)
In certain situations, content can overflow the boundaries given by the parent element. The overflow property controls what happens in these situations.

####Overflow Values

| Value | Description |
|-------|-------------|
|Visible|Default value; content allowed outside of box model|
|Hidden	|Content is hidden	|
|Scroll	|Same as hidden, but there is always a scrollbar present|
|Auto	|Same as Scroll, but the scrollbar is only there when it needs to be|

####Overflow-x and Overflow-y
These values control the overflow properties of each axis and can use all values

####Example
This is taken directly from the sample site
```css
.overflow-box {
    border: 5px solid rgb(40, 97, 172);
    height: 150px;
    width: 50%;
    /*overflow: hidden;*/
    /*overflow: scroll;*/
    overflow-y: scroll;
    margin: 0 auto;/* for centering using this method, your width must be defined */
    background-color: white;
    padding: 0 10px;
    box-sizing: border-box;
}
```

##Transitions

Transitions are a smooth change from one CSS property value to another over a duration.

The shorthand for a transition is this:
```css
transition: [transition-property] [transition-duration] [transition-timing-function] [transition-delay];
```

For example, if you have:
```css
button {
	background-color: blue;
}
```
and that changes on hover to:
```css
button:hover {
	background-color: red;
}
```
**It's not a smooth transition**. It is blue one moment and red the next when you hover.

However, if we add the transition property to button like so:
```css
button {
	background-color: blue;
	transition: background-color 400ms ease-in 0s;
}
```
The button will change from blue to red over 400ms and it will begin the transition immediately since the delay was 0s.

Let's break down the transition shorthand property.

####Transition-property

* This is the CSS property you want to have a gradual change.
* Not every CSS property can be animated.
  * [This](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties) is a list of all properties available for transitions/animations.

####Transition-duration
* This is duration of your transition
* Valid values are:
  * [time]ms (e.g. 400ms)
  * [time]s (e.g. 0.4s)
* Default is 0s

####Transition-timing-function
* This property is a function that speeds up or slows down periods during the transition. [Here's](https://css-tricks.com/almanac/properties/t/transition-timing-function/) a great page explaining this property.
* For example, "ease-in" has a parabolic function. It starts slow and get fast within the transition-duration.
* These are the predefined functions you can use:
  * ease
  * linear
  * ease-in
  * ease-out
  * ease-in-out
  * step-start
  * step-end
* You can also provide your own function too!
  * step() allows you show distinct steps in the transition
  * cubic-bezier() allows you to provide a smooth transition function
     * A fun tool we use to simplify their creation is [here](http://cubic-bezier.com/)

####Transition-delay
* This is the amount of time before your transition begins
* Valid values are:
  * [time]ms (e.g. 400ms)
  * [time]s (e.g. 0.4s)
* Default is 0s

CSS Animations are really similar to transitions, but they take considerably more effort to create as you break down your transition piece-by-piece using keyframes. We won't be covering them in this talk, but a knowledge of transitions is a good start.

##Media Queries

Media Queries are a new technique introduced in CSS3 that basically act as if-else statements in CSS despite the syntax for them being quite different.

####Syntax

The syntax for media queries is fairly simple. Just choose the media type and a condition to decide when the styles will be used and you're good to go! Here's the example that we used in our sample webpage.

```css
@media screen and (max-width: 600px) {
  body {
    background-color: purple;
  }
}
```

####Media Types

|Value 	|Description			|
|------ |-----------------------|
|all	|Used for all devices	|
|print	|Used for print view/printers|
|screen	|Used for desktop and mobile views|
|speech	|Used for screenreaders|

####Media Features

This is a list of the most commonly used features. A full list can be found [here](http://www.w3schools.com/cssref/css3_pr_mediaquery.asp)

|Value			|Description					|
|---------------|-------------------------------|
|max-height		|The maximum height of the display area	|
|max-width		|The maximum width of the display area	|
|min-height		|The minimum height of the display area	|
|min-width		|The minimum width of the display area	|
|orientation	|Orientation of the viewport (landscape/portrait)|

##Resources and Further Reading

Here's a list of all the resources we used in the making of this talk. 

######General Resources

* [Can I use](http://caniuse.com/)
* [CSS Tricks](https://css-tricks.com/)

######CSS Normalize and Reset

* [Normalize CSS](https://necolas.github.io/normalize.css/)
* [The most commonly used version of CSS Reset](http://cssreset.com/scripts/eric-meyer-reset-css/)
* [A good summary of the two and their differences](https://ddessaunet.gitbooks.io/css-training/content/content/reset/reset-vs-normalize-doc.html)
* [Codepen showing the differences between them](http://codepen.io/nategreen/pen/MwxRvP?editors=110)

######!important

* [Article on how and when to use !important](https://www.smashingmagazine.com/2010/11/the-important-css-declaration-how-and-when-to-use-it/)

######Selectors & Pseudo-Selectors

* [In depth explanation of some important selectors](http://code.tutsplus.com/tutorials/the-30-css-selectors-you-must-memorize--net-16048)

######Units

* [Article on the subtleties between each of the different units](http://code.tutsplus.com/tutorials/the-30-css-selectors-you-must-memorize--net-16048)

######Margin and Padding

* [Margin rules](https://css-tricks.com/almanac/properties/m/margin/)
* [Padding Rules](https://css-tricks.com/almanac/properties/p/padding/)
* [Border-box](https://css-tricks.com/almanac/properties/b/box-sizing/)

######Positioning

* [General rules](https://css-tricks.com/almanac/properties/p/position/)
* [Centering in CSS](https://css-tricks.com/centering-css-complete-guide/)

######Flex

* [CSS Tricks guide](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
* [scotch.io tutorial](https://scotch.io/tutorials/a-visual-guide-to-css3-flexbox-properties)

######Overflow

* [General rules](https://css-tricks.com/almanac/properties/o/overflow/)

######Transitions and Animations

* [General rules and information](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions)
* [List of elements that can have transition properties](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties)
* [CSS Animations vs. Javascript](https://css-tricks.com/myth-busting-css-animations-vs-javascript/)

######Media Queries

* [General rules and information](http://www.w3schools.com/cssref/css3_pr_mediaquery.asp)
* [Article on using Media Queries](http://www.w3schools.com/css/css_rwd_mediaqueries.asp)

######Miscellaneous

* [How CSS actually works](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_started/How_CSS_works)
* [CSS History](https://en.wikipedia.org/wiki/Cascading_Style_Sheets#History)
* [CSS Preprocessors](http://www.sitepoint.com/6-current-options-css-preprocessors/)
	* [Sass](http://sass-lang.com/)
	* [Less](http://lesscss.org/)
* [initial-scale=1](https://css-tricks.com/probably-use-initial-scale1/)
* CSS Autoprefixers for [Sublime Text](https://github.com/sindresorhus/sublime-autoprefixer) and [Atom](https://atom.io/packages/autoprefixer)
* [Another HUGE css resource list](https://github.com/sotayamashita/awesome-css)
