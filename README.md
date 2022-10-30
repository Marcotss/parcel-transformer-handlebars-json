# Handlebars plugin for Parcel 2 using JSON

This plugin allows [Parcel 2](https://parceljs.org/) to load and compile [Handlebars](https://handlebarsjs.com/) templates. It was heavily inspired by [partiellkorrekt/parcel-transformer-handlebars](https://github.com/partiellkorrekt/parcel-transformer-handlebars), which only works using frontmatter data, this will works only using JSON files

## Installation

Install with [npm](https://www.npmjs.com/):

```bash
npm install --save @marcotss/parcel-transformer-handlebars-json
```

Then activete the plugin for `.hbs` and `.handlebars` files by adding `transformers` to your `.parcelrc`:

```json
{
  "extends": "@parcel/config-default",
  "transformers": {
    "*.{hbs,handlebars}": ["@marcotss/parcel-transformer-handlebars-json"]
  }
}
```

(For reference see: https://parceljs.org/features/plugins/#transformers)

## Configuration

The plugin has the following config defaults. These are required for handlebars to map all dependencies for compiling handlebars templates.

```json
{
  "data": "src/markup/data",
  "decorators": "src/markup/decorators",
  "helpers": "src/markup/helpers",
  "layouts": "src/markup/layouts",
  "partials": "src/markup/partials"
}
```

### Custom Configuration

If you would like to enforce your own folder structure simply create  `handlebars.config.json` or `hbs.config.json` in your project root. Each property of the configuration file is optional and can also take an array of paths instead of just one path. If a property is not set, it will be taken from the defaulf configuration.

```json
{
  "data": "views/json",
  "decorators": "views/decorators",
  "helpers": "views/tools",
  "layouts": "views/templates",
  "partials": "views/partials"
}
```

If you want, you can also use `js` instead of `json`.


```js
module.exports = {
  data: 'views/json',
  decorators: 'views/decorators',
  helpers: 'views/tools',
  layouts: 'views/templates',
  partials: 'views/partials'
}
```
## Features

## Features

### Reads from JSON

The original plugin has built in support for frontmatter yaml. I edited it to pull content from JSON files:

#### Source - `content.json`

```json
{
    "title": "This is a heading",
    "desc": "this is a paragraph",
    "names": [
        "bob",
        "jane",
        "mark"
    ]
}

```

#### Source - `example.hbs`

```html

{{!< mainlayout}}

<h1>{{title}}</h1>
<p>{{desc}}</p>
<ul>
    {{#each names}}
    <li>{{this}}</li>
    {{/each}}
</ul>
```

#### Output - `example.html`

```html

<html>
<body>
<h1>This is a heading</h1>
<p>this is a paragraph</p>
<ul>
    <li>bob</li>
    <li>jane</li>
    <li>mark</li>
</ul>
</body>
</html>
```

### Handlebars Layouts

The plugin has built in support for [handlebars-layouts](https://www.npmjs.com/package/handlebars-layouts).

### Handlebars Helpers

The plugin is also including all helpers found in the npm package [handlebars-helpers](https://www.npmjs.com/package/handlebars-helpers).
Please refer to their documentation for example usages.


### Environment Variables

During compililation the plugin will also pass the following variable(s) to the template:

- NODE_ENV

This can be useful when you want specific code to show up on production builds.

```html
{{#eq NODE_ENV "production"}}
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXX');</script>
<!-- End Google Tag Manager -->
{{/eq}}
```

Or perhaps the opposite

```html
{{#isnt NODE_ENV "production"}}
<span class="dev-banner sticky full">
  You're in DEVELOPMENT mode
</span>
{{/isnt}}
```
