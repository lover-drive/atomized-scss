This is a simple library that can be used to generate atomic css rules, based on attributes.

```
npm i atomized-scss
```

I've made my own atomic css for a pretty silly reason: [acss.io](https://acss.io) doesn't work with pug. Well, it does, but it's ugly. So, I did it, using attributes, so it looks like this:

```pug
    div(
        inline-block
        bg="primary" bg:hover="light"
        text-color="light" text-color:hover="dark"
        corners="rounded"
    ) Click me!
```

&#x200B;

Basically, it's two SASS mixins: **atom** and **quark**.

&#x200B;

Atom creates property=value pairs. Like `bg="primary"`

|Properties|Type||
|:-|:-|:-|
|**$values**|list or map|A list of values that property can be set to.|
|**$property**|string (css property)|A name of css property like `background-color` or `padding`|
|$property-name|string (optional)|A human-friendly name for you atom. Like `bg` instead of `background-color`. If none is provided, then $property is used.|
|$directional|boolean (optional)|Whether additional atoms (`-left`, `-right`, `-bottom`, `-top`, `-horizontal` and `-vertical`) should be created.|
|$selector-modifier|string (optional)|Basically it appends to the atom selector. Example: `":hover"`|


Example:

```SASS
@use 'atomized-scss' as atomizer

$colors: (dark: #212121, light: #fff, primary: #F84C26) // You can use maps
$sizes: 4px, 8px // Or lists
$font-weights: bold, normal, (light: 300) // Or any combination of them

+atomizer.atom($colors, color, text-color) // Good ol' simple atom
+atomizer.with-states($colors, color, text-color) // Creates additional atoms for :hover, :active and :disabled states

+atomizer.directional($sizes, margin)

+atom($font-weights, font-weight)
```

This is compiled to:

    *[text-color=dark] {
      color: #212121;
    }
    
    /* The same for other colors */
    
    *[text-color\:hover=dark]:hover {
      color: #212121;
    }
    
    /* The same for other colors */
    
    *[margin="4px"] {
      margin: 4px;
    }
    
    *[margin-left="4px"] {
      margin-left: 4px;
    }
    
    /* The same for other directions */
    
    *[font-weight=bold] {
      font-weight: bold;
    }
    
    *[font-weight=normal] {
      font-weight: normal;
    }
    
    *[font-weight=light] {
      font-weight: 300;
    }

&#x200B;

&#x200B;

Quark creates a simple attribute. Like `bold` or `flex`

|Properties|Type||
|:-|:-|:-|
|**$values**|list or map|A list of values that property can be set to.|
|**$property**|string (css property)|A name of css property like `display` or `font-weight`|
|$selector-modifier|string|Basically it appends to the atom selector. Example: `":hover"`|

Example:

    $fonts: sans-serif, serif, monospace
    
    +quark($fonts, font-family)

Is compiled to:

    *[sans-serif] {
      font-family: sans-serif;
    }
    
    *[serif] {
      font-family: serif;
    }
    
    *[monospace] {
      font-family: monospace;
    }