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

* [Use `<script>` inside tag](#use-script-inside-tag)


## Use `<script>` inside tag

While Riot supports writing JavaScript inside a tag element [without a `<script>`](http://riotjs.com/guide/#no-script-tag),
you should **always use `<script>`** around scripting. This is closer to web standards and prevents confusing developers and IDEs. 

**Why?**

* Prevents markup being interpreted as script.
* Improves IDE support (signals how to interpret).
* Tells developers where markup stops and scripting starts.

**How?**

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
