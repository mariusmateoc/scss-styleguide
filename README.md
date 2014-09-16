SCSS-Styleguide
====================

Personal coding standards for crafting SCSS

My goal is to update this as my knowledge evolves and promote more consistency in my own code.


## Guidelines
* Prefer one global stylesheet. ex. `main.scss`
* Include a link at the top of the stylesheet back to these styleguides.
* Prefix all javascript-based selectors, (IDs, Classes) with `js-`. [Source](https://github.com/styleguide/javascript)
* Never reference `js-` prefixed class names from CSS files. `js-` base classes are to be used exclusively in JS files. [Source](https://github.com/styleguide/css)
* Use the `is-` prefix for state rules that are shared between CSS and JS.
* Each style that is applied directly to an element will go in _normalize.scss.


## Formatting SCSS

* Prefer soft-tabs with 2 space indent.
* Put spaces after `: ` in property declarations.
* Put spaces before `{` in rule declarations.
* Put line breaks between rulesets.
* When grouping selectors, keep individual selectors to a single line.
* Place closing braces of declaration blocks on a new line.
* Each declaration should appear on its own line for more accurate error reporting.
* Avoid specifying units for zero values, e.g., margin: 0; instead of margin: 0px;
* Use lowercase format for property and value names.
* Omit leading “0”s in values. `font-size: .8em;`
* Add one blank line bewteen rule sets.

These standards have been adopted from Chris Coyier's Sass Style Guide article. [Source](http://css-tricks.com/sass-style-guide/)

* List extends first.
* List includes next, unless they are media queries.
* List regular styles next.
* List include media queries next.
* List nested selectors last.
* Use same order inside nested selectors.
* Add a line before nested selectors.


**Formatting Example:**
```scss
.block {
  @extend %silent;
  @include border-radius(2px);
  margin: 10px;
  padding: 10px;
  border: 1px solid red;
  background: blue;
  color: white;
  font-size: 16px;
  -webkit-transform: translate3d(0,0,0);

  @include breakpoint($small) {
    width: 25%;
  }

  &--modifier {
    font-size: 20px;
  }

  &__element {
    margin: 0;
  }

  &__element--modifier {
    font-size: 2rem;
  }
}
```

## File Stucture

Currently, the [most efficient way to serve CSS for multiple devices is via 1 stylesheet](http://andydavies.me/blog/2012/12/29/think-twice-before-using-matchmedia-to-conditionally-load-stylesheets/). Try to keep all styles in one stylesheet to avoid additional HTTP requests.

**How my `main.scss` looks?**

```scss
/* Learn about styleguide | Link to styleguide document */

/* ==========================================================================
   Tools
   ========================================================================== */

@import "compass",  /* http://compass-style.org/ */
        "susy",  /* http://susy.oddbird.net/ */
        "breakpoint";  /* http://breakpoint-sass.com/ */

/* ==========================================================================
   Globals
   ========================================================================== */

@import "partials/variables",
        "partials/functions",  /* If there are any */
        "partials/mixins";  /* If there are any */

/* ==========================================================================
   Main Styles
   ========================================================================== */

@import "partials/normalize",  /* https://github.com/mariusmateoc/normalize.scss */
        "partials/module-name",
        "partials/utils",
        "partials/print";
```

**Naming Conventions**

There are [various naming conventions](http://www.brettjankord.com/2013/03/06/more-thoughts-on-html-class-naming-conventions/). Below is my preferred naming convention for block, modifiers, and subcomponents/element. Learn the easy way to use [BEM](https://bem.info/) with [SASS](http://sass-lang.com/) [HERE](http://alwaystwisted.com/post.php?s=2014-02-27-even-easier-bem-ing-with-sass-33)

```scss
.block {...}
.block--modifier {...}
.block__element {...}
.block__element--modifier {...}
.js-*  /* Prefix JavaScript states */
.is-*  /* Prefix for state rules that are shared between CSS and JS. */
```

**How to structure a new module?**

Inspired by [Geek Roket](http://geek-rocket.de/frontend-development/scss-styleguide-with-bem-oocss-smacss/)

* Area for Config. Where I define colors, silent clases etc...
* Base area is where I write the styles for the module


```scss
/* ==========================================================================
   Config
   ========================================================================== */

  $color-primary: $blue;
  $color-secondary: $white;


  %silent-class {
    color: $color-primary;
    box-shadow: 0 0 1rem gold;
  }


/* ==========================================================================
   Base
   ========================================================================== */

  .button {
    @extend %silent-class;


    &--primary {
      @extend %silent-class;
      background: $color-primary;

      &.is-disabled {
        cursor: not-alowed;
      }

      &:hover {
        background: darken($color-primary, 10%);
      }
    }


    &--secondary {
      @extend %silent-class;
      background: $color-secondary;

      &:hover {
        border: 2px solid darken($color-secondary, 10%);
      }
    }
  }
```

## Comments & Documentation

I've adopted [HTML5 Boilerplate](http://html5boilerplate.com/) commenting guidelines.

* Place comments on a new line above their subject.
* Keep line-length to a sensible maximum, e.g., 80 columns.
* Create a comment for each main section, base, layout, modules, and state.


**Comment types example inspired by HTML5 Boilerplate comments**
```
/* ==========================================================================
   Main section comment block
   ========================================================================== */


/* Sub-section comment block
   ========================================================================== */


/*
 * The first sentence of the long description starts here and continues on this
 * line for a while finally concluding here at the end of this paragraph.
 *
 * The long description is ideal for more detailed explanations and
 * documentation. It can include example HTML, URLs, or any other information
 * that is deemed necessary or useful.
 */


/* Basic comment */
```

### Scss File Structure

For small project I like to keep it simple

```bash
scss
   ├── main.scss
   ├── partials
   │  ├── _normalize.scss
   │  ├── _variables.scss
   │  ├── _functions.scss
   │  ├── _mixins.scss
   │  ├── _module-name.scss
   │  ├── _utils.scss
   │  ├── _syntax.scss
   │  └── _print.scss
   └── readme.md
```

Alternative folder organization for big projects

```bash
scss
   ├── main.scss
   ├── globals
   │  ├── _mixins.scss
   │  ├── _variables.scss
   │  ├── _functions.scss
   ├── vendors
   │  ├── _normalize.scss
   │  ├── _syntax.scss
   ├── modules
   │  ├── _buttons.scss
   │  ├── _tabs.scss
   ├── utils
   │  ├── _utils.scss
   ├── print
   │  ├── _print.scss
   ├── states (mainly JS states, other states should follow the styles they are affecting)
   └── readme.md
```
