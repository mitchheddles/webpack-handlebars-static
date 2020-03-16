# Webpack Handlebars Static Site Builder

A very simple static site builder.

Built with:

* Handlebars (with JSON data)
* SASS
* Babel
* Webpack

## Commands

Setup using:
```bash
npm install
```

Start a development server with watch tasks:
```bash
npm start
```

Build for production:
```bash
npm build
```

## Project Structure

```
/
├── src
│   ├── css
│   │   └── index.scss
│   ├── data
│   │   ├── replacements.json
│   │   └── site.json
│   ├── fonts
│   ├── img
│   ├── js
│   │   └── index.js
│   ├── views
│   │   ├── layout
│   │   │   └── template.hbs
│   │   ├── partials
│   │   ├── page.hbs
│   │   └── index.hbs
│   └── entry.js
├── webpack.config.js
└── webpack.helpers.js
```

**Output:**
```
dist
├── bundle[hash].js
├── fonts
├── img
├── index.html
├── main[hash].css
└── page
    └── index.html
```

### Pages

Any `.hbs` file in `src/views`, outside of `layout` and `partials` will become it's own page. Each page will be created as a new directory with an index.html file. Pages can be nested. Asset paths will be resolved using the `webpack.output.publicPath` option (default `/`).

**Examples:**
```
Input: src/views/about-us.hbs
Output: dist/about-us/index.html

Input: src/views/projects/slug.hbs
Output: dist/projects/slug/index.html
```

### Data

Webpack will look for any direct `.json` files in the `data` directory (does not support folders). The data will be available for use in the Handlebars templates. The file name will be used as the key.

For usage help see the [Handlebars expressions docs](https://handlebarsjs.com/guide/expressions.html).

**Example:**

```json
// site.json
{
  "title": "My Website"
}
```

```html
// index.hbs
<h1>{{site.title}}</h1>
```

```html
// index.html
<h1>My Website</h1>
```


#### Replacements

Replacements are a way of organising common, or repeated, data. All replacements are applied to the data before it is passed to Handlebars via string replacement.

To add a 'replacment', add a new key-value pair to `src/data/replacements.json`.

**Example:**

```json
// replacements.json
{
  "[images]": "/img/"
}
```

```json
// site.json
{
  "title": "My Website",
  "logo": "[images]logo.png"
}
```

```html
// index.hbs
<img src="{{site.logo}}" alt="{{site.title}} logo" />
```

```html
// index.html
<img src="/img/logo.png" alt="My Website logo" />
```
