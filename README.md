# RiotJS Style Guide

Opinionated *RiotJS Style Guide* for teams by [De Voorhoede](https://twitter.com/devoorhoede).


## Purpose

This guide provides a uniform way to structure your RiotJS code. Making it

* easier for developers / team members to understand and find things.
* easier for IDEs to interpret the code and provide assistance.
* easier to (re)use build tools you already use.
* easier to cache and serve bundles of code separately.

This guide is inspired by [AngularJS Style Guide](https://github.com/johnpapa/angular-styleguide) by John Papa.


## Module based development

Riot fits well in the *Module based development* philosophy.

Just like any module - each Riot component - must be [FIRST](https://addyosmani.com/first/):

* **F**ocused (single responsibility)
* **I**ndependent
* **R**eusable
* **S**mall
* **T**estable
