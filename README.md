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
* AddClass, RemoveClass, and ToggleClass
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
2. External style sheet (for example, a stylesheet pulled from a CDN)
3. Internal style sheet (the stylesheet you've created)
4. Inline style (CSS styles included in your HTML file)

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

* Pixels
	* Syntax
		* `width: 1000px;`
	* What to know
		* Most commonly used unit
		* Is actually an [angular measurement](http://inamidst.com/stuff/notes/csspx) and is not mapped to the physical pixels on the screen
* Inches
	* Syntax
		* `width: 10in;`
	* What to know
		* These are not actual inches, but instead directly map to pixels
		* `1in == 96px`
* Centimeters
	* Syntax
		* `width: 100cm;`
	* What to know
		* Also just mapped to pixels
		* `1cm == 37.8px`
* Millimeters
	* Syntax
		* `width: 1000mm;`
	* What to know 
		* They are exactly as one would expect
		* `1mm == 0.1cm ==3.78px`
* Ems
	* Syntax
		* `width: 10em;`
	* What to know
		* Is relative to the current font size of the parent element
		* Ems also will multiply upon themselves upon nesting
		* When not nested at all, `1em == 16px`
* Rems
	* Syntax
		* `width: 10rem;`
	* What to know
		* Like em, but is relative to the [root element](https://en.wikipedia.org/wiki/Root_element) instead of the parent element
		* Does not work in IE 8, Safari 4, or iOS 3.2
* Points
	* Syntax
		* `width: 100pt;`
	* What to know
		* Physical measurement equal to 1/72 of an inch. 
		* Makes most sense for items that will be displayed in print
		* There used to be large differences between browser's interpretations of what a point was. See [ here](http://css-tricks.com/wp-content/csstricks-uploads/pointsizeonscreen.jpg)
* Pica
	* Syntax
		* `width: 12pc;`
	* What to know
		* Same as points
		* `1 pc == 12pt`
* ex
	* Syntax
		* `width: 50ex;`
	* What to know
		* Similar to ems, but based on the height of the x-character
		* Different from ems in that the size will change according to the font-family
* ch
	* Syntax
		* `width: 50ch;`
	* What to know
		* Same as ex, but based on the width of the 0 character
* Viewport Width
	* Syntax
		* `width: 10vw;`
	* What to know
		* `1vw` is equal to 1% of the width of the viewport
		* Remains consistent for all elements on the page, regardless of the font-size of the parent element
* Viewport Height
	* Syntax
		* `width: 10v;`
	* What to know
		* Same as vw, but based on the height instead
* Viewport Minimum
	* Syntax
		* `width: 10vmin;`
	* What to know
		* Chooses whichever is smaller at the moment, vw or vh. 
* Viewport Maximum
	* Syntax
		* `width: 10vmax;`
	* What to know
		* Chooses whichever is larger at the moment, vw or vh
* Percentage
	* Syntax
		* `width: 50%;`
	* What to know
		* Based on the property of the parent element
		* E.g. If the parent element is 500px, a child element with a setting of `width: 50%;` would produce an element with a width of 250px

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
* [CSS Animations vs. Javascript](https://css-tricks.com/myth-busting-css-animations-vs-javascript/)

######Media Queries

* [General rules and information](http://www.w3schools.com/css/css_rwd_mediaqueries.asp)

######Miscellaneous

* [How CSS actually works](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_started/How_CSS_works)
* [CSS History](https://en.wikipedia.org/wiki/Cascading_Style_Sheets#History)
* [CSS Preprocessors](http://www.sitepoint.com/6-current-options-css-preprocessors/)
	* [Sass](http://sass-lang.com/)
	* [Less](http://lesscss.org/)
* [initial-scale=1](https://css-tricks.com/probably-use-initial-scale1/)
* CSS Autoprefixers for [Sublime Text](https://github.com/sindresorhus/sublime-autoprefixer) and [Atom](https://atom.io/packages/autoprefixer)
