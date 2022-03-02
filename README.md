# 🧑‍🚀 Launchpad SCSS

_Launchpad SCSS_ is a boilerplate for _Sass (scss)_ used in projects to maintain a well structured and scalable _CSS_. Comes with basic functions, mixins and coding standards.

It is a well tested architecture that works in projects of any size, from small single-page applications to large enterprise e-commerce.

> __Looking for a component library?__ [🚀 Rocket SCSS](https://github.com/alexerlandsson/rocket-scss) is a minimal component libary built on _Launchpad SCSS_ used to make the the start of a new project skyrocket.

## Methodology

The design of this boilerplate is based on the _CSS_ mothodology **BEM**.

The following is an example of how a component could be structured.

```html
<div class="foo foo--alternative">
  <div class="foo__bar">
    <!-- HTML -->
  </div>
</div>
```

In the example above, `.foo` is the name of the component (_block_). `.foo__bar` is a child _element_. `.foo--alternative` is a _modifier_. To convert this into _scss_ it could look like this:

```scss
.foo {
  color: #000;

  &--altenative {
    color: #757575;
  }

  &__bar {
    margin-bottom: 8px;
  }
}
```

## Structure

The _scss_ is structured in four main folders with `styles.scss` as a main input file.

- helpers
- imports
- materials
- partials

### helpers

Helpers are features such as functions, mixins and theme related variables such as colors.

To use helpers in your components, import them using _scss_ `@use` rule.

```scss
@use "../../../helpers" as *;
```

You can also import each subcategory in helpers if you just want to use a specific part. It is also possible to add a custom namespace by raplacing `*` with desired namespace.

#### breakpoint

This folder contains all information about breakpoints used in the project. Breakpoints are defined in `helpers/breakpoint/_breakpoints.scss`.

To customize breakpoints, add the breakpoints needed in `$breakpoint-defs`. This _scss_ map will automatically create `min-width` and `max-width` breakpoints based on its values. As an example, if you add a breakpoint called `sm`, two breakpoints will be generated; `sm` and `lt-sm`.

#### color

This folder contains all information about colors used in the project. Colors are defined in `$color-defs` (`helpers/color/_definition.scss`) and assigned to a variable in `$colors` (`helpers/color/_colors.scss`).

To use colors in other files, use the function `color()` that are exposed and imported using the `@use` rule. It is imported with everything else from **helpers** but can also be imported alone using the following `@use` rule.

```scss
@use "../../../helpers/color" as *;
```

When **colors** is imported, set the color using the `color()` function. This function can fetch values from a deep nested _scss_ map.

```scss
.foo {
  color: color(text, main);
}
```

The example above refers to the variable `text.main` defined in the `$colors` map:

```scss
$colors: (
  text: (
    main: color-defs(black),
  ),
);
```

#### font

This folder contains all styling related to the font(s) used in the project. `helpers/font/_fonts.scss` includes mixins to define a specific font style.

If you are going to add own fonts using `@font-face`, this is the place where that code should be placed.

To use colors in other files, use the mixins exposed and imported using the `@use` rule. It is imported with everything else from **helpers** but can also be imported alone using the following `@use` rule.

```scss
@use "../../../helpers/font" as *;
```

When **font** is imported, set the font using any of the font mixins.

```scss
.foo {
  @include font-regular;
}
```

#### function

This folder contains global functions used across the project.

To use these functions in other files, use the functions exposed and imported using the `@use` rule. It is imported with everything else from **helpers** but can also be imported alone using the following `@use` rule.

```scss
@use "../../../helpers/function" as *;
```

##### size

Size is a useful function used to create spacing in the project. This function is found in `helpers/function/_size.scss` and should be used everywhere where a value is set in `px`.

The size is calculated using a multiplier and base unit. The base unit is set to `8px` by default but could be changed in this file.

```scss
.foo {
  padding: size(4); // 4 * 8px = 32px
}
```

##### str-replace

This is a basic function found in `helpers/function/_str-replace.scss` used for replacing substrings in a string. The functions takes `$string` (input string), `$search` (substring to be replaced) and `$replace` (string to replace `$search` with. Default `''`) as parameters.

```scss
$hex: #f5f5f5;
$hex-digits: str-replace(#{$hex}, '#', ''); // f5f5f5
```

#### mixin

This folder contains all global mixins used in the project. Mixins are primarily used to add prefix to properties, but there can also be other use cases.

To use these mixins in other files, use the mixins exposed and imported using the `@use` rule. It is imported with everything else from **helpers** but can also be imported alone using the following `@use` rule.

```scss
@use "../../../helpers/mixin" as *;
```

#### variable

This folder contains all global variables used in the project.

To use variables in other files, use the functions exposed and imported using the `@use` rule. It is imported with everything else from **helpers** but can also be imported alone using the following `@use` rule.

```scss
@use "../../../helpers/variable" as *;
```

#### variables

Global variables are defined in the map `$variables` found in `helpers/variable/_variables.scss`. To use the variables in other files, use the `scss-var()` function. This function can fetch values from a deep nested scss map.

```scss
.foo {
  font-size: scss-var(font, size, large);
}
```

The example above refers to the variable `font.size.large` defined in the `$variables` map:

```scss
$variables: (
  font: (
    size: (
      large: 1.125rem,
    ),
  ),
);
```

#### constants

Constants are located in `helpers/variable/_constants.scss` and includes variables that are bound to _CSS_ itself.

Constants are defined in the _scss_ map `$contants` and used using the exposed function `const()`. This function can fetch values from a deep nested scss map.

These constants could be useful when positioning a modular component for intance. The example below uses the constant `position, inline, x` to add modifiers for text alignment.

```scss
.foo {
  @each $value in const(position, inline, x) {
    &--align-#{$value} {
      text-align: $value;
    }
  }
}
```

The example above refers to the variable `$position-inline-x` used in the `$contants` map:

```scss
$position-inline-x: left, center, right; // This variable is not exposed using @forward

$contants: (
  position: (
    inline: (
      x: $position-inline-x,
    ),
  ),
);
```

### imports

Imports are located in `imports` and this is where all components are imported.

To minimize unnecessary _CSS_ for media queries, each media query for the breakpoints are only declared once. Breakpoint specific files are then imported in the correct import file.

Each component should include a _scss_ file for each breakpoint it uses. These breakpoint files are imported in `imports` using _scss_ `@use` rule.

As an example, `demo.scss` is the default _scss_ file for that component. This file contains styling across all breakpoints. This file is imported in `imports/imports.scss`. The breakpoint specific file in the same component, `demo-sm.scss`, contains styling used in the `sm` breakpoint (and larger) and is imported in `imports/imports-sm.scss`.

To summarize, breakpoint media quieries should **not** be included in any components, but be handled in `imports` instead.

### materials

Materials is the building blocks in the project. Each material should be based on the lowest common dominator. A material is modular and could be used by iteself, inside other materials or in combination with other materials.

Materials is split into two parts; _componets_ and _layout_. They are both containing components, but separated to make the structure easier. This is the place where the majority of the work is done.

> **Important:** It is important to name the components and thier breakpoint folders correctly. The folder and default _scss_ file should both be named as the comopnent itself. Breakpoint specific _scss_ files should be suffixed with `*-[BREAKPOINT]` (`[COMPONENT_NAME]-[BREAKPOINT].scss`). If the components is named `demo` the breakpoint file for the `sm` breakpoint should be named `demo-sm.scss`.

#### components

Components are found in `materials/components`. Each folder in this location component represents a component. Each component consists of one or more _scss_ files (one for each breakpoint).

> A demo component is included in this boilerplate called `demo` to demonstrate how a component is supposed to be built.

#### layout

Components that are used for layout purposes only should be placed in `materials/layout`. Other than that, the same rules as for _components_ applies here.

> The layout component `container` is included in this boilerplate as an example of what a layout material is. This is also a very useful component in most projects.

### partials

Partials contains styling aimed at elements and global styling, such as base styling, focus, keyframes and reset _CSS_.

## Work in this repository

Before you start working in this repository, install the dependencies.

```bash
npm install
```

### Compile scss to CSS

Compile the _scss_ in this project into _CSS_ by running the following commands.

```bash
npm run sass
```

The scripts will output a _CSS_ file at `compiled/styles.css`.

### Code formatting

This project uses [Prettier](https://prettier.io/) to format the code. To format all files, run the following command.

```bash
npm run prettier
```

### Lint

This project uses [Stylelint](https://stylelint.io) to lint the _scss_. The settings is based on `stylelint-config-standard-scss`. To run the lint, run the following command.

```bash
npm run stylelint
```
