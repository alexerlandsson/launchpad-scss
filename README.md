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

To use colors in other files, use the function `color()` are exposed and imported using the `@use` rule. It is imported with everything else from __helpers__ but can also be imported alone using the following `@use` rule.

```scss
@use '../../../helpers/colors' as *;
```

When __colors__ is imported, set the color using the `color()` function. This function can fetch values from a deep nested _scss_ map.

```scss
color: color(text, main);
```

The example above refers to the variable `$colors.text.main` defined in the `$colors` map:

```scss
$colors: (
  text: (
    main: colorDefs(black),
  ),
);
```

## Compile

Compile the _scss_ in this project by running the following commands.

```bash
npm install
```

```bash
npm run sass
```

The script will output a CSS file at `compiled/styles.css`.
