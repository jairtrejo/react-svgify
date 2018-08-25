react-transform-svgify
======================

[![Build Status](https://travis-ci.org/jairtrejo/react-transform-svgify.png?branch=v2.x)](https://travis-ci.org/jairtrejo/react-transform-svgify?branch=v2.x)
[![Dependency Status](https://david-dm.org/jairtrejo.mx/react-transform-svgify/2.1.0.png)](http://david-dm.org/jairtrejo/react-transform-svgify/2.1.0)
[![NPM version](https://badge.fury.io/js/react-transform-svgify.png)](http://badge.fury.io/js/react-transform-svgify)

Transform SVG files into React elements.

This is a fork of [`svg-reactify`](https://www.npmjs.com/package/svg-reactify)

Configuration
-------------

As with most browserify transforms, you can configure it via the second argument to `bundle.transform`:

```js
bundle.transform(require('react-transform-svgify'), { default: 'image' });
```

or inside your `package.json` configuration:

```json
{
    "browserify": {
        "transform": [
            ["react-transform-svgify", { "default": "image" }]
        ]
    }
}
```

Requiring SVG files
-------------------

Now you can do things like...

```javascript
var React = require('react'),
	SVG   = {
	    Dog   : require('images/dog.svg'),
	    Parrot: require('images/parrot.svg'),
	    Horse : require('images/horse.svg')
	};

module.exports = React.createClass({
    render: function () {
        return (
            <h2>Animals</h2>
			<ul>
				<li>
					<SVG.Dog/>
				</li>
				<li>
					<SVG.Parrot/>
				</li>
				<li>
					<SVG.Horse/>
				</li>
			</ul>
        );
    }
});
```

Templates
---------

react-transform-svgify uses templates to produce the react components. Currently there are two templates available - `image` and `icon`. Maybe there will be more in the future, like one for symbols for example.

Choose the default template using the `default` option. You can also set a regex for choosing templates based on filename like:

```json
{
    "browserify": {
        "transform": [
            ["react-transform-svgify", { "default": "image", "icon": "\.icon" }]
        ]
    }
}
```

This will use the `image` template by default and `icon` if the filename matches the regex `\.icon`. The other way around would be for example `["react-transform-svgify", { "default": "icon", "image": "\.image" }]`.

### Icon Template

This template will produce a DOM tree looking like:

```html
<span class="icon icon-__FILENAME_IN_KEBABCASE__">
   <svg .... />
</span>
```

The `<span>` element will also inherit any props given to the element, for example the following react element:

```html
<SVG.Dog className="customClass" something="else"/>
```

... will override the default class and produce:

```html
<span class="customClass" something="else">
   <svg .... />
</span>
```

You could then style the svg element further through CSS, for example:

```css
.customClass > svg {
  width: 100px;
  height: 100px;
  fill: #00ff00;
}
```

### Image Template

This template will produce a DOM tree containing only the SVG:

```html
<svg .... />
```

It will NOT pass on any props given to the element, so you cannot set the className or such.
