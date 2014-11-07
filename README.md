# Box

Box is a set of mixins for setting an element's box properties.

Aswell as enforcing a consistant approach to setting box-properties, it offers a hook for your own handling of unknown values, allowing you to define your own custom values. By avoiding explicit declaration of values, this allows your custom units to flex across breakpoints.

## Docs

You can view the docs online [here](http://undistraction.github.io/box/docs/) or locally in Chrome by running:

```
$ grunt docs
```

There is also a Grunt task to build the docs:

```
$ grunt sassdoc
```

## Tests

Tests are available from the excellent [Bootcamp](https://github.com/thejameskyle/bootcamp) and can
be run using:

```
$ grunt test
```

## API

### Basic API

There are mixins to cover all the box properties, both those that accept multiple arguments and those that accept single. Each mixin shadows the css property it represents, accepting the same values including `auto`, 'initial`, `inherit` and `calc`, and with `margin`, `border` and `padding` accepting multiple values.

For `margin` you have the following mixins:

```
@include margin(10px 20px);
@include margin-top(10px);
@include margin-right(10px);
@include margin-bottom(10px);
@include margin-left(10px);
```

For `border` you have the following mixins:

```
@include border(10px 20px);
@include border-top(10px);
@include border-right(10px);
@include border-bottom(10px);
@include border-left(10px);
```

For `padding` you have the following mixins:

```
@include padding(10px 20px);
@include padding-top(10px);
@include padding-right(10px);
@include padding-bottom(10px);
@include padding-left(10px);
```

There are also mixins setting pairs of values by orientation; either `top` and `bottom` values or `left` and `right` values for each box property. These mixins accept either one or two values.

```
@include h-margins(10px);
@include v-margins(10px);
@include h-borders(10px);
@include v-borders(10px);
@include h-padding(10px);
@include v-padding(10px);
```

There is also a single box mixin that allows you to set all box properties at the same time by passing a map:

```
@include box(
  (
    margin: 5px,
    padding: 20px,
    padding-left: 2px,
    border: 2px 3px 2px
  )
);
```

### Using custom values

Where things get interesting is a function called `box-parse-value-filter`. This function is called when a value isn't recognised (it is an unknown, unitless value). By default it will throw an error, but by overriding this function (by declaring a function with the same name and signiture after you've imported box), you can process this value yourself.

For example, most projects are full hardcoded box-property declarations which quickly become inconsistant and ad-hoc. Why not enforce consistancy and improve readability on your projectby using a set of custom units:

```

// Define a map of units
$custom-units-map: (
  hairline: 1px,
  single: 10px,
  double: 20px,
  triple: 30px,
  quadruple: 40px,
  half: 5px,
  third: 3.33333333px,
  quarter: 2,5px
);

// Override
@function box-parse-value-filter($key, $orientation){
  @if map-has-key($custom-units-map, $key) {
    @return map-get($custom-units-map, $key);
  } @else {
    // If it isn't recognised throw an error
    // Unfortunately Sass doesn't allow us to call super.
    @return box-throw-error($box-invalid-value-error, "Invalid value #{$key}");
  }
}

// Then you can use

.Example {
  @include box(
    (
      padding: single,
      margin-bottom: double,
      border: hairline
    )
  );
}

```

There is a lot more that you can do with this simple functionality. For example you could use unitless values in the `$custom-units-map` and multiply them with a vertical rhythm unit, or
use breakpoint context to tweak these units across breakpoints, so that a declaration of `single` can mean different values at different breakpoints. *More examples coming soon.*

## Dependencies & Compatability

It has no dependencies on other Sass libs and should work with Sass 3.3 and up, though it's currently only tested in 3.4.