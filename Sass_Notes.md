# <img height="30" src="https://sass-lang.com/assets/img/styleguide/color-1c4aab2b.png">  Notes
#### 10/02/2023

## Syntax Formats of Sass:
1. SCSS Syntax - Sassy CSS
    - Similar to CSS. <br>
    - Superset of CSS. <br>
    - ext: .scss

2. Indented Syntax
    - Uses indentation instead of `{}` & `;`. <br>
    - Original syntax for Sass. <br>
    - ext: .sass

* We use SCSS Syntax.

## Variables in SCSS:
CSS does have variables whose values can be set. On the contrary, SCSS variables are converted actual values during the compile time.

Examples: <br>
> CSS <br>    
```css
:root {
    --primary-color: #272727
    --accent-color: #ff6527;
    --text-color: #fff;
}

body {
    background: var(--primary-color);
}
```
Above code sets the background to `#272727`.

> SCSS
```scss
// '$' indicates variable
$primary-color: #272727
$accent-color: #ff6527;
$text-color: #fff;

body {
    background: $primary-color;
}
```
Does the same job but instaed sets the actual background color value of the body `#272727`.

## Maps in SCSS
Maps are essentially key-value pairs like js objects or dictionaries in python. <br>
To retrieve map values we use:<br>
`map-get(key, value)`.

> Example:
```scss
$font-weights: (
    "regular": 400,
    "medium": 500,
    "bold": 800
);

body {
    font-weight: map-get($font-weight, bold);
}
```

## Nesting in SCSS
Multiple elements can be nested in SCSS for applying styling to the particular nested elements. <br>

##### NOTE: Nest only if needed to avoid `Pyramid of Doom`.

>SCSS <br>
```scss
.main {
    color: $text-color;
    p {
        background: $nested-para-bg-color;
    }
}
```

## Imports & Partial SCSS:
One can write small snippets of SCSS in different files and prefix those filenames with `_` underscore then import them in other SCSS files like this.

Say we want to import "`_resets.scss`".

```scss
@import './resets';
```

## Functions in SCSS:
- Functions are similar to JavaScript functions.
- Used for computing and returning values.

> Example
```scss
@function weight($weight-name) {
    @return map-get($font-weights, $weight-name);
}
```

## Mixins in SCSS:
- Mixins are used for reducing code duplication in SCSS.
- Mixins contain the code which needs to be reused in multiple elements and can be referenced in them using the `@include mixinname;` syntax.
- Must be used for styling.

 > Example 1:
 ```scss
 @mixin flexContent {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
 }

 .class-a {
    @include flexContent;
    ...

 }

 .class-c {
    @include flexContent;
    ...
 }
 ```

 Mixins can also take in arguments like this:

```scss
 @mixin flexContent(direction) {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
 }

 .class-b {
    @include flexContent(row);
    ...

 }
```

> Example 2: Theme Color
```scss
@mixin theme($light-theme: true) {
    @if $light-theme {
        background: lighten($primary-color, 100%);
        color: lighten($primary-color, 100%)
    }
}

.light {
    @include theme($light-theme: true);
    // no need to mention the parameter name but can be done to increase readability.
}
```

- Mixins can be directly included into css sections like this:
```scss
.class {
    @include mymixin() {
        ...
    }
    ...
}
```

## Extending styles in SCSS:
Used to duplicate the styles of one element over another using the `@extend` keyword.

> Example:
```scss
.class1 {
    ...
}

.class2 {
    @extend class1;
    ...
}

// class2 inherits all the properties of class1 apart from having its own properties as well.
```

## Shortcuts:
1. `&` - Refers to the parent. <br>
    >Usage:
    ```scss
    .main {
        #{&}__paragraph {
            color: red;
        }
    }
    ```
    - Here `#{}` (interpolation) tells the compiler that we need everything before `main` class and not just copy the `main`.
    > Result in css:
    ```css
    .main {
        ...
    }
    .main .main__paragraph {
        ...
    }
    ```

    - If we do not interpolate then the result would be: <br>
    ```css
    .main {
        ...
    }
    .main__paragraph {
        ...
    }
    ```
    - can also be used with selectors:
    ```scss
    .class-x {
        color: white;
        &:hover{
            color: red;
        }
    }
    ```