# A Lighthouse Style Guide: SCSS

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
4. [Framework](#framework)

## 1. Installation

For both `sass-lint` and `scss-lint` you should place the linter config in the root of your project. You can find the appropriate config in [`resources`](resources).

**N.B.** you only need to install one of the below!

### [`sass-lint`](https://www.npmjs.com/package/sass-lint)

Use the following command to add `sass-lint` as a project `devDependency`:

`npm install --save-dev sass-lint`

Once that is done you can then lint files with the following command:

`./node_modules/.bin/sass-lint -c .sass-lint.yml '**/*.scss' -v -q`

This is cumbersome so you should probably install a linter package for your favourite text editor.

#### Sublime Text

Follow these [instructions](https://packagecontrol.io/packages/SublimeLinter-contrib-sass-lint) to get setup with Sublime Text.

### [`scss-lint`](https://github.com/brigade/scss-lint)

Use the following commad to install `scss-lint`:

`gem install scss_lint`

Once that is done you canthen lint files with the following commad:

`scss-lint path/to/your/scss/**/*.scss`

Again...this is cumbersome so you should probably install a linter package for your favourite text editor.

#### Sublime Text

Follow these [instructions](https://packagecontrol.io/packages/SublimeLinter-contrib-scss-lint) to get stup with Sublime Text.

## 2. Styles

### Formatting

* Use soft tabs (2 spaces) for indentation.
* Use dashes in class names.
  - Underscores and PascalCasing are okay if you are using BEM (see [BEM](#bem) below).
* Do not use ID selectors.
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations.
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line.
* Put blank lines between rule declarations.

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-dont-even {
  // ...
}
```

**Good**

```css
.avatar {
  margin: 10px;
  border-radius: 50%;
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
  
We use BEM to build up `blocks` - like a header, a sidebar, a 'content' block. Anything that isn't covered by the global /library/ styles found in places like `_forms.scss`, or the single-use classes we have in place for typography or icons etc.

[How we use BEM in Blocks](/resources/scss/blocks)

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

### Ordering of property declarations (property-sort-order)

1. `@include` declarations - Ordered by [property groups](.scss-lint.yml#L133).
2. `@extend` declarations - Ordered by [property groups](.scss-lint.yml#L133).
2. Property declarations - Ordered by [property groups](.scss-lint.yml#L133).
3. Nested selectors

```
.c-tab {
  @include border();
  border-bottom-width: 1px;

  .tab__button {
    ...
  }
}
```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this.

Mixins can often be replaced with a generic set of classes like an Object (for example, .o-aspect-ratio) with a few variations.

Ensure that your use of a mixin is not adding repetitive CSS in your compiled CSS.

### Nested selectors

**Avoid nesting selectors more than three levels deep!**

```scss
.profile {

  &:hover,
  &:focus {
    .profile__face {

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
> Usually this happens when you're writing some hovers styles for a child element, even a grandchild element - for example:

```scss
.table {

    &:hover,
    &:focus {
        .row {
            opacity: .2;
            // other
        }

        .row {
            &:hover,
            &:focus {
                opacity: 1;
                // other
            }
        }
    }
}
```

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

## 3. Framework

When building a project's CSS, you should start with the files found at [wearelighthouse/iotaCSS](https://github.com/wearelighthouse/iotacss).

This is our own version of the IotaCSS framework [IotaCSS Docs](http://iotacss.com/docs) - which you're also welcome to use.
