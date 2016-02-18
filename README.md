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

* [1 module = 1 directory](#1-module--1-directory)


## 1 module = 1 directory

Bundle all files which construct a module into a single place.

**Why?**

Bundling module files (Riot tags, tests, assets, docs, etc.) makes them easy to find, move and reuse.

**How?**

Use the module name as directory name and file basename.
The file extension depends on the purpose of the file.

	modules/
		my-example/
			my-example.tag.html
			my-example.less
			...
			README.md
