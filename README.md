# WEBPACK PLUGIN

```
 ____  _  _   ___    ____  ____  __  ____  ____                         
/ ___)/ )( \ / __)  / ___)(_  _)/  \(  _ \(  __)                        
\___ \\ \/ /( (_ \  \___ \  )( (  O ))   / ) _)                         
(____/ \__/  \___/  (____/ (__) \__/(__\_)(____)                        
 ____  ____  __  ____    ____  _  _    ____  ____  __  _  _  _  _  __   
(  __)(    \(  )(_  _)  (  _ \( \/ )  / ___)(_  _)(  )( \/ )/ )( \(  )  
 ) _)  ) D ( )(   )(     ) _ ( )  /   \___ \  )(   )( / \/ \) \/ (/ (_/\
(____)(____/(__) (__)   (____/(__/    (____/ (__) (__)\_)(_/\____/\____/                                             
```


## Installation

This is a fork from https://www.npmjs.com/package/webpack-svgstore-plugin. Make sure to fallback on its dock if you have any issue.

```bash
npm i webpack-svgstore-plugin-stimulWP --save-dev
```

## We are not maintain version for node.js 0.12 more
Only:
- "7.0+"
- "6.0"
- "4.0"

## Webpack configuration

[EXAMPLE here](https://github.com/mrsum/webpack-svgstore-plugin/tree/develop/platform)

## Usage
#### 1) require plugin
```javascript
//webpack.config.js
var SvgStore = require('webpack-svgstore-plugin');
module.exports = {
  plugins: [
    // create svgStore instance object
    new SvgStore({
      // svgo options
      svgoOptions: {
        plugins: [
          { removeTitle: true }
        ]
      },
      prefix: 'icon'
    })
  ]
}
```

#### 2) Put function mark at your chunk

```javascript
// plugin will find marks and build sprite

var __svg__           = { path: './assets/svg/**/*.svg', name: 'assets/svg/[hash].logos.svg' };
// will overwrite to var __svg__ = { filename: "assets/svg/1466687804854.logos.svg" };

// also you can use next variables for sprite compile
// var __sprite__     = { path: './assets/svg/minify/*.svg', name: 'assets/svg/[hash].icons.svg' };
// var __svgstore__   = { path: './assets/svg/minify/*.svg', name: 'assets/svg/[hash].stuff.svg' };
// var __svgsprite__  = { path: './assets/svg/minify/*.svg', name: 'assets/svg/[hash].1logos.svg' };

// require basic or custom sprite loader
require('webpack-svgstore-plugin/src/helpers/svgxhr')(__svg__);
```

#### 2.1) **Stimul Notes**

As we tend to keep our source files at the root of our project, this is the kind of config we need.

The name param is relative to the webpack main target directory (in our case, it is usually the dist folder inside our template folder).
As we keep our images/icons in the `/assets` folder (which is at the same level as `/dist`), we need to do `../assets/...`.

```javascript
const __svg__ = { path: './../icons/**/*.svg', name: '../assets/svg-sprite.svg' };
require('webpack-svgstore-plugin/src/helpers/svgxhr')(__svg__);
```

#### 2.2) **Important**
The `webpack-svgstore-plugin` is great, however, it doesn't really fit our needs when it comes to the icons path we get in the HTML when using the following:

```
<svg class="svg-icon">
    <use xlink:href="#icon-name"></use>
</svg>
```

With the default module, the outputted generated/compiled path to the image would be `http://127.0.0.1:8888/assets/svg-sprite.svg` when we would need it to be `http://127.0.0.1:8888/wp-content/themes/projectname/assets/svg-sprite.svg`.
 
In order to get this working with our constraints, here is what has been changed in the module:

```
// webpack-svgstore-plugin/src/helpers/svgxhr.js

// Find this line
baseUrl = window.location.protocol + '//' + window.location.hostname + (window.location.port ? ':' + window.location.port : '');
 
// Replace it with this
baseUrl = window.themeUrl + '/assets/';
```

We can call the **window.themeUrl** as we set it in the footer of our WP projects for convenience.




##### Dear friends...
As you know, last version has integrated ajax sprite loader.
Right now, you can override that.
Or create your own sprite ajax loader function.

#### 3) HTML code for happy using
HTML:
```html
  <svg class="svg-icon">
    <use xlink:href="#icon-name"></use>
  </svg>
```
React JSX:
```html
  <svg className='svg-icon'>
    <use xlinkHref='#icon-name' />
  </svg>
```
## Plugin settings

#### options
- `template` - add custom jade template layout (optional)
- `svgoOptions` - options for [svgo](https://github.com/svg/svgo) (optional, default: `{}`)


## License

NPM package available here: [webpack-svgstore-plugin](https://www.npmjs.com/package/webpack-svgstore-plugin)

MIT © [Chernobrov Mike](http://mrsum.ru), [Gordey Levchenko](https://github.com/lgordey)
