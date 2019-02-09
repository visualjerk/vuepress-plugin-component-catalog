# vuepress-plugin-component-catalog

[![npm version](https://badge.fury.io/js/vuepress-plugin-component-catalog.svg)](https://badge.fury.io/js/vuepress-plugin-component-catalog)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

This plugin is for generating a component catalog of Vue.js.
The catalog page is generated by adding it to the VuePress plugin.

## Status: alpha

This plugin is for [VuePress Next(v1.x.x)](https://github.com/vuejs/vuepress).


## Setup

Available in **2** steps.

### Install

```bash
$ yarn add -D vuepress@next vuepress-plugin-component-catalog
```

### Add plugin

`.vuepress/config.js`

```JavaScript
module.exports = {
  // ...
  themeConfig: {
    nav: [
      { text: 'Home', link: '/' },
      { text: 'Components', link: '/components/' },
    ]
  },
  plugins: [
    ['vuepress-plugin-component-catalog'],
  ],
};
```

Scan the project and complete the setup automatically.

> Depending on the project it may not work.


## Docs block

This plugin uses custom blocks of SFC.

### example

``````HTML
<docs>
# Base Button

Can be written with Markdown.

VuePress markdown extensions are also available.

[[toc]]

When you write a component, it will be mounted and displayed.

<base-button>Sample Button</base-button>

Code blocks tagged with `@playground` will be rendered as code examples.

```html
@playground
<base-button variation="primary">Primary Button</base-button>
```

</docs>

<template>
  <button type="button"><slot /></button>
</template>

<script>
export default {
  // ...
}
</script>
``````

## Options

```JavaScript
module.exports = {
  plugins: [
    [
      'vuepress-plugin-component-catalog',
      {
        // All options
        rootDir: '<your project root dir>',
        include: ['**/components/**']  // Specify the target to create a catalog
        exclude: ['**/views/**', '**/App.vue']  // Specify a target that does not create a catalog

        distDirPrefix: 'components',

        alias: { // import path alias
          '@': \<your project alias\>,
        },
        vueCli: {  // vue cli option
          configPath: path.join(PROJECT_DIR, 'vue.config.js'),
        },
        nuxt: {  // nuxt option
          configPath: path.join(PROJECT_DIR, 'nuxt.config.js'),
        },
      },
    ],
  ],
};
```

### `rootDir`(option)

- type: `string`
- default: `process.cwd()`

### `staticDir`(option)

Directory path of static resources.

- type: `string`
- deafult: `null`

### `include`(option)

Specify the target to create a catalog.
Can use glob.

- type: `string` or `Array<string>`

e.g.

```JavaScript
{
  include: ['**/components/**'],
}
```

### `exclude`(option)

Specify a target that does not create a catalog.
Can use globs.

- type: `string` or `Array<string>`

### `distDirPrefix`(option)

- type: `string`
- default: `components`

It becomes the URL path.

`http://localhost:8080/components/your-component`

### `vueCli.configPath`(option)

- type: `string`
- default: `process.cwd() + vue.config.js`

Please try to set it when the alias such as `@` can not be resolved automatically.

### `nuxt.configPath`(option)

- type: `string`
- default: `process.cwd() + nuxt.config.js`

Please try to set it when the alias such as `@`(`~`) can not be resolved automatically.

## FAQ

### What if an error occurs in the application?

It is not possible to process `docs` custom blocks, and errors may occur in the build of your application.

Please load and use the prepared webpack loader.
Here is a sample.

#### Vue CLI v3

vue.config.js

```JavaScript
module.exports = {
  // ...
  chainWebpack: config => {
    config.module
      .rule('docs')
      .oneOf('docs')
      .resourceQuery(/blockType=docs/)
      .use('through-loader')
      .loader('vuepress-plugin-component-catalog/dist/through-loader')
      .end();
  },
};
```

#### Nuxt.js

nuxt.config.js

```JavaScript
module.exports = {
  // ...
  build: {
    extend(config) {
      config.module.rules.push({
        resourceQuery: /blockType=docs/,
        loader: 'vuepress-plugin-component-catalog/dist/through-loader',
      });
    },
  },
};
```

### How to use with Vuex?

Please use `.vuepress/enhancedApp.js`.
In the future, if you are using Nuxt.js, it will be loaded automatically.

### How do you use sass resource loader?

Please use [VuePress Build Pipeline](https://vuepress.vuejs.org/config/#build-pipeline).

Refer to the project in the [example](https://github.com/mya-ake/vuepress-plugin-component-catalog/tree/master/example) directory.

### How to use with UI framework?

Sorry, I haven't tried it yet.
I'm planning to make samples.

Probably I will use `.vuepress/enhancedApp.js`.

## Example

See [example](https://github.com/mya-ake/vuepress-plugin-component-catalog/tree/master/example) directory.

- [Vue Cli v3](https://github.com/mya-ake/vuepress-plugin-component-catalog/tree/master/example/vue-cli)
- [Nuxt.js](https://github.com/mya-ake/vuepress-plugin-component-catalog/tree/master/example/nuxt)

## In the future

There is such an [idea](https://github.com/mya-ake/vuepress-plugin-component-catalog/issues?q=is%3Aissue+is%3Aopen+label%3Aidea).

