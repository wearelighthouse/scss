# Lighthouse SCSS Lint

Linting SCSS for consistency - and victory!

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Installation](#installation)
	- [[Node Sass Lint](https://www.npmjs.com/package/sass-lint)](#node-sass-linthttpswwwnpmjscompackagesass-lint)
	- [[Atom linter-sass-lint](https://atom.io/packages/linter-sass-lint)](#atom-linter-sass-linthttpsatomiopackageslinter-sass-lint)
	- [[Sublime​Linter-contrib-scss-lint](https://packagecontrol.io/packages/SublimeLinter-contrib-scss-lint)](#sublimelinter-contrib-scss-linthttpspackagecontroliopackagessublimelinter-contrib-scss-lint)
- [Styles](#styles)
	- [Formatting](#formatting)
	- [Comments](#comments)
	- [BEM](#bem)
	- [ID selectors](#id-selectors)
	- [JavaScript selectors](#javascript-selectors)
- [Sass](#sass)
	- [Syntax](#syntax)
	- [Ordering of property declarations](#ordering-of-property-declarations)
	- [Variables](#variables)
	- [Mixins](#mixins)
	- [Extend directive](#extend-directive)
	- [Nested selectors](#nested-selectors)

<!-- /TOC -->

## Installation

### [Node Sass Lint](https://www.npmjs.com/package/sass-lint)

Install the npm package globally:

```bash
npm install -g sass-lint
```

### [Atom linter-sass-lint](https://atom.io/packages/linter-sass-lint)

You'll need to have [Linter](https://atom.io/packages/linter) installed to use this plugin.

```
apm install linter-sass-lint
```

### [Sublime​Linter-contrib-scss-lint](https://packagecontrol.io/packages/SublimeLinter-contrib-scss-lint)

[Instructions](https://packagecontrol.io/packages/SublimeLinter-contrib-scss-lint)

## Styles

### Formatting

* Use soft tabs (2 spaces) for indentation
* Prefer dashes over camelCasing in class names.
  - Underscores and PascalCasing are okay if you are using BEM (see [BEM](#bem) below).
* Do not use ID selectors
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line
* Put blank lines between rule declarations

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

<!-- TODO: rules for comments  -->

### BEM

<!-- TODO: Rules and links for BEM -->

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript selectors

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

<!-- TODO: Standards for DOM js bindings -->

## Sass

### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax

### Ordering of property declarations

1. Property declarations

Follow the order spcified by the `.scss-lint.yml`

2. `@include` declarations

<!-- TODO: Specify mixin <-> property order -->

3. Nested selectors

<!-- TODO: Specify nested selector property order -->

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behaviour, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Nested selectors

<!-- TODO: Specify max slector depth (if any) and BEM< depth, eg: -->
**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable
