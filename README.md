# RM CSS utilities
These CSS utilities are my ‘one true source’ of design properties across my projects that use my universal website template. All based on the work I did in 2015 on [fa-css-utilities](https://github.com/fac/fa-css-utilities). They comprise:

* Universal scales and aliases for: `font-size`, `font-family`, `line-height`, `color`, `background-color`, `margin`, `padding`, `border-radius`, and more
* Library of colours for text and block-level elements
* Mixins and `.u-` utility classes for 40+ CSS properties

This means properties are managed and versioned in a single place, and there's a universal language for using them across projects.

## Install

* [Install using NPM](https://www.npmjs.com/package/rm-css-utilities)

## Overview

**Global values and aliases**. Global utility values are defined in [`_utility-values`](https://github.com/robbiemanson/rm-css-utilities/blob/master/_utility-values.scss). Utilities `font-size`, `line-height`, `padding`, `margin`, etc have scales of values, as well as aliases. All aliases live in [`_utility-aliases.scss`](https://github.com/robbiemanson/rm-css-utilities/blob/master/_utility-aliases.scss). Aliases provide a finite scale (`x-small`, `default`, `xx-large`, etc) to make authoring easier and more meaningful.

**Local aliases**. You can assign local, project-specific aliases to global values by adding rules to [`_project-aliases`](https://github.com/robbiemanson/rm-css-utilities/blob/master/_project-aliases.scss). This way you aren’t continually declaring something like `@include line-height(x-loose)` for your project’s default `line-height`. You can just assign `x-loose` to `default` for your project.

**Turning utility classes on/off**. Utility _classes_ mirror the behaviour of our mixins but can be applied to HTML elements directly. Rendering all of them naturally adds a lot of weight a `.css` file, so they can be individually turned on/off in [`_utility-settings.scss`](https://github.com/robbiemanson/rm-css-utilities/blob/master/assets/scss/_utility-settings.scss). Utility classes have `!important` because utilities exist to do _one thing no matter what_.

**Optional `!important`**. `!important` can be added to any mixin declaration, e.g.: `@include padding(large, !important);`. This is especially useful for refactoring; `!important` allows us a way to turn existing UI patterns that rely on the cascade into completely self-contained components — _without_ breaking lots of stuff. Once a component has been created and rolled out, any `!important` arguments can just be removed.

## Examples

Most utilities can be applied two ways: using mixins in a `.scss` file, or directly to HTML elements as utility classes. Using mixins is preferred as you get all of the utility functionality without any extra weight being added to the output CSS file, and it encourages the idea of a component-based system.

### Using mixins in `.scss` files

```scss
.MyComponent {
  @include background-color(gray-13);
  @include font-family(default);
  @include font-size(large);
  @include line-height(tight);
  @include text-color(gray-5);
}

.MyOtherComponent {
  @include flexbox(flex);
  @include flex-direction(row-reverse);
  @include flex-justify-content(center);
  @include flex-grow(2);
  @include flex-shrink(0);
  @include margin-top(large);
}
```

### Using utility classes in HTML
Utility classes are useful for prototyping ideas, but use them with care and treat them as stop-gaps (remember they apply `!important`). Components should handle their own states and variants, so there should be relatively few utility classes being used in production.

**You might start with this&hellip;**

```html
<div class="u-margin--0 u-padding--0 u-text-align--center u-border-radius--default u-border--thin--solid--rm-blue u-line-height--tight u-flexbox u-flex-align-items--stretch u-flex-direction--row">
  <button class="u-background--none u-display--inline-block u-margin--0 u-border-right--thin--solid--blue u-padding--x-small--small u-text-color--x-light u-flex-grow--1 u-text-truncate">Payment</button>
  <button class="u-background--none u-display--inline-block u-margin--0 u-padding--x-small--small u-text-color--x-light u-flex-grow--1 u-text-truncate">Refund</button>
</div>
```

**&hellip;but you should end up with:**

```html
<div class="SegmentedControl">
  <button class="SegmentedControl-segment">Payment</button>
  <button class="SegmentedControl-segment">Refund</button>
</div>
```

```scss
.SegmentedControl {
  margin: 0;
  padding: 0;
  text-align: center;
  @include border-radius(default);
  @include border(thin, solid, gray-8);
  @include flexbox(flex);
  @include flex-align-items(stretch);
  @include flex-direction(row);
  @include line-height(tight);

.SegmentedControl-segment {
  background: none;
  display: inline-block;
  margin: 0;
  @include border-right(thin, solid, rm-blue);
  @include flex-grow(1);
  @include padding(x-small, small)
  @include text-color(x-light);
  @include text-truncate;
}
```

### Syntax
```
.u-<utility>[-descendent][--value]
```

Utilities are written in lower case as they tend to serve as direct representations of actual CSS properties (as opposed to our components which are written in upper camel case). Keeping the syntax closer to actual CSS not only makes utilties more predictable, it also helps to create a further distinction between components and utilities.

## Making changes

#### Branch off, make changes, create a PR

1. Make sure you're up to date with the master branch (`git checkout master; git fetch; git pull origin master`)
2. Branch off master (`git checkout -b <your-branch-name>`)
3. Make changes
4. Increment the version number in [package.json](https://github.com/robbiemanson/rm-css-utilities/blob/master/package.json). Generally: backwards-compatible fixes and smaller amendments to existing code are patches, new components and breaking changes count as minor releases, and full rewrites are major releases. See [semver.org](http://semver.org/) for detailed guidance.
5. Push up changes (`git push origin <your-branch-name>`) and write a [helpful pull request](https://github.com/blog/1943-how-to-write-the-perfect-pull-request) describing changes

#### Review PR, merge to master, create new release

5. Make any required changes then merge branch into master: `git checkout master; git merge --no-ff <branch-name>`
6. Push up the new version of master (`git push origin master`).
7. [Create a new release](https://help.github.com/articles/creating-releases/) with a useful description. Original PR is probably a good starting point. **If making breaking changes, provide guidance in the release notes about what changes will have to be made in other projects**
8. [Publish the npm package](https://docs.npmjs.com/getting-started/publishing-npm-packages) (`npm publish`) with a new version number.

### How to…

#### Add a new colour
To create a new colour variable, add it to the *Colors* section in [_utility-values.scss](https://github.com/robbiemanson/rm-css-utilities/blob/master/_utility-values.scss). New colour should be defined using the `rgb(0, 0, 0)` format, and should follow the standard naming syntax: `$color--rm-colour-name--variant`, e.g. `$color--rm-blue--dark`.

**NB:** New colours should only be added to the utilities after exact value has been decided, so be sure to test them locally beforehand.

## Acknowledgements
* [Nicolas Gallagher](https://github.com/necolas) particularly for his work on [SUIT CSS](https://suitcss.github.io/), which has informed much of the thinking behind this work
* [Brian Franco](https://github.com/mastastealth) for the excellent [SASS flex mixin](https://github.com/mastastealth/sass-flex-mixin) we used as the basis for our own partials