# CSS Styleguide

Welcome to the Designkitchen CSS Styleguide. It's pretty rad. Before reading this, you should have a general understanding for **specificity** and the [**SCSS**][1] syntax.

Compiled and taken from:
* https://github.com/styleguide/css
* https://github.com/necolas/idiomatic-css
* https://github.com/anthonyshort/idiomatic-sass
* https://github.com/csswizardry/CSS-Guidelines

## General principles

* All code in any code-base should look like a single person typed it, no
  matter how many people contributed.
* Strictly enforce the agreed-upon style.
* If in doubt when deciding upon a style, use existing, common patterns.

## Coding Style

*   Use hyphen delimited class names
*   Use soft-tabs with a four (4) space indent.
*   Use one discrete selector per line in multi-selector rulesets.
*   Put spaces after `:` in property declarations.
*   Put spaces before `{` in rule declarations.
*   Include one declaration per line in a declaration block.
*   Use hex color codes `#000` unless using rgba.
*   Use single or double quotes consistently. Preference is for double quotes,
  e.g., `content: ""`.
*   Quote attribute values in selectors, e.g., `input[type="checkbox"]`.
* _Where allowed_, avoid specifying units for zero-values, e.g., `margin: 0`.
* Include a space after each comma in comma-separated property or function
  values.
* Include a semi-colon at the end of the last declaration in a declaration
  block.
* Place the closing brace of a ruleset in the same column as the first
  character of the ruleset.
* Separate each ruleset by a blank line.
*   Use `//` for comment blocks (instead of `/* */`).

Here is good example syntax:

    .selector-1,
    .selector-2,
    .selector-3[type="text"] {
        -webkit-box-sizing: border-box;
        -moz-box-sizing: border-box;
        box-sizing: border-box;
        display: block;
        font-family: helvetica, arial, sans-serif;
        color: #333;
        background: #fff;
        background: linear-gradient(#fff, rgba(0, 0, 0, 0.8));
    }

    .selector-a,
    .selector-b {
        padding: 10px;
    }

#### Declaration order

If declarations are to be consistently ordered, it should be in accordance with
a single, simple principle. My preference is for structurally important
properties (e.g. positioning and box-model) to be declared prior to all
others.

    .selector {
        // Positioning
        position: absolute;
        z-index: 10;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;

        // Display & Box Model
        display: inline-block;
        overflow: hidden;
        box-sizing: border-box;
        width: 100px;
        height: 100px;
        padding: 10px;
        border: 10px solid #333;
        margin: 10px;

        // Other
        background: #000;
        color: #fff;
        font-family: sans-serif;
        font-size: 16px;
        text-align: right;
    }

#### Exceptions and slight deviations

Large blocks of single declarations can use a slightly different, single-line
format. In this case, a space should be included after the opening brace and
before the closing brace.

    .selector-1 { width: 10%; }
    .selector-2 { width: 20%; }
    .selector-3 { width: 30%; }

Long, comma-separated property values - such as collections of gradients or
shadows - can be arranged across multiple lines in an effort to improve
readability and produce more useful diffs. There are various formats that could
be used; one example is shown below.

    .selector {
        background-image:
            linear-gradient(#fff, #ccc),
            linear-gradient(#f3c, #4ec);
        box-shadow:
            1px 1px 1px #000,
            2px 2px 1px 1px #ccc inset;
    }


## SCSS Style

*   Any `$variable` or `@mixin` that is used in more than one file should be put in `globals/`. Others should be put at the top of the file where they're used.
*   As a rule of thumb, don't nest further than 3 levels deep. If you find yourself going further, think about reorganizing your rules (either the specificity needed, or the layout of the nesting).
* Avoid large numbers of nested rules. Break them up when readability starts to
  be affected. Preference to avoid nesting that spreads over more than 20
  lines.
* Consider prefixing custom functions with `x-` or another namespace. This
  helps to avoid any potential to confuse your function with a native CSS
  function, or to clash with functions from libraries.

### Ordering

1. `$variable` should **always** appear at the top.
2. `@extend` should always appear before properties. It's like extending a class in Ruby.
3. `@include` should appear second. This allows the properties to override the mixins.
4. Properties should appear after this, optionally grouped by type or sorted alphabetically.
5. Mixins with content blocks should appear next. `@include someMixin { properties }`
6. Selectors that target itself. `&.modifier`
7. Child selectors appear last.

The basic rule of thumb is at-rules, properties, then blocks.

## File Organization

* Each logical module of code should belong in its own file. Avoiding putting multiple objects in the same file. This allows you to use the filesystem to navigate your Sass rather than relying on comment blocks.
* Mixins/placeholders/functions should, if possible, belong in their own file.
* Files should be named for the component they are housing. A `block-list` object will live in a `block-list.scss` file.

In general, the CSS file organization should follow something like this:

    styles
    ├── components
    │   ├── comments.scss
    │   ├── block-list.scss
    │   └── listings.scss
    ├── defaults
    │   ├── browser_helpers.scss
    │   ├── responsive_helpers.scss
    │   ├── normalize.scss
    │   ├── reset.scss
    │   ├── functions.scss
    │   ├── print.scss
    │   ├── variables.scss
    ├── plugins
    │   ├── jquery.fancybox-1.3.4.css
    │   └── flexslider.scss
    ├── sections
    │   ├── home.scss
    │   ├── landing.scss
    └── shared
        ├── forms.scss
        └── headings.scss


Use [Sprockets][3] to **require** files. However, you should explicitly **import** any scss that does not generate styles (`globals/`) in the particular SCSS file you'll be needing it's helpers in. Here's a good example:

    //= require_tree ./plugins
    //= require my_awesome_styles

    @import "../globals/basic";

    .rule { ... }


## Pixels vs. Ems

Font sizes should be set in rems with a pixel fallback. This gives the accessibility
benefits of ems with the confidence of pixels. Here is a handy Sass mixin to
work out a rem and pixel fallback for you (assuming you set your base font
size in a variable somewhere):

    @mixin font-size($font-size){
        font-size:$font-size +px;
        font-size:$font-size / $base-font-size +rem;
    }

I only use pixels for items whose dimensions were defined before the came into
the site. This includes things like images and sprites whose dimensions are
inherently set absolutely in pixels.

Additionally, unit-less `line-height` is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the `font-size`.

## Layout

All components you build should be left totally free of widths; they should always remain fluid and their widths should be governed by a parent/grid system.

Heights should **never** be be applied to elements. Heights should only be
applied to things which had dimensions _before_ they entered the site (i.e.
images and sprites). Never ever set heights on `p`s, `ul`s, `div`s, anything.
You can often achieve the desired effect with `line-height` which is far more
flexible.

Grid systems should be thought of as shelves. They contain content but are not
content in themselves. You put up your shelves then fill them with your stuff.
By setting up our grids separately to our components you can move components
around a lot more easily than if they had dimensions applied to them; this makes
our front-ends a lot more adaptable and quick to work with.

You should never apply any styles to a grid item, they are for layout purposes
only. Apply styling to content _inside_ a grid item. Never, under _any_
circumstances, apply box-model properties to a grid item.

## Class naming conventions

Always ensure classes are sensibly named; keep them as short as possible but as long as necessary. Ensure any objects or abstractions are very vaguely named (e.g. `.ui-list`, `.media`) to allow for greater reuse.

If you need to bind to some markup use a JS specific CSS class. This is simply a class namespaced with `.js-`, e.g. `.js-toggle`, `.js-drag-and-drop`. This means that we can attach both JS and CSS to classes in our markup but there will never be any troublesome overlap.

    <th class="is-sortable  js-is-sortable">
    </th>

Never reference `js-` prefixed class names from CSS files. `js-` are used exclusively from JS files.

Use the `is-` prefix for state rules that are shared between CSS and JS.

**Remember:** classes are neither semantic or insemantic; they are sensible or
insensible! Stop stressing about ‘semantic’ class names and pick something
sensible and futureproof.

## Specificity (classes vs. ids)

Elements that occur **exactly once** inside a page should use IDs, otherwise, use classes. When in doubt, use a class name.

*   **Good** candidates for ids: header, footer, modal popups.
*   **Bad** candidates for ids: navigation, item listings, item view pages (ex: issue view).

When styling a component, start with an element + class namespace (prefer class names over ids), prefer direct descendant selectors by default, and use as little specificity as possible. Here is a good example:

    <ul class="category-list">
      <li class="item">Category 1</li>
      <li class="item">Category 2</li>
      <li class="item">Category 3</li>
    </ul>


    ul.category-list { // element + class namespace

      &>li { // direct descendant selector > for list items
        list-style-type: disc;
      }

      a { // minimal specificity for all links
        color: #ff0000;
      }
    }


### CSS Specificity guidelines

*   If you must use an id selector (`#selector`) make sure that you have no more than *one* in your rule declaration. A rule like `#header .search #quicksearch { ... }` is considered harmful.
*   When modifying an existing element for a specific use, try to use specific class names. Instead of `.listings-layout.bigger` use rules like `.listings-layout.listings-bigger`. Think about `ack/grep`ing your code in the future.
*   The class names `disabled`, `mousedown`, `danger`, `hover`, `selected`, and `active` should *always* be namespaced by a class (`button.selected` is a good example).

## CSS selector intent

Instead of using selectors to drill down the DOM to an element, it is often best
to put a class on the element you explicitly want to style. Let’s take a
specific example with a selector like `.header ul{}`…

Let’s imagine that `ul` is indeed the main navigation for our website. It lives
in the header as you might expect and is currently the only `ul` in there;
`.header ul{}` will work, but it’s not ideal or advisable. It’s not very future
proof and certainly not explicit enough. As soon as we add another `ul` to that
header it will adopt the styling of our main nav and the the chances are it
won’t want to. This means we either have to refactor a lot of code _or_ undo a
lot of styling on subsequent `ul`s in that `.header` to remove the effects of
the far reaching selector.

Your selector’s intent must match that of your reason for styling something;
ask yourself **‘am I selecting this because it’s a `ul` inside of `.header` or
because it is my site’s main nav?’**. The answer to this will determine your
selector.

Make sure your key selector is never an element/type selector or
object/abstraction class. You never really want to see selectors like
`.sidebar ul{}` or `.footer .media{}` in our theme stylesheets.

Be explicit; target the element you want to affect, not its parent. Never assume
that markup won’t change. **Write selectors that target what you want, not what
happens to be there already.**

## !important

It is okay to use `!important` on helper classes only. To add `!important`
preemptively is fine, e.g. `.error{ color:red!important }`, as you know you will
**always** want this rule to take precedence.

Using `!important` reactively, e.g. to get yourself out of nasty specificity
situations, is not advised. Rework your CSS and try to combat these issues by
refactoring your selectors. Keeping your selectors short and avoiding IDs will
help out here massively.

## Magic numbers and absolutes

A magic number is a number which is used because ‘it just works’. These are bad
because they rarely work for any real reason and are not usually very
futureproof or flexible/forgiving. They tend to fix symptoms and not problems.

For example, using `.dropdown-nav li:hover ul{ top:37px; }` to move a dropdown
to the bottom of the nav on hover is bad, as 37px is a magic number. 37px only
works here because in this particular scenario the `.dropdown-nav` happens to be
37px tall.

Instead you should use `.dropdown-nav li:hover ul{ top:100%; }` which means no
matter how tall the `.dropdown-nav` gets, the dropdown will always sit 100% from
the top.

Every time you hard code a number think twice; if you can avoid it by using
keywords or ‘aliases’ (i.e. `top:100%` to mean ‘all the way from the top’)
or&mdash;even better&mdash;no measurements at all then you probably should.

Every hard-coded measurement you set is a commitment you might not necessarily
want to keep.

## Debugging

If you run into a CSS problem **take code away before you start adding more** in
a bid to fix it. The problem exists in CSS that is already written, more CSS
isn’t the right answer!

Delete chunks of markup and CSS until your problem goes away, then you can
determine which part of the code the problem lies in.

It can be tempting to put an `overflow:hidden;` on something to hide the effects
of a layout quirk, but overflow was probably never the problem; **fix the
problem, not its symptoms.**

 [1]: http://sass-lang.com