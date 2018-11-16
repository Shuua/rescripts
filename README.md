<h1 align='center'>
  <img src='readme-assets/banner.png' alt='banner' />
</h1>

<!-- LICENSE -->

<p align='center'>

<a href='https://github.com/harrysolovay/rescripts/blob/master/LICENSE'>
  <img src='https://img.shields.io/npm/l/rescripts.svg?style=flat-square' />
</a>

<!-- NPM -->
<a href='https://www.npmjs.com/package/rescripts'>
  <img src='https://img.shields.io/npm/v/rescripts.svg?style=flat-square' />
</a>

<!-- PRs -->
<a href='http://makeapullrequest.com'>
  <img src='https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square' />
</a>

<!-- Lerna -->
<a href='https://lernajs.io/'>
  <img src='https://img.shields.io/badge/maintained%20with-lerna-cc00ff.svg' />
</a>

</p>

<hr />

> Advance your [CRA (create-react-app)](https://github.com/facebook/create-react-app) project configurations. No ejecting, no custom react-scripts fork, no more limitations.

## Highlights

- 🎯 create the perfect configuration with minimal effort

- 🎩 take advantage of cutting-edge software that hasn't made its way into CRA

- 🥳 pick and choose from a vast collection of "rescripts" (presets/plugins)

## Background

[CRA (create-react-app)](https://github.com/facebook/create-react-app) provides a first-class React developer experience. For building single-page web apps, it's not only the fastest boostrap––it's also the most carefully-curated, well-supported, and feature-fledged. There is a downside, however: in an effort to create stability and simplicity for beginners, it excludes many configuration options and newer technologies (such as Babel transformations based on early-stage [TC39](https://github.com/tc39) proposals). CRA comes with an "eject" script which––once irreversibly run––allows customization of the "start", "build", and "test" scripts, along with their corresponding configurations. While this does allow you some DX freedom, it isn't always preferable; ejection makes it impossible to upgrade to new versions of `react-scripts`, and it exposes a lot of tedious, knarly-lookin' code. Rescripts is for developers who don't want to eject or worry about configuration, but still want to use cutting-edge developer tooling.

Tim Arney's [react-app-rewired](https://github.com/timarney/react-app-rewired) was––in my humble opinion––the first piece of open source to successfully and reliably solve this problem with CRA. On top of offering a solution, it led to many "rewires" (community-made plugins for simpler setup). But––when CRA 2.0 came around––there were some breaking changes. Not to mention, the react-app-rewired DX was something to be simplified. And now... without further adieu.. enter Rescripts.

Rescripts tackles this same probem for CRA 2.0+ in a way that is compatible with rewires built for react-app-rewired. It introduces a default "rescript" (similar to a rewire) which automatically scans your project for the existence of ESLint and Babel configurations. Rescripts are loaded in a modular style, with a DX similar to that of Babel preset loading. The default rescript also installs must-have optimizations, which result in higher lighthouse scores (aka. better performance and shorter load times). Rescripts is highly configurable, yet can be used in two (extremely simple) steps. If you end up liking this library, please tweet at [@gearon](https://twitter.com/dan_abramov) requesting a `rescript-everything-i-did-not-include` package 🤩

## Installation

Install `@rescripts/cli` as a dev dependency. Although it might––depending on your configuration––affect code transformation, it doesn't by default intermingle any code with yours. For the opposite perspective (installing as a regular dependency), check out [this discussion](https://github.com/facebook/create-react-app/pull/2657).

```sh
yarn add -D @rescripts/cli
```

## Basic Usage

#### 1) Replace `react-scripts` calls with `rescripts` calls

```diff
{
  "name": "built-with-rescripts",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "react": "^16.6.1",
    "react-dom": "^16.6.1",
    "react-scripts": "2.1.1"
+   "@rescripts/cli": "^0.1.0"
  }
  "scripts": {
-   "start": "react-scripts start",
+   "start": "rescripts start",
-   "build": "react-scripts build",
+   "build": "rescripts build",
-   "test": "react-scripts test",
+   "test": "rescripts test",
-   "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": "react-app"
  },
  "browserslist": [
    ">0.2%",
    "not dead",
    "not ie <= 11",
    "not op_mini all"
  ]
}
```

#### 2) Add custom Babel & ESLint configs

Add custom configurations for Babel and/or/nor ESLint at your project's root director. For file name, use whatever convention you prefer:

**Babel:** `.babelrc`, `.babelrc.js`, or `babel.config.js`

**ESLint:** `.eslint`, `.eslintrc.js`, or `eslint.config.js`

Or specify the configuration in your package.json:

```diff
{
  "name": "built-with-rescripts",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "react": "^16.6.1",
    "react-dom": "^16.6.1",
    "react-scripts": "2.1.1"
  },
  "devDependencies": {
    "@rescripts/cli": "^0.1.0"
  }
  "scripts": {
    "start": "rescripts start",
    "build": "rescripts build",
    "test": "rescripts test"
  },
+ "eslintConfig": {
+   "extends": "react-app",
+   "rules": {
+     "some-rule": [1, "always"]
+   }
+ },
+ "babel": {
+   "presets": [
+     "react-app"
+   ],
+   "plugins": [
+     "some-babel-plugin"
+   ]
+ }
  "browserslist": [
    ">0.2%",
    "not dead",
    "not ie <= 11",
    "not op_mini all"
  ]
}
```

Note: when specifying the destination of a config/preset from your package.json, you cannot specify an rc file (only accepts js or json formats).

## Advanced Configuration

### Processes

Rescripts exposes the three main configurable processes of CRA:

1. **Webpack** is responsible for both development and production builds. This is likely where the bulk of your rescripting will go.
2. **Development server**––you'll want to configure `devServer` if you need to make use of proxying or are working with APIs that request assets of your site in development (aka. allowing CORS).
3. **Jest** is for testing your application. Rescripts take care of prepping any custom babel configuration to play well with Jest. If you look at [the testing script](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/scripts/test.js) in `react-scripts`, you'll find comments about plans for a more stable replacement. Keyboard blood & tears may ensue. Please file issues!

### Configuration

#### The "Root Rescript"

By default, rescripts will look for a rescripts.js file in the root directory of your app. This file (a "rescript") should export an object with any combination of the following:

| property    | type & return                                            | description                                                                                                | purpose                                                                                                                                                  |
| ----------- | -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `rescripts` | Array\<string> => void                                   | Relative or node modules paths to other "rescripts" to be applied before the current (in sequential order) | Allows for a more modular composition of rescripts                                                                                                       |  |
| `webpack`   | WebpackConfig => Modified                                | This function takes in the webpack configuration, transforms it, and returns the new config                | Allows you to edit your Webpack development and production configurations as a single object                                                             |  |
| `devServer` | () => (proxy, allowedHost) => () => (proxy, allowedHost) | Returns a function that calls the supplied (via sole argument) function and returns the altered value      | decide how you want your development server to behave––alternatively, disable this and create your own (for instance with a little docker + NGINX magic) |  |
| `jest`      | Array\<FillInLater>                                      |                                                                                                            |                                                                                                                                                          |  |

<details>
<summary>view example `rescript.js`</summary>

```jsx
module.exports = {
  presets: [require('@rescripts/preset-default')],
  webpack: config => {
    // do something to the config
    return config
  },
  devServer: configFn => (proxy, allowedHost) => ({
    ...configFn(proxy, allowedHost),
    headers: {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, PUT, POST, DELETE, PATCH, OPTIONS',
      'Access-Control-Allow-Headers':
        'X-Requested-With, Content-Type, Authorization',
    },
  }),
}
```

</details>

#### Point to your rescript(s)

Point to a "rescript" (configuration file) from your `package.json`:

```diff
{
  "name": "built-with-rescripts",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "react": "^16.6.1",
    "react-dom": "^16.6.1",
    "react-scripts": "2.1.1"
  },
  "devDependencies": {
    "@rescripts/cli": "^0.1.0"
  }
  "scripts": {
    "start": "rescripts start",
    "build": "rescripts build",
    "test": "rescripts test"
  },
+ "rescripts": "path/to/rescript",
  "browserslist": [
    ">0.2%",
    "not dead",
    "not ie <= 11",
    "not op_mini all"
  ]
}
```

... or point to a rescript in node_modules:

```diff
+ "rescripts": "@rescripts/preset-lighthouse",
```

... or point to multiple rescripts:

```diff
+ "rescripts": [
+   "@rescripts/preset-default",
+   "@rescripts/preset-lighthouse"
+ ],
```

#### Webpack-only

If your reconfiguration targets Webpack only, your rescript can directly export the Webpack function:

```js
module.exports = () => {
  // do something to the config
  return config
}
```

#### Development vs. Production

To differentiate between development and production reconfiguration, go ahead and reference environment variables as necessary.

```js
module.exports => {
  "presets": [
    process.env.NODE_ENV === 'production' &&
      require('@rescripts/preset-image-compression')
  ]
}
```

## Rescripts

- [rescript-default](https://github.com),
- [rescript-lighthouse](https://github.com),
- [rescript-image-compression](https://github.com),

## Miscellaneous

Thank you for checking out (maybe even building software with) Rescripts. If you have any bug reports or feature ideas, please go ahead and file an issue. If you have any other questions, comments, etc., please reach out to harrysolovay@gmail.com.

## Acknowledgements

Big shout out to...

- [Nilan Marktanner](https://github.com/marktani), an inspirational human being
- [Daniel Shaffer](https://github.com/danielshaffer), learning Vue instead of React ;)
- [Weiliy](https://github.com/weiliy), the original owner of the NPM name

This library has been released under the [MIT license](../blob/master/LICENSE)

**SO DO WHATEVER THE \$%#@ YOU WANT WITH IT!!!**