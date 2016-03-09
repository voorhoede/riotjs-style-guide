# RiotJS Style Guide

Opinionated *RiotJS Style Guide* for teams by [De Voorhoede](https://twitter.com/devoorhoede).


## Purpose

This guide provides a uniform way to structure your [RiotJS](http://riotjs.com/) code. Making it

* easier for developers / team members to understand and find things.
* easier for IDEs to interpret the code and provide assistance.
* easier to (re)use build tools you already use.
* easier to cache and serve bundles of code separately.

This guide is inspired by the [AngularJS Style Guide](https://github.com/johnpapa/angular-styleguide) by John Papa.


## Table of Contents

* [Module based development](#module-based-development)
* [Tag module names](#tag-module-names)
* [1 module = 1 directory](#1-module--1-directory)
* [Use `*.tag.html` extension](#use-taghtml-extension)
* [Use `<script>` inside tag](#use-script-inside-tag)
* [Keep tag expressions simple](#keep-tag-expressions-simple)
* [Keep tag options primitive](#keep-tag-options-primitive)
* [Assign `this` to `tag`](#assign-this-to-tag)
* [Put tag properties and methods on top](#put-tag-properties-and-methods-on-top)
* [Avoid fake ES6 syntax](#avoid-fake-es6-syntax)
* [Avoid `tag.parent`](#avoid-tagparent)
* [Put styles in external files](#put-styles-in-external-files)
* [Use tag name as style scope](#use-tag-name-as-style-scope)
* [Document your tag API](#document-your-tag-api)
* [Add a tag demo](#add-a-tag-demo)
* [More Resources](#more-resources)

## Module based development

Always construct your app out of small modules which do one thing and do it well. 

A module is a small self-contained part of an application. The RiotJS micro-framework is specifically designed to help you create *view-logic modules*, which Riot calls *tags*.

### Why?

Small modules are easier to learn, understand, maintain, reuse and debug. Both by you and other developers.

### How?

Each riot tag (like any module) must be [FIRST](https://addyosmani.com/first/): *Focused* ([single responsibility](http://en.wikipedia.org/wiki/Single_responsibility_principle)), *Independent*, *Reusable*, *Small* and *Testable*.

If your module does too much or gets too big, split it up into smaller modules which each do just on thing.
As a rule of thumb, try to keep each tag file less than 100 lines of code.
Also ensure your tag module works in isolation. For instance by adding a stand-alone demo.

[↑ back to Table of Contents](#table-of-contents)


## Tag module names

A tag module is a specific type of module, containing a Riot tag. 

Each module name must be:

* **Meaningful**: not overspecific, not overly abstract.
* **Short**: 2 or 3 words.
* **Pronouncable**: we want to be able talk about them.

Tag module names must also be:

* **Custom element spec compliant**: [include a hyphen](https://www.w3.org/TR/custom-elements/#concepts), don't use reserved names.
* **`app-` namespaced**: if very generic and otherwise 1 word, so that it can easily be reused in other projects.

### Why?

* The name is used to communicate about the module. So it must be short, meaningful and pronouncable.
* The `tag` element is inserted into the DOM. As such, they need to adhere to the spec.

### How?

```html
<!-- recommended -->
<app-header />
<user-list />
<range-slider />

<!-- avoid -->
<btn-group /> <!-- short, but unpronouncable. use `button-group` instead -->
<ui-slider /> <!-- all tags are ui elements, so is meaningless -->
<slider /> <!-- not custom element spec compliant -->
```

[↑ back to Table of Contents](#table-of-contents)


## 1 module = 1 directory

Bundle all files which construct a module into a single place.

### Why?

Bundling module files (Riot tags, tests, assets, docs, etc.) makes them easy to find, move and reuse.

### How?

Use the module name as directory name and file basename.
The file extension depends on the purpose of the file.

```
modules/
`--	my-example/
    |-- my-example.tag.html
	|--	my-example.less
	|--	...
	`--	README.md
```

If your project uses nested structures, you can nest a module within a module.
For example a generic `radio-group` module may be placed directly inside "modules/". While `search-filters` may only make sense inside a `search-form` and may therefore be nested:

```
modules/
|--	radio-group/
|   `-- radio-group.tag.html
`--	search-form/
    |-- search-form.tag.html
    |-- ...
    `-- search-filters/
        `-- search-filters.tag.html
```            

[↑ back to Table of Contents](#table-of-contents)


## Use `*.tag.html` extension

Riot introduces a new concept called *tags*, and suggests to use a `*.tag` extension.
However in essence these tags are simply custom elements containing markup. Therefore you should **use the `*.tag.html` extension**.

### Why?

* Tells developers it's not just HTML, but a Riot tag element.
* Improves IDE support (signals how to interpret).

### How?

In case of [in-browser compilation](http://riotjs.com/guide/compiler/#in-browser-compilation):
```html
<script src="path/to/modules/my-example/my-example.tag.html" type="riot/tag"></script>
```

In case of [pre-compilation](http://riotjs.com/guide/compiler/#pre-compilation), set the [custom extension](http://riotjs.com/guide/compiler/#custom-extension):
```bash
riot --ext tag.html modules/ dist/tags.js
```

[↑ back to Table of Contents](#table-of-contents)


## Use `<script>` inside tag

While Riot supports writing JavaScript inside a tag element [without a `<script>`](http://riotjs.com/guide/#no-script-tag),
you should **always use `<script>`** around scripting. This is closer to web standards and prevents confusing developers and IDEs. 

### Why?

* Prevents markup being interpreted as script.
* Improves IDE support (signals how to interpret).
* Tells developers where markup stops and scripting starts.

### How?

```html
<!-- recommended -->
<my-example>
	<h1>The year is { this.year }</h1>
	
	<script>
		this.year = (new Date()).getUTCFullYear();
	</script>
</my-example>

<!-- avoid -->
<my-example>
	<h1>The year is { this.year }</h1>
	
	this.year = (new Date()).getUTCFullYear();
</my-example>
```

[↑ back to Table of Contents](#table-of-contents)


## Keep tag expressions simple

Riot's inline [expressions](http://riotjs.com/guide/#expressions) are 100% Javascript. This makes them extemely powerful, but potentially also very complex. Therefore you should **keep tag expressions simple**.

### Why?

* Complex inline expressions are hard to read.
* Inline expressions can't be reused elsewehere. This can lead to code duplication and code rot.
* IDEs typically don't have support for expression syntax, so your IDE can't autocomplete or validate.

### How?

Move complex expressions to tag methods or tag properties. 

```html
<!-- recommended -->
<my-example>
	{ year() + '-' + month() }
	
	<script>
		const twoDigits = (num) => ('0' + num).slice(-2);
		this.month = () => twoDigits((new Date()).getUTCMonth() +1);
		this.year  = () => (new Date()).getUTCFullYear();
	</script>
</my-example>

<!-- avoid -->
<my-example>
	{ (new Date()).getUTCFullYear() + '-' + ('0' + ((new Date()).getUTCMonth()+1)).slice(-2) }
</my-example>
```

[↑ back to Table of Contents](#table-of-contents)


## Keep tag options primitive

Riot supports passing options to tag instances using attributes on tag elements. Inside the tag instance these options are available through `opts`. For example the value of `my-attr` on `<my-tag my-attr="{ value }" />` will be available inside `my-tag` via `opts.myAttr`. 

While Riot supports passing complex JavaScript objects via these attributes, you should try to **keep the tag options as primitive as possible**. Try to only use [JavaScript primitives](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) (strings, numbers, booleans) and functions. Avoid complex objects.

Exceptions to this rule are situations which can only be solved using objects (eg. collections or recursive tags) or well-known objects inside your app (eg. a product in a web shop).

### Why?

* By using an attribute for each option separately the tag has a clear and expressive API.
* By using only primitives and functions as option values our tag APIs are similar to the APIs of native HTML(5) elements. Which makes our custom elements directly familiar.
* By using an attribute for each option, other developers can easily understand what is passed to the tag instance.
* When passing complex objects it's not apparent which properties and methods of the objects are actually being used by the custom tags. This makes it hard to refactor code and can lead to code rot.

### How?

Use a tag attribute per option, with a primitive or function as value:

```html
<!-- recommended -->
<range-slider
	values="[10, 20]"
	min="0"
	max="100"
	step="5"
	on-slide="{ updateInputs }"
	on-end="{ updateResults }"
	/>
	
<!-- avoid -->
<range-slider config="{ complexConfigObject }">
```
```html
<!-- exception: recursive tag, like menu item -->
<menu-item>
	<a href="{ opts.url }">{ opts.text }</a>
	<ul if="{ opts.items }">
		<li each="{ item in opts.items }">
			<menu-item 
				text="{ item.text }" 
				url="{ item.url }" 
				items="{ item.items }" />
		</li>
	</ul>
</menu-item>
```

[↑ back to Table of Contents](#table-of-contents)


## Assign `this` to `tag`

Within the context of a Riot tag element, `this` is bound to the [tag instance](http://riotjs.com/api/#tag-instance).
Therefore when you need to reference it in a different context, ensure `this` is available as `tag`.

### Why?

* By assigning `this` to a variable named `tag` the variable tells developers it's bound to the [tag instance](http://riotjs.com/api/#tag-instance) wherever it's used.

### How?

```javascript
/* recommended */
// ES5: assign `this` to `tag` variable
var tag = this;
window.onresize = function() { 
    tag.adjust();
}

// ES6: assign `this` to `tag` constant
const tag = this;
window.onresize = function() { 
    tag.adjust();
}

// ES6: you can still use `this` with fat arrows
window.onresize = () => {
    this.adjust();
}

/* avoid */
var self = this;
var _this = this;
// etc
```

[↑ back to Table of Contents](#table-of-contents)


## Put tag properties and methods on top

Inside a Riot tag element you typically put its markup first, followed by its script.
Properties and methods bound to the tag (`this`) in the script are directly available in the markup. You should put those tag properties and methods alphabetized at the top of the script. Tag methods longer than a one-liner should be linked to separate functions later in the script.

### Why?

* Placing the tag properties and methods at the top of the script allows you to instantly identify which parts of the tag can be used in the markup.
* Alphabetizing the properties and methods makes them easy to find.
* By keeping each tag method or property declaration a one-liner you can get a full overview of the tag at a glance.
* By moving the full functions behind the tag methods down, you initially hide implementation details.

### How?

Put tag properties and methods on top:

```javascript
/* recommended: alphabetized properties then methods */
var tag = this;
tag.text = '';
tag.todos = [];
tag.add = add;
tag.edit = edit;
tag.toggle = toggle;

function add(event) {
    /* ... */
}

function edit(event) {
    /* ... */
}

function toggle(event) {
    /* ... */
}   

/* avoid: don't spread out tag properties and methods over script */
var tag = this;

tag.todos = [];
tag.add = function(event) {
    /* ... */
}

tag.text = '';
tag.edit = function(event) {
    /* ... */
}

tag.toggle = function(event) {
    /* ... */
}
```
    
Also put mixins and observables up top:

```javascript
/* recommended */
var tag = this;
// alphabetized properties
// alphabetized methods
tag.mixin('someBehaviour');
tag.on('mount', onMount);
tag.on('update', onUpdate);
// etc
```

[↑ back to Table of Contents](#table-of-contents)


## Avoid fake ES6 syntax

Riot supports a [shorthand *ES6 like* method syntax](http://riotjs.com/guide/#tag-syntax). Riot compiles the shorthand syntax `methodName() { }` into `this.methodName = function() {}.bind(this)`. Since this is non-standard you should **avoid fake ES6 method shorthand syntax**.

### Why?

* The fake ES6 shorthand syntax is non-standard and can therefore confuse developers.
* The tag scripts are not actual ES6 classes, so IDEs won't be able to interpret the fake ES6 class method syntax.
* It should always be clear which methods are bound to the tag and thus available in the markup. The shorthand syntax obscures the principle of writing code which is transparent and easy to understand.

### How? 

Use `tag.methodName =` instead of magic `methodName() { }` syntax:

```javascript
/* recommended */
var tag = this;
tag.todos = [];
tag.add = add;

function add() {
    if (tag.text) {
        tag.todos.push({ title: tag.text });
        tag.text = tag.input.value = '';
    }
}

/* avoid */
todos = [];

add() {
    if (this.text) {
        this.todos.push({ title: this.text });
        this.text = this.input.value = '';
    }
}
```

[↑ back to Table of Contents](#table-of-contents)


## Avoid `tag.parent`

Riot supports [nested tags](http://riotjs.com/guide/#nested-tags) which have access to their parent context through `tag.parent`. Accessing context outside your tag module violates the [FIRST](https://addyosmani.com/first/) rule of [module based development](#module-based-development). Therefore you should **avoid using `tag.parent`**.

The exception to this rule are anonymous child tags in a [for each loop](http://riotjs.com/guide/#loops) as they are defined directly inside the tag module.

### Why?

* A tag module, like any module, must work in isolation. If a tag needs to access its parent, this rule is broken.
* If a tag needs access to its parent, it can no longer be reused in a different context. 
* By accessing its parent a child tag can modify properties on its parent. This can lead to unexpected behaviour.

### How?

* Pass values from the parent to the child tag using attribute expressions.
* Pass methods defined on the parent tag to the child tag using callbacks in attribute expressions.

```html
<!-- recommended -->
<parent-tag>
	<child-tag value="{ value }" /> <!-- pass parent value to child -->
</parent-tag>

<child-tag>
	<span>{ opts.value }</span> <!-- use value passed by parent -->
	<script></script>
</child-tag>

<!-- avoid -->
<parent-tag>
	<child-tag />
</parent-tag>

<child-tag>
	<span>value: { parent.value }</span> <!-- don't do this -->
</child-tag>
```
```html
<!-- recommended -->
<parent-tag>
	<child-tag on-event="{ methodToCallOnEvent }" /> <!-- use method as callback -->
	<script>this.methodToCallOnEvent = () => { /*...*/ };</script>
<parent-tag>

<child-tag>
	<button onclick="{ opts.onEvent }"></button> <!-- call method passed by parent -->
</child-tag>

<!-- avoid -->
<parent-tag>
	<child-tag />
	<script>this.methodToCallOnEvent = () => { /*...*/ };</script>
<parent-tag>

<child-tag>
	<button onclick="{ parent.methodToCallOnEvent }"></button> <!-- don't do this -->
</child-tag>
```
```html
<!-- allowed exception -->
<parent-tag>
	<button each="{ item in items }"
		onclick="{ parent.onEvent }"> <!-- okay, because button is not a riot tag -->
		{ item.text }
	</button>
	<script>this.onEvent = (e) => { alert(e.item.text); }</script>
</parent-tag>
```

[↑ back to Table of Contents](#table-of-contents)


## Put styles in external files

For developer convenience, Riot allows you to define a tag element's style in a [nested `<style>` tag](http://riotjs.com/guide/#tag-styling). While you can [scope](http://riotjs.com/guide/#scoped-css) these styles to the tag element, Riot does not provide true encapsulation. Instead Riot extracts these styles from the tags (JavaScript) and injects them into the document on runtime. Since Riot compiles nested styles to JavaScript and doesn't have true encapsulation, you should instead **put styles in external files**.

### Why?

* External stylesheets can be handled by the browser independently of Riot and tag files. This means styles can be applied to initial markup even if JavaScript errors occur or isn't loaded (yet).
* External stylesheets can be used in combination with pre-processors (Less, Sass, PostCSS, etc) and your own (existing) build tools.
* External stylesheets can be minified, served and cached separately. This improves performance.
* Riot expressions are not supported in nested `<style>`s so there's no added benefit in using them.

### How?

Styles related to the tag and its markup, should be placed in a separate stylesheet file next to the tag file, inside its module directory:

```
my-example/
|-- my-example.tag.html
|-- my-example.(css|less|scss)    <-- external stylesheet next to tag file
`-- ...
```

[↑ back to Table of Contents](#table-of-contents)


## Use tag name as style scope

Riot tag elements are custom elements which can very well be used as style scope root.
Alternatively the module name can be used as CSS class namespace.

### Why?

* Scoping styles to a tag element improves predictability as its prevents styles leaking outside the tag element.
* Using the same name for the module directory, the Riot tag and the style root makes it easy for developers to understand they belong together.

### How?

Use the tag name as selector, as parent selector or as namespace prefix (depending on your CSS naming strategy):

```css
/* recommended */
my-example { }
my-example li { }
.my-example__item { }

/* avoid */
.my-alternative { } /* not scoped to tag or module name */
.my-parent .my-example { } /* .my-parent is outside scope, so should not be used in this file */
```

[↑ back to Table of Contents](#table-of-contents)


## Document your tag API

A Riot tag instance is created by using the tag element inside your application. The instance is configured through its custom attributes. For the tag to be used by other developers, these custom attributes - the tag's API - should be documented in a `README.md` file.

### Why?

* Documentation provides developers with a high level overview to a module, without the need to go through all its code. This makes a module more accessible and easier to use.
* A tag's API is the set of custom attributes through which its configured. Therefore these are especially of interest to other developers which only want to consume (and not develop) the tag.
* Documentation formalises the API and tells developers which functionality to keep backwards compatible when modifying the tag's code.
* `README.md` is the de facto standard filename for documentation to be read first. Code repository hosting services (Github, Bitbucket, Gitlab etc) display the contents of the the README's, directly when browsing through source directories. This applies to our module directories as well.
  
### How?

Add a `README.md` file to the tag's module directory:

```
range-slider/
|-- range-slider.tag.html
|--	range-slider.less
`--	README.md
```
	
Within the README file, describe the functionality and the usage of the module. For a tag module its most useful to describe the custom attributes it supports as those are its API:

```markdown
# Range slider

## Functionality

The range slider lets the user to set a numeric range by dragging a handle on a slider rail for both the start and end value.

This module uses the [noUiSlider](http://refreshless.com/nouislider/) for cross browser and touch support.

## Usage

`<range-slider>` supports the following custom tag attributes:

| attribute | type | description
| --- | --- | ---
| `min` | Number | number where range starts (lower limit).
| `max` | Number | Number where range ends (upper limit).
| `values` | Number[] *optional* | Array containing start and end value.  E.g. `values="[10, 20]"`. Defaults to `[opts.min, opts.max]`.
| `step` | Number *optional* | Number to increment / decrement values by. Defaults to 1.
| `on-slide` | Function *optional* | Function called with `(values, HANDLE)` while a user drags the start (`HANDLE == 0`) or end (`HANDLE == 1`) handle. E.g. `on-slide={ updateInputs }`, with `tag.updateInputs = (values, HANDLE) => { const value = values[HANDLE]; }`.
| `on-end` | Function *optional* | Function called with `(values, HANDLE)` when user stops dragging a handle.

For customising the slider appearance see the [Styling section in the noUiSlider docs](http://refreshless.com/nouislider/more/#section-styling).
```

[↑ back to Table of Contents](#table-of-contents)


## Add a tag demo

Add a `*.demo.html` file with demos of the tag with different configurations, showing how the tag can be used.

### Why?

* A tag demo proves the module works in isolation.
* A tag demo gives developers a preview before having to dig into the documentation or code.
* Demos can illustrate all the possible configurations and variations a tag can be used in. 

### How?

Add a `*.demo.html` file to your module directory:

```
city-map/
|--	city-map.tag.html
|--	city-map.demo.html
|--	city-map.css
`--	...
```

Inside the demo file:

* Include `riot+compiler.min.js` to also compile during runtime.
* Include the tag file (e.g. `./city-map.tag.html`).
* Create a `demo` tag (`<yield/>`) to embed your demos in (otherwise option attributes are not supported).
* Write demos inside the `<demo>` tags.
* As a bonus add `aria-label`s to the `<demo>` tags and style those as title bars.
* Initialise using `riot.mount('demo', {})`.

Example demo file in `city-tag` module:

```html
<!-- modules/city-map/city-map.demo.html: -->
<body>
    <h1>city-map demos</h1>
    
    <demo aria-label="City map of London">
        <city-map location="London" />    
    </demo>
    
    <demo aria-label="City map of Paris">
        <city-map location="Paris" />    
    </demo>
    
    <link rel="stylesheet" href="./city-map.css">
    
    <script src="path/to/riot+compiler.min.js"></script>
    <script type="riot/tag" src="./city-map.tag.html"></script> 
    <script>
        riot.tag('demo','<yield/>');
        riot.mount('demo', {});
    </script>
    
    <style>
    	/* add a grey bar with the `aria-label` as demo title */
    	demo:before {
        	content: "Demo: " attr(aria-label);
	        display: block;
        	background: #F3F5F5;
        	padding: .5em;
        	clear: both;
        }
    </style>
</body>
```
Note: this is a working concept, but could be much cleaner using build scripts.

[↑ back to Table of Contents](#table-of-contents)

## More Resources

For more information, take a look at these other resources:

* [RiotJS Guide](http://riotjs.com/guide/) on riotjs.com
* [RiotJS API](http://riotjs.com/api/) on riotjs.com
* [Learning RiotJS](https://egghead.io/playlists/learning-riotjs) video series on egghead.io

[↑ back to Table of Contents](#table-of-contents)
