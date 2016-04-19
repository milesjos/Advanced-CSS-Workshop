Advanced CSS Techniques
=====================

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

!important would override all of these. 

If there were a second !important, it would only override the first if:

* The selector were more specific (ID vs. Class == ID)
* If the the second use was defined further down in the stylesheet

It's up to you to decide when to use !important, but know that with great power comes great responsibility!

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
