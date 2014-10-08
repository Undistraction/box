# Box

Box is a set of mixins for setting an element's box properties with support for custom units.

There are mixins to cover all the box properties, both those that accept multiple arguments and those that accept single. For example for `padding` you have the following mixins:

```
@include padding(10px 20px);
@include padding-top(10px);
@include padding-right(10px);
@include padding-bottom(10px);
@include padding-left(10px);
```

You also have a single `box` mixin that allows you to set all box properties at the same time by passing a map:

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

This is all nice, but where things get interesting is that box offers a function called `box-parse-value-filter` that does nothing by default but can be overriden to hook you own unit handling into the mix. To enforce consistancy and improve readability on your project you can use a set of custom units and tie them into a unit of rhythm:

```

// Define a map of units
$rhythm-units-map: (
  single: 1,
  double: 2,
  triple: 3,
  quadruple: 4,
  half: 0.5,
  third: 0.33333333333,
  quarter: 0.25
);

// Define different rhythm units for vertical and horizontal to allow for optical distortion
$rhythm-map: (
  vertical: 12px,
  horizontal: 16px
);

// Define a function that overrides the hook
// If box encounters an unrecognised unitless value it will call this function to resolve it.
// In this example, the function looks that value up on our `$rythm-units-map` and multiplies
// it by the rhythm for the property's orientation.
@function box-parse-value-filter($key, $orientation){
  @if map-has-key($rhythm-units-map, $key) {
    $unit: map-get($rhythm-units-map, $key);
    @return map-get($rhythm-map, $orientation) * $unit;
  } @else {
    @error "Unsupported key #{$key}";
  }
}
```

Docs and Media Query support coming soon.