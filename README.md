# jqPropertyGrid
A small and simple property grid in JS to view/edit POJOs, excellent if you have a "settings" object you want to give the user to edit (that's why I have created it).

### Dependencies
* jQuery - This is mandatory
* jQueryUI - If jQueryUI is loaded so properties that are defined as `number` would be displayed using the jQueryUI spinner widget
* spectrum - A very cool Color Picker that will be used (if loaded) on properties that are defined as `color` [see spectrum on GitHub](https://github.com/bgrins/spectrum) 

### Usage
The property grid needs a `div` to live in, then just initialize it by calling to the `jqPropertyGrid` method on it:

The html part:
```html
<script src='jqPropertyGrid.js'></script>
<link rel='stylesheet' href='jqPropertyGrid.css' />

<div id='propGrid'></div>
```

The Javascript part:
``` javascript
// This is our target object
var theObj = {
  font: 'Consolas',
  fontSize: 14,
  fontColor: '#a3ac03',
  jQuery: true,
  modernizr: false,
  framework: 'angular',
  iHaveNoMeta: 'Never mind...'
};

// This is the metadata object that describes the target object properties (optional)
var theMeta = {
    // Since string is the default no nees to specify type
    font: { group: 'Editor', name: 'Font', description: 'The font editor to use'},
    // The "options" would be passed to jQueryUI as its options
    fontSize: { group: 'Editor', name: 'Font size', type: 'number', options: { min: 0, max: 20, step: 2 }},
    // The "options" would be passed to Spectrum as its options
    fontColor: { group: 'Editor', name: 'Font color', type: 'color', options: { preferredFormat: 'hex' }},
    // since typeof jQuery is boolean no need to specify type, also since "jQuery" is also the display text no need to specify name
    jQuery: { group: 'Plugins', description: 'Whether or not to include jQuery on the page' },
    // We can specify type boolean if we want...
    modernizr: {group: 'Plugins', type: 'boolean', description: 'Whether or not to include modernizr on the page'},
    framework: {name: 'Framework', group: 'Plugins', type: 'options', options: ['None', {text:'AngularJS', value: 'angular'}, {text:'Backbone.js', value: 'backbone'}], description: 'Whether to include any additional framework'}
};

// Create the grid
$('#propGrid').jqPropertyGrid(theObj, theMeta);

// In order to get back the modified values:
var theNewObj = $('#propGrid').jqPropertyGrid('get');
```
The result would be:

![jqPropertyGrid](https://github.com/ValYouW/jqPropertyGrid/raw/master/example/example.png)

### The metadata object
As seen from the example above the metadata object **can** be used (it's optional) in order to describe the object properties.

Each property in the metadata object could have the following:
* browsable - Whether this property should be included in the grid, default is true (can be omitted).
* group - The group this property belongs to
* name - The display name of the property in the grid
* type - The type of the property, supported are:
    * boolean - A checkbox would be used
    * number - If the jQueryUI Spinner is loaded then it would be used, otherwise textbox
    * color - If the Spectrum Color Picker is loaded then it would be used, otherwise textbox
    * options - A dropdown list would be used in case the metadata contains the `options` array property
* options - An extra options object per type:
    * If the type is `number` then the options would be passed as the jQueryUI Spinner options
    * If the type is `color` then the options would be passed as the Spectrum options
    * If the type is `options` then options should be an array with the drop-down values, if an element in the array is  `string` it will be used both as the value and text of the `option` element. If an element in the array is `object` then it should contains a `text` and `value` properties which would be used on the `option` element
* description - A description of the property, will be used as tooltip on an hint element (a span with text "[?]")

### Live example
See this CodePen page: http://codepen.io/ValYouW/pen/zInBg