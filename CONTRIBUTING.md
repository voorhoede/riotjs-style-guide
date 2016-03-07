# Contributing

For new additions or changes to the guide, create a branch and submit a Pull Request.
Only add/change 1 style guide rule per Pull Request.
The Pull Request serves as a place to discuss and refine the additions/changes.

## Format

Use [Github Flavoured Markdown](https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown).

Use short and descriptive rule names as 2nd level heading:

```markdown
## Keep expressions simple
```
  
Optionally add an short summary for the rule.

```markdown
RiotJS allows any type of JavaScript expression as inline expression (`{ expression }`).
```

Describe **why** the rule exists. What purpose does it serve?

```markdown
### Why?

Keep inline expressions simple because:

* complex inline expressions are hard to read
* inline expressions cant be reused
```

Describe **how** (and how not) to apply the rule.

```markdown
### How?

If an expression becomes complex, move it to a tag property or method.
```

When using code snippets:
* use <code>```html</code> for improved syntax highlighting of Riot tag elements. 
* use <code>```javascript</code> for improved syntax highlighting when script only.
* use `<!-- recommended -->` and `/* recommended */` or `<!-- avoid -->` and `/* avoid */` at the start of the snippet to indicate if it's a good or bad practice.

For example:

```html
<!-- recommended -->
<datetime>{ this.timestamp() }</datetime>

<!-- avoid -->
<datetime>
    { ((new Date()).getUTCMonth()+1) + ' ' + (new Date()).getUTCFullYear() }
</datetime>
```

Add a [↑ back to Table of Contents](README.md#table-of-contents) link at the end of each rule, so the reader can easily navigate back:

```markdown
[↑ back to Table of Contents](#table-of-contents)
```
