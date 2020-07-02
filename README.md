# Nuxt Style Resources - Nobody likes extra @import statements!

[![npm (scoped with tag)](https://img.shields.io/npm/v/@nuxtjs/style-resources/latest.svg?style=flat-square)](https://npmjs.com/package/@nuxtjs/style-resources)
[![npm](https://img.shields.io/npm/dt/@nuxtjs/style-resources.svg?style=flat-square)](https://npmjs.com/package/@nuxtjs/style-resources)
[![Build Status](https://travis-ci.com/nuxt-community/style-resources-module.svg?branch=master)](https://travis-ci.com/nuxt-community/style-resources-module)
[![codecov](https://codecov.io/gh/nuxt-community/style-resources-module/branch/master/graph/badge.svg)](https://codecov.io/gh/nuxt-community/style-resources-module)
[![Dependencies](https://david-dm.org/nuxt-community/style-resources-module/status.svg?style=flat-square)](https://david-dm.org/nuxt-community/style-resources-module)
[![js-standard-style](https://img.shields.io/badge/code_style-standard-brightgreen.svg?style=flat-square)](http://standardjs.com)

>

[📖 **Release Notes**](CHANGELOG.md)

## Features

* Share variables, mixins, functions across all style files (no `@import` needed)
* Support for SASS, LESS, Stylus, and PostCSS
* Aliases (`~/assets/variables.css`) and globbing as supported
* Compatible with Nuxt's `build.styleResources` (and will take them over directly if included!)
* Blazing fast:tm:

## Warning

**Do not import actual styles**.
Use this module only to import variables, mixins, functions (et cetera) as they won't exist in the actual build. Importing actual styles will include them in every component and will also make your build/HMR magnitudes slower. 
**Do not do this!**

## Setup

- If not already present, add the dependencies you need for SASS/LESS/Stylus (depending on your needs). PostCSS and `postcss-loader` are included by default as part of Nuxt.js.
  - SASS: `yarn add sass-loader node-sass`
  - LESS: `yarn add less-loader less`
  - Stylus: `yarn add stylus-loader stylus`
- Add `@nuxtjs/style-resources` dependency using yarn or npm to your project (`yarn add -D @nuxtjs/style-resources`)
- Add `@nuxtjs/style-resources` to `buildModules` section of `nuxt.config.js`:

```js
export default {
  buildModules: [
    '@nuxtjs/style-resources',
  ],

  styleResources: {
   // your settings here
   sass: [],
   scss: [],
   less: [],
   stylus: [],
   postcss: []
  }
}
```

## Examples

### LESS Example

`nuxt.config.js`:
```js
export default {
  css: ['~assets/global.less'],
  buildModules: ['@nuxtjs/style-resources'],
  styleResources: {
    less: './assets/vars/*.less'
  }
}
```

`assets/global.less`
```less
h1 {
  color: @green;
}
```

`assets/vars/variables.less`

```less
@gray: #333;
```

`assets/vars/more_variables.less`

```less
@green: #00ff00;
```

`pages/index.vue`
```vue
<template>
  <div>
    <!-- This h1 will be green -->
    <h1>Test</h1>
    <test/>
  </div>
</template>

<script>
import Test from '~/components/Test'

export default {
  components: { Test }
}
</script>

```

`components/Test.vue`
```vue
<template>
  <div class="ymca">
    Test
  </div>
</template>

<style lang="less">
  .ymca {
    color: @gray; // will be resolved to #333
  }
</style>
```

---

### SCSS Example

`nuxt.config.js`:
```js
export default {
  buildModules: ['@nuxtjs/style-resources'],
  styleResources: {
    scss: [
      './assets/vars/*.scss',
      './assets/abstracts/_mixins.scss' // use underscore "_" & also file extension ".scss"
      ]
  }
}
```

> Instead of `'./assets/abstracts/_mixins.scss'` you can use also `'~assets/abstracts/_mixins.scss'`

`assets/vars/_colors.scss`
```scss
$gray: #333;
```

`assets/abstracts/_mixins.scss`

```scss
@mixin center() {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate3d(-50%, -50%, 0);
}
```

`components/Test.vue`
```vue
<template>
  <div class="ymca">
    Test
  </div>
</template>

<style lang="scss">
  .ymca {
    @include center; // will be resolved as position:absolute....
    color: $gray; // will be resolved to #333
  }
</style>
```

---
### PostCSS Example

`nuxt.config.js`:
```js
export default {
  buildModules: ['@nuxtjs/style-resources'],
  styleResources: {
    postcss: [
      './assets/_variables.pcss' // use ".pcss" or ".postcss" file extension
    ]
  }
}
```

`assets/_variables.pcss`
```scss
$gray: #333;
```


`components/Test.vue`
```vue
<template>
  <div class="post">
    Test
  </div>
</template>

<style lang="scss">
  .post {
    color: $gray; // will be resolved to #333
  }
</style>
```

---

## License

Inspired by [nuxt-sass-resources-loader](https://github.com/anteriovieira/nuxt-sass-resources-loader).

[MIT License](LICENSE)

Copyright (c) Alexander Lichter
