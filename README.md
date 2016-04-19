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