#Umwelt CSS Style Guide
A mostly reasonable approach to CSS. Most of it is based on and is an excerpt of **Harry Roberts' CSS Styleguide** (http://cssguidelin.es/), and is aimed to be presented in the style of **Airbnb's Javascript Style Guide** (https://github.com/airbnb/javascript).

##Table of Contents
* [Syntax and Formatting](#syntax-and-formatting)
	* [80 characters wide](#80-characters-wide)
	* [Titles](#titles)
	* [Anatomy of a ruleset](#anatomy-of-a-ruleset)
	* [Alignemnt](#alignment)
	* [Meaningful whitespace](#meaningful-whitespace)
* [Commenting](#commenting)
* [Naming Conventions](#naming-conventions)
	* [Hyphen Delimited](#hyphen-delimited)
	* [BEM-like Naming](#bem-like-naming)
		* [In use](#in-use)
		* [More Layers](#more-layers)
	* [Naming Conventions in HTML](#naming-conventions-in-html)
* [CSS Selectors](#css-selectors)
	* [Selector Intent](#selector-intent)
	* [Reusability](#reusability)
	* [Location Independence](#location-independence)	
	* [Qualified Selectors](#qualified-selectors)
		* [Quasi-Qualified Selectors](#quasi-qualified-selectors)
	* [Naming](#naming)
	* [Selector Performance](#selector-performance)
	* [Sum Up](#sum-up)
* [Specificity](#specificity)
* [What now?](#what-now)

		



##Syntax and Formatting

At a very high-level, we want:

* Use tabs for indentation;
* 80 character wide columns;
* Multi-line CSS;
* Meaningful use of whitespace.

But, as with anything, the specifics are somewhat irrelevant — **consistency is key**.


###80 characters wide

Where possible, limit CSS files’ width to 80 characters. Reasons for this include

* the ability to have multiple files open side by side;
* viewing CSS on sites like GitHub, or in terminal windows;
* providing a comfortable line length for comments.


###Titles

Begin every new SASS file with a title, example:

```
/*------------------------------------*\
    #_nav
\*------------------------------------*/

.selector {}

```

The title of the section is prefixed with a hash (#) symbol so we can perform targeted searches. Leave a carriage return (line break) between this title and the next line of code.


###Anatomy of a ruleset



**Terminology:**
```
[selector] {
    [property]: [value];
}
```

**Rules:**

* related selectors on the same line; unrelated selectors on new lines;
* a space before our opening brace ({);
* properties and values on the same line;
* a space after our property–value delimiting colon (:);
* each declaration on its own new line;
* the opening brace ({) on the same line as our last selector;
* our first declaration on a new line after our opening brace ({);
* our closing brace (}) on its own new line;
* each declaration indented by four (4) spaces;
* a trailing semi-colon (;) on our last declaration.

Example:
```
// Bad
.foo, .foo--bar, .baz
{
	display:block;
	background-color:green;
	color:red }


// Good
.foo, .foo--bar,
.baz {
    display: block;
    background-color: green;
    color: red;
}
```


###Alignment
Attempt to align common and related identical strings in declarations, for example:
```
.foo {
    -webkit-border-radius: 3px;
       -moz-border-radius: 3px;
            border-radius: 3px;
}

.bar {
    position: absolute;
    top:    0;
    right:  0;
    bottom: 0;
    left:   0;
    margin-right: -10px;
    margin-left:  -10px;
    padding-right: 10px;
    padding-left:  10px;
}
```
This makes life a little easier for developers whose text editors support column editing, allowing them to change several identical and aligned lines in one go.


###Meaningful Whitespace

As well as indentation, we can provide a lot of information through liberal and judicious use of whitespace between rulesets. We use:

* One (1) empty line between closely related rulesets.
* Two (2) empty lines between loosely related rulesets.
* Five (5) empty lines between entirely new sections.

For example:
```
// Bad
.foo {}
    .foo__bar {}
.foo--baz {}


// Good
/*------------------------------------*\
    #FOO
\*------------------------------------*/

.foo {}

    .foo__bar {}


.foo--baz {}





/*------------------------------------*\
    #BAR
\*------------------------------------*/

.bar {}

    .bar__baz {}

    .bar__foo {}
```





##Commenting

The worst situation for most developers is being the-person-who-didn’t-write-this-code. Remembering your own classes, rules, objects, and helpers is manageable to an extent, but anyone inheriting CSS barely stands a chance.

####CSS needs more comments.

In doubt about when to?
**As a rule, you should comment anything that isn’t immediately obvious from the code alone.**

Comment, when:

* **some CSS relies on other code elsewhere;**
* **changing some code will have effects elsewhere;**
* **explaining what you intended a piece of CSS to be used for.**

Or even when: 

* various sates of overflow triggers some block formatting context; 
* or certain transform properties trigger hardware acceleration, etc.

Example:
```
/**
 * These rules extend `.btn {}` in _objects.buttons.scss.
 */

.btn--positive {}

.btn--negative {}
```





##Naming Conventions

Naming conventions in CSS are hugely useful in making your code more strict, more transparent, and more informative.

A good naming convention will tell you and your team:

* what type of thing a class does;
* where a class can be used;
* what (else) a class might be related to.

A good rule of thumbs: **Hyphen (-) delimited strings for simple parts of the UI, with BEM-like naming for more complex pieces of code.**

**Note:** *A naming convention is not normally useful in the CSS; they really come into their own when viewed in HTML.*


###Hyphen Delimited

All strings in classes are *delimited with a hyphen* (`-example`). Camel case and underscores are not used for regular classes.

Example:

```
// Bad
.pageHead {}

.sub_content {}


// Good
.page-head {}

.sub-content {}
```


###BEM-like Naming

For larger, more interrelated pieces of UI that require a number of classes, we use a BEM-like naming convention.

*BEM*, meaning *Block*, *Element*, *Modifier*, is a front-end methodology, here we are only concerned with its naming convention. 
Our naming convention is only BEM-like; the principles are exactly the same, but the actual syntax differs slightly.

BEM splits components’ classes into three groups:

**Block**: The sole root of the component.
**Element**: A component part of the Block - *delimited with two (2) underscores* (`__example`).
**Modifier**: A variant or extension of the Block - *delimited by two (2) hyphens* (`--example`).

To take an analogy (note, not an example):
```
.person {}
.person__head {}
.person--tall {}
```

`.person {}` is the Block; it is the sole root of the entity. `.person__head {}` is an Element; it is a smaller part of the `.person {}` Block. Finally, `.person--tall {}` is a Modifier; it is a specific variant of the `.person {}` Block.


####In use

**Your Block context starts at the most logical, self-contained, discrete location.** 

To continue with our person-based analogy, we’d not have a class like `.room__person {}`, as the room is another, much higher context. We’d probably have separate Blocks, like so:
```
.room {}

    .room__door {}

.room--kitchen {}


.person {}

    .person__head {}
```

If we did want to denote a `.person {}` inside a `.room {}`, it is more correct to use a selector like `.room .person {}` which bridges two Blocks than it is to increase the scope of existing Blocks and Elements.

More realistic example:

```
// Bad
.page {}

    .page__content {}

    .page__sub-content {}

    .page__footer {}

        .page__copyright {}


// Good
.page {}


.content {}


.sub-content {}


.footer {}

    .footer__copyright {}
```

It is important to know when BEM scope starts and stops. **As a rule, BEM applies to self-contained, discrete parts of the UI.**

**Further Reading**
[MindBEMding – getting your head ’round BEM syntax](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

####More Layers

If we were to add another Element — called, let’s say, `.person__eye {}` — to this `.person {}` component, we would not need to step through every layer of the DOM. Like:

```
//Bad
.person__head__eye {}


// Good
.person__eye {}
```

**Your classes do not reflect the full paper-trail of the DOM.**


###Naming Conventions in HTML

Where naming conventions’ power really lies is in your markup. Take the following, non-naming-conventioned HTML:

```
<div class="box  profile  pro-user">

    <img class="avatar  image" />

    <p class="bio">...</p>

</div>
```

How are the classes **box** and **profile** related to each other? How are the classes **profile** and **avatar** related to each other? Are they related at all? Should you be using **pro-user** alongside **bio**? Will the classes **image** and **profile** live in the same part of the CSS? Can you use **avatar** anywhere else?

From that markup alone, it is very hard to answer any of those questions. Using a naming convention, however, changes all that:

```
<div class="box  profile  profile--is-pro-user">

    <img class="avatar  profile__image" />

    <p class="profile__bio">...</p>

</div>
```
Now we can clearly see which classes are and are not related to each other, and how; we know what classes we can’t use outside of the scope of this component; and we know which classes we may be free to reuse elsewhere.


###JavaScript Hooks

As a rule, it is unwise to bind your CSS and your JS onto the **same class** in your HTML, because you can’t have (or remove) one without (removing) the other. It is much cleaner, much more transparent, and much more maintainable to bind your JS onto specific classes.

Typically, these are classes that are prepended with `js-`, for example:

```
<input type="submit" class="btn  js-btn" value="Follow" />
```


##CSS Selectors

One of the most fundamental, critical aspects of writing maintainable and scalable CSS is selectors. Their specificity, portability, and reusability all have a direct impact on the mileage we will get out of our CSS, and the headaches it might bring us.


###Selector Intent

**To explicitly select the right thing for exactly the right reason.**

Let's say we want to style our website’s main navigation menu:

```
// Bad
header ul {}


// Good
.site-nav {}
```

`header ul {}` selector’s intent is to style any ul inside any header element. This is poor Selector Intent: a selector like this runs the risk of applying very specific styling to a very wide number of elements.

`.site-nav {}` is an unambiguous, explicit selector with good Selector Intent.

Poor Selector Intent is one of the biggest reasons for headaches on CSS projects. Writing rules that are far too greedy — and that apply very specific treatments via very far reaching selectors — causes unexpected side effects and leads to very tangled stylesheets, with selectors overstepping their intentions and impacting and interfering with otherwise unrelated rulesets.


###Reusability

With a move toward a more OOCSS approach to constructing UIs, reusability becomes crucial. We want the option to be able to move, recycle, duplicate, and syndicate components across our projects.

For this reason, **we use classes**. IDs, as well as being hugely over-specific, cannot be used more than once on any given page, whereas classes can be reused an infinite amount of times. **Everything you choose, from the type of selector to its name, should lend itself toward being reused.**


###Location Independence

Given the ever-changing nature of most UI projects, and the move to more component-based architectures, it is in our interests **not** to style things based on **where they are**, **but** on **what they are**. That is to say, our components’ styling should not be reliant upon where we place them — they should remain entirely location independent.

**A component shouldn’t have to live in a certain place to look a certain way.**

Let's say we need some buttons in our frontpage column blocks:
```
// Bad
// button restricted to a certain location
.btn--frontpage__columns {}


// Good
// button class reusable and relocatable
.btn--primary {}
.btn--medium {}
```


###Qualified Selectors

Definition: a qualified selector is when you specify both the element and the attribute, like so: `
input.button`.

Using qualified selectors in CSS is bad practice, because of the above explained reusability and portability issues. The class `.button` could be reused by for example `<a>`, `<button>` elements too.

####Quasi-Qualified Selectors

One thing that qualified selectors can be useful for is signalling where a class might be expected or intended to be used, for example:

```
ul.nav {}
```
Here we can see that the .nav class is meant to be used on a ul element, and not on a nav. By using quasi-qualified selectors we can still provide that information without actually qualifying the selector:

```
/*ul*/.nav {}
```
By commenting out the leading element, we can still leave it to be read, but avoid qualifying and increasing the specificity of the selector.


###Naming

Pick a name that is sensible, but somewhat ambiguous: aim for high reusability.

For example:
```
// Bad
// Tied to a very specific use case: they can only be used for that one thing.
.site-nav {}
.footer-links {}


// Good
// More ambiguous names, increased chance to reuse components in different circumstances
.primary-nav {}
.sub-links {}
```

Avoid using classes which describe the exact nature of the content and/or its use cases. 

For example:
```
// Bad
.btn--blue {} 

// Good
.btn--primary {}
```

**Further Reading**
[Naming UI components in OOCSS](http://csswizardry.com/2014/03/naming-ui-components-in-oocss/)


###Selector Performance

Selector performance is how quickly a browser can match the selectors your write in CSS up with the nodes it finds in the DOM.

Generally speaking, the longer a selector, the slower it is, for example:

```
// Bad
body.home div.header ul {}

// Good
.primary-nav {}
```

Browsers read CSS selectors **right-to-left**, so last selector - a.k.a. the **key selector** is the most critical.

The following selector might appear highly performant at first glance:
```
.foo * {}
```
The problem with this selector is that the key selector (`*`) is very, very far reaching. What this selector actually does is finds *every single node in the DOM* and then looks to see if it lives anywhere at any level within .foo. This is a very, very expensive selector, and should most likely be avoided or rewritten.

###Sum Up

1. **Select what you want explicitly**, rather than relying on circumstance or coincidence. Good Selector Intent will rein in the reach and leak of your styles.
2. **Write selectors for reusability**, so that you can work more efficiently and reduce waste and repetition.
3. **Do not nest selectors unnecessarily**, because this will increase specificity and affect where else you can use your styles.
4. **Do not qualify selectors unnecessarily**, as this will impact the number of different elements you can apply styles to.
5. **Keep selectors as short as possible**, in order to keep specificity down and performance up.




##Coming up: Specificity


##What now?

**Hungry after more?**
[Read Harry Roberts' original article here.](http://cssguidelin.es/)
