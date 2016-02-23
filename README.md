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
* [Use `*.tag.html` extension](#use-taghtml-extension)
* [Use `<script>` inside tag](#use-script-inside-tag)


## Module based development

Always construct your app out of small modules which do one thing and do it well. 

A module is a small self-contained part of an application. The RiotJS micro-framework is specifically designed to help you create *view-logic modules*, which Riot calls *tags*.

### Why?

Small modules are easier to learn, understand, maintain, reuse and debug. Both by you and other developers.

### How?

Each riot tag (like any module) must be [FIRST](https://addyosmani.com/first/): *Focused* ([single responsibility](http://en.wikipedia.org/wiki/Single_responsibility_principle)), *Independent*, *Reusable*, *Small* and *Testable*.

If your module does too much or gets too big, split it up into smaller modules which each do just on thing.
Also ensure your tag module works in isolation. For instance by adding a stand-alone demo.


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
