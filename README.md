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

1. [Component / Tag names](#component-tag-names)

## Component / Tag names

Component or tag names need to be:

* **Spec compliant**: [include a hyphen](https://www.w3.org/TR/custom-elements/#concepts).
* **Meaningful**: not overspecific, not overly abstract.
* **Short**: 2 or 3 words.
* **Pronouncable**: we want to be able talk about them.
* **`app` namespaced**: using a standard namespace allows easy reuse in other projects

*Why?*: Your `tag`s are custom elements in the document. As such, they need to adhere the spec.

```markup
<!-- recommended -->
<app-header />
<app-user-list />
<app-ui-slider />
```
