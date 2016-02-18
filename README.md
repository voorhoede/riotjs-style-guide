# RiotJS Style Guide

Opinionated *RiotJS Style Guide* for teams by [De Voorhoede](https://twitter.com/devoorhoede).


## Purpose

This guide provides a uniform way to structure your RiotJS code. Making it

* easier for developers / team members to understand and find things.
* easier for IDEs to interpret the code and provide assistance.
* easier to (re)use build tools you already use.
* easier to cache and serve bundles of code separately.

This guide is inspired by [AngularJS Style Guide](https://github.com/johnpapa/angular-styleguide) by John Papa.


## Table of Contents

* [Module based development](#module-based-development)


## Module based development

Always construct your app out of small modules which do one thing and do it well. 

### Why?
Small modules are easier to learn, understand, maintain, reuse and debug. Both by you and other developers.

### How?
Riot tags fit well in the *Module based development* philosophy. Just like any module - each Riot tag - must be [FIRST](https://addyosmani.com/first/): *Focused* ([single responsibility](http://en.wikipedia.org/wiki/Single_responsibility_principle)), *Independent*, *Reusable*, *Small* and *Testable*.
 
Ensure your component works in isolation. For instance by adding a stand-alone demo.
