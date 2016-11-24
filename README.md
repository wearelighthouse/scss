# Lighthouse SCSS Lint

Linting SCSS for consistency - and victory!

1. [Installation](#1-installation)
2. [Styles](#styles)
	- [Formatting](#formatting)
	- [Comments](#comments)
	- [BEM](#bem)
	- [ID selectors](#id-selectors)
	- [JavaScript selectors](#javascript-selectors)
3. [Sass](#sass)
	- [Syntax](#syntax)
	- [Ordering of property declarations](#ordering-of-property-declarations)
	- [Variables](#variables)
	- [Mixins](#mixins)
	- [Extend directive](#extend-directive)
	- [Nested selectors](#nested-selectors)
  - [Nested modifiers](#nested-modifiers)

## 1. Installation

### Sass Lint

Regardless of what text editor/IDE you use you will need to install the [sass-lint](https://www.npmjs.com/package/sass-lint) package.

Use the following command to install it globally:

`npm install -g sass-lint`

You will also need to place [.sass-lint.yml](.scss-lint.yml) in the root of your project.

Once thats done you can then lint files with the following command:

`sass-lint -c .scss-lint.yml '**/*.scss' -v -q`

This is cumbersome so you should probably install one of the bellow!

### Atom

You'll need to have [Linter](https://atom.io/packages/linter) installed to use [linter-sass-lint](https://atom.io/packages/linter-sass-lint).

`apm install linter-sass-lint`

### Sublime

Follow these [instructions](https://packagecontrol.io/packages/SublimeLinter-contrib-scss-lint) to get setup with Sublime Text.

## 2. Styles

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

* Make use of both line comments and block comments for given situations.


**Usage**
```
// For inline explanations

/**
 * For documentation
 */
```

### BEM

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript selectors

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

* Use class names for styling only _not_ for js binding.

**Helper**

```
$.js = function(el) {
  return $('[data-js=' + el + ']');
};
```

**Usage**
```
<div data-js="gallery">
 ...
</div>

var $gallery = $.js('gallery');
```

See https://toddmotto.com/data-js-selectors-enhancing-html5-development-by-separating-css-from-javascript/.

## 3. Sass

### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax

### Ordering of property declarations

1. `@include` declarations - Ordered by [property groups](.scss-lint.yml#L133).
2. Property declarations
3. Nested selectors

```
.tab {
  @include border();

  border-bottom: 0px;

  a {
    ...
  }
}
```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behaviour, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Nested selectors

**Avoid nesting selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // Avoid going deeper!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable

**However there will be times where this is necessary!**

**Russell Bishop**

> It probably won't be your favourite bit of code either, but if you're writing it, you're probably unable to get around.

### Nested modifiers

When nesting modifiers like `@media` and `:hover` it is imporant that any nested selectors appear in the appropriate order.

**Modifier doesn't affect children**

```
.parent {
  &:hover {
    // .parent specific modifiers
  }

  &__child {
    ...
  }
}
```

**Modifier affects children**

```
.parent {
  &__child {
    ...  
  }

  &:hover {
    &__child {
      ...
    }
  }
}
```

### Ordering of modifiers

1. Element states
2. [Modifiers](https://en.bem.info/methodology/naming-convention/#modifier-name)
3. Media queries

```
.link {
  
  &:hover {
    ...
  }

  &--active {
    ...
  }

  @media only sceen {
    ...
  }
}
```