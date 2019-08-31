# Webpack  


## Initialization 
| Command | Description |
| ------- | ----------- |
| `npm install --save-dev webpack webpack-cli` |install webpack & CLI|
| `npm install --save-dev style-loader css-loader sass-loader node-sass` |install style ,css & sass loader|
| `npm install --save-dev webpack-dev-server` |install webpack-dev-server|
| `npm install --save-dev webpack-merge` |install webpack-merge to merge config-files|
| `npm install --save-dev html-loader` |loads different type of items from html|
| `npm install --save-dev file-loader` |file-loader loads different types of images|
| `npm install --save-dev clean-webpack-plugin` |cleans junks of the dist folder|
| `npm install --save-dev mini-css-extract-plugin` |creates css file in dist folder in production mode and links in index.html|
| `npm install --save-dev mini-css-extract-plugin` |minify css code in dist folder in production mode|


-----------------------------------------
## Webpack zero0 configuration--
It search for index.js at src folder . And compile it at dist folder as main.js. Path - dist/main.js

-----------------------------------------

## Webpack config file
```
// webpack.common.js
const path = require('path');



module.exports = {
    entry: {
        main: "./src/index.js",
        vendor: "./src/vendor.js"
    },

    module: {
        rules: [
            {
                test: /\.html$/,
                use: "html-loader"
            },
            {
                test:/\.(svg|png|jpg|gif)$/,
                use: {
                    loader: "file-loader",
                    options: {
                        name: "[name].[hash].[ext]",
                        outputPath: "imgs"
                    }
                }
            }
        ]
    }

}
```
```
// webpack.dev.js
const path = require('path');
const common = require('./webpack.common');
const merge = require('webpack-merge');
const htmlWebpackPlugin = require('html-webpack-plugin');

module.exports = merge(common, {
    mode: "development",
    output: {
        filename: "[name].bundle.js",
        path: path.resolve(__dirname, "dist")
    },

    plugins: [
        new htmlWebpackPlugin({
            template: "./src/index.html",
        })
    ],

    module: {
        rules: [
            {
                test: /\.scss$/,
                use: [
                    "style-loader",
                    "css-loader",
                    "sass-loader",
                ]
            },
        ]
    }
});
```
```
// webpack.prod.js
const path = require('path');
const common = require('./webpack.common');
const merge = require('webpack-merge');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserPlugin = require('terser-webpack-plugin');
const htmlWebpackPlugin = require('html-webpack-plugin');

module.exports = merge(common, {
    mode: "production",
    output: {
        filename: "[name].[contentHash].bundle.js",
        path: path.resolve(__dirname, "dist")
    },

    optimization: {
        minimizer: [
            new OptimizeCssAssetsPlugin(),
            new TerserPlugin()
        ]
    },

    plugins: [
        new MiniCssExtractPlugin({
            filename: "[name].[contentHash].css"
        }),
        new CleanWebpackPlugin(),

       
        new htmlWebpackPlugin({
            template: "./src/index.html",
            minify: {
                removeAttributeQuotes: true,
                collapseWhitespace: true,
                removeComments: true
            }
        })
    
    ],

    module: {
        rules: [
            {
                test: /\.scss$/,
                use: [
                    MiniCssExtractPlugin.loader,
                    "css-loader",
                    "sass-loader",
                ]
            },
        ]
    }
});
```

-----------------------------------------

## Webpack Loaders
Loaders are used to bundle any static resource other than JavaScript.

Eg. -- Style loader , Css loader , SaaS loader, json loader , svg loader etc ...

_________________________________________
### Css-Loaders & Style-loaders & Sass-Loaders
Sass , css and style loaders run together . Sass-loader compiles sass into css. Css-loader converts css into JS . And style loader injects styles into DOM .
```
// index.js file

import "./style.scss //to add sass into javaScript
```
-------------------------------------------

### Html-loader & File-loader
html-loader loads different type of files from html doc.
File-loader is dependency of html-loader . loads different type of files such as images etc . 

--------------

## Content Hashing
```
fileName[contentHash].js
```
Add junks to names .

--------

## Plugins
plugins gives extra functionality. Eg. -- HtmlWebpackPlugin
```
"scripts": {
    "start": "webpack-dev-server --config webpack.dev.js --open"
  }
```

----------------------------
### mini-css-extract-plugin
Minifies Css file in production mode is obtained by using this plugin

--------------------------
### Terser-webpack-plugin
Minifies JS file in production mode is obtained by using this plugin






