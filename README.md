# scss-boilerplate

This is my boilerplate I use in projects to maintain a well structured and scalable _scss_.

## Methodology

The design of this boilerplate is based on the _CSS_ mothodology __BEM__.

The following is an example of how a component could be structured.

```html
<div class="foo foo--alternative">
  <div class="foo__bar">
    <!-- HTML -->
  <div>
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

* helpers
* imports
* materials
* partials

### helpers

Helpers are features such as functions, mixins and theme related variables such as colors.

To use helpers in your components, import them using _scss_ `@use` rule.

```scss
@use '../../../helpers' as *;
```

You can also import each subcategory in helpers if you just want to use a specific part. It is also possible to add a custom namespace by raplacing `*` with desired namespace.

#### colors

This folder contains all information about colors used in the project. Colors are defined in `helpers/colors/_colors.scss`.

Use the map `$colorDefs` to define the color codes and then use these colors in `$colors` in combination with the color name you will use to reference that color in other files.

To use colors in other files, use the function `color()` that are exposed and imported using the `@use` rule. It is imported with everything else from __helpers__ but can also be imported alone using the following `@use` rule.

```scss
@use '../../../helpers/colors' as *;
```

When __colors__ is imported, set the color using the `color()` function. This function can fetch values from a deep nested _scss_ map.

```scss
.foo {
  color: color(text, main);
}
```

The example above refers to the variable `text.main` defined in the `$colors` map:

```scss
$colors: (
  text: (
    main: colorDefs(black),
  ),
);
```

#### font

This folder contains all styling related to the font(s) used in the project. `helpers/font/_font.scss` includes mixins to define a specific font style.

If you are going to add own fonts using `@font-face`, this is the place where that code should be placed.

To use colors in other files, use the mixins exposed and imported using the `@use` rule. It is imported with everything else from __helpers__ but can also be imported alone using the following `@use` rule.

```scss
@use '../../../helpers/font' as *;
```

When __font__ is imported, set the font using any of the font mixins.

```scss
.foo {
  @include font-regular();
}
```

#### functions

This folder contains global functions used across the project.

To use these functions in other files, use the functions exposed and imported using the `@use` rule. It is imported with everything else from __helpers__ but can also be imported alone using the following `@use` rule.

```scss
@use '../../../helpers/functions' as *;
```

##### Size

Size is a useful function used to create spacing in the project. This function is found in `helpers/functions/_size.scss` and should be used everywhere where a value is set in `px`.

The size is calculated using a multiplier and base unit. The base unit is set to `8px` by default but could be changed in this file.

```scss
.foo {
  padding: size(4); // 4 * 8px = 32px
}
```

### media-query

This folder contains all information about media queries used in the project.

Breakpoints are defined in `helpers/media-query/_breakpoint.scss`. This is the place where you can add or remove breakpoints.

> __Important:__ The mixins in this folder are used in `styles.scss` and should __not__ be used anywhere else. Read more about why in the _imports_ section.

### mixins

This folder contains all global mixins used in the project. Mixins are primarily used to add prefix to properties, but there can also be other use cases.

To use these mixins in other files, use the mixins exposed and imported using the `@use` rule. It is imported with everything else from __helpers__ but can also be imported alone using the following `@use` rule.

```scss
@use '../../../helpers/mixins' as *;
```

### variables

This folder contains all global variables used in the project.

To use variables in other files, use the functions exposed and imported using the `@use` rule. It is imported with everything else from __helpers__ but can also be imported alone using the following `@use` rule.

```scss
@use '../../../helpers/variables' as *;
```

#### variables

Global variables are defined in the map `$variables` found in `helpers/variables/_variables.scss`. To use the variables in other files, use the `var()` function. This function can fetch values from a deep nested scss map.

```scss
.foo {
  font-size: var(font, size, large);
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

#### constans

Constants are located in `helpers/variables/_constants.scss` and includes variables that are bound to _CSS_ itself.

Constants are defined in the _scss_ map `$contants` and used using the exposed function `const()`. This function can fetch values from a deep nested scss map.

These constants could be useful when positioning a modular component for intance. The example below uses the constant `CSS_POSITIONS_X` to add modifiers for text alignment.

```scss
.foo {
  @each $value in const(CSS_POSITIONS_X) {
    &--align-#{$value} {
      text-align: $value;
    }
  }
}
```

The example above refers to the variable `CSS_POSITIONS_X` defined in the `$contants` map:

```scss
$CSS_POSITIONS_X: left, center, right; // This variable is not exposed using @forward

$contants: (
  CSS_POSITIONS_X: $CSS_POSITIONS_X,
);
```

## imports

Imports are located in `imports` and this is where all components are imported.

To minimize unnecessary _CSS_ for media queries, each media query for the breakpoints are only declared once. Breakpoint specific files are then imported in the correct import file.

Each component should include a _scss_ file for each breakpoint it uses. These breakpoint files are imported in `imports` using _scss_ `@use` rule.

As an example, `demo.scss` is the default _scss_ file for that component. This file contains styling across all breakpoints. This file is imported in `imports/imports.scss`. The breakpoint specific file in the same component, `demo-sm.scss`, contains styling used in the `sm` breakpoint (and larger) and is imported in `imports/imports-sm.scss`.

To summarize, breakpoint media quieries may __not__ be included in any components, but be handled in `imports` instead.

## materials

Materials is the building blocks in the project. Each material should be based on the lowest common dominator. A material is modular and could be used by iteself, inside other materials or in combination with other materials.

Materials is split into two parts; _componets_ and _layout_. They are both containing components, but separated to make the structure easier. This is the place where the majority of the work is done.

> __Important:__ It is important to name the components and thier breakpoint folders correctly. The folder and default _scss_ file should both be named as the comopnent itself. Breakpoint specific _scss_ files should be suffixed with `*-[BREAKPOINT]` (`[COMPONENT_NAME]-[BREAKPOINT].scss`). If the components is named `demo` the breakpoint file for the `sm` breakpoint should be named `demo-sm.scss`.

### components

Components are found in `materials/components`. Each folder in this location component represents a component. Each component consists of one or more _scss_ files (one for each breakpoint).

> A demo component is used in this boilerplate called `demo` to demonstrate how it is supposed to work.

### layout

Components that are used for layout purposes only should be placed in `materials/layout`. Other than that, the same rules as for _components_ applies here.

> The layout component `container` is included in this boilerplate as an example of what a layout material is. This is also a very useful component in most projects.

## partials

Partials contains styling aimed at elements and global styling, such as base styling, keyframes and reset _CSS_.

## Compile into CSS

Compile the _scss_ in this project into _CSS_ by running the following commands.

```bash
npm install
```

```bash
npm run sass
```

The script will output a _CSS_ file at `compiled/styles.css`.
