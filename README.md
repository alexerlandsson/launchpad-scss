# scss-boilerplate

This is my boilerplate I use in projects to maintain a well structured and scalable _scss_.

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

Coming soon...

## materials

Coming soon...

## partials

Coming soon...

## Compile

Compile the _scss_ in this project by running the following commands.

```bash
npm install
```

```bash
npm run sass
```

The script will output a CSS file at `compiled/styles.css`.
