# CSS Coding Standards

### Avoid fractional units
Always keep the formula for how you determined a value, rather than using the calculated result. When you use the calculated result it becomes extremely difficult for another developer (or your future self) to understand how that value was arrived at, or how to safely change that value.

```
.col {
  /* groan */
  width: 14.2857142857%;

  /* oh, I get it */
  width: calc(100% / 7);
}
```
[Longer explanation](https://css-tricks.com/keep-math-in-the-css/)
#### Abstraction

Furthermore, if you keep the formula instead of the values you will likely start to notice math patterns in your code that can be abstracted and reused. For example, there's a pattern revealed in this set of grid classes:
```
.column-1-7 {
   width: calc(100% / 7);
}

.column-2-7 {
   width: calc(100% / 7 * 2);
}

.column-3-7 {
   width: calc(100% / 7 * 3);
}
```
If you're using a CSS preprocessor, that pattern can be abstracted and encoded for reusability for any future grid dimension:
```
// This mixin can be used for any arbitrary grid class
@mixin grid-column(
  $cols: 7,
  $count: 1
) {
  .#{$class}-#{$ccount}-#{$cols} {
    width: calc(100% / $cols * $count);
  }
}
// This loop outputs the classes and values
@for $i from 1 through 3 {
  @include grid-column(7, $i);
}
```
