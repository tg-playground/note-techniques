# Vue-CLI

### Content

- Overview
  - CLI
  - CLI Service
  - CLI Plugins
- Installation
- Basic
- Development

### I. Overview

#### CLI

The CLI (`@vue/cli`) is a globally installed npm package and provides the `vue` command in your terminal.

scaffold a new project via `vue create`

instantly prototype new ideas via `vue serve`.

a graphical user interface via `vue ui`.

#### CLI Service

The CLI Service (`@vue/cli-service`) is a development dependency. It's an npm package installed locally into every project created by `@vue/cli`.

The CLI Service is built on top of [webpack](http://webpack.js.org/) and [webpack-dev-server](https://github.com/webpack/webpack-dev-server). It contains:

- The core service that loads other CLI Plugins;
- An internal webpack config that is optimized for most apps;
- The `vue-cli-service` binary inside the project, which comes with the basic `serve`, `build` and `inspect` commands.

#### CLI Plugins

CLI Plugins are npm packages that provide optional features to your Vue CLI projects.

such as Babel/TypeScript transpilation, ESLint integration, unit testing, and end-to-end testing.

It's easy to spot a Vue CLI plugin as their names start with either `@vue/cli-plugin-` (for built-in plugins) or `vue-cli-plugin-` (for community plugins).

When you run the `vue-cli-service` binary inside your project, it automatically resolves and loads all CLI Plugins listed in your project's `package.json`.



### II. Installation Vue-CLI

1. **Remove** old vue-cli

   ```shell
   npm uninstall vue-cli -g
   yarn global remove vue-cli
   ```

2. Install npm

3. Install yarn

4. Install vue-cli

   ```shell
   npm install -g @vue/cli
   # OR
   yarn global add @vue/cli
   ```

4. Check version of vue-cli

   ```shell
   vue --version
   ```



### III. Basic



### Install Prototyping

You can rapidly prototype with just a single `*.vue` file with the `vue serve` and `vue build`commands.

#### require an additional global addon to be installed first:

```shell
npm install -g @vue/cli-service-global
# or
yarn global add @vue/cli-service-global
```

#### vue serve

Running vue application.

- Add an App.vue file

```
<template>
  <h1>Hello!</h1>
</template>
```

  - Run

```
vue serve
or 
vue serve MyComponent.vue
```

#### vue build

You can also build the target file into a production bundle for deployment

```
vue build
or
vue build MyComponent.vue
```



### Creating a Project

#### Vue Create

```shell
vue create hello-world
vue create .
```

#### Using the  GUI

You can also create and manage projects using a graphical interface with the `vue ui` command:

```shell
vue ui
```



### Plugins and Presets

#### Plugins

Vue CLI uses a plugin-based architecture. If you inspect a newly created project's `package.json`, you will find dependencies that start with `@vue/cli-plugin-`. Plugins can modify the internal webpack configuration and inject commands to `vue-cli-service`.

##### Installing Plugins in a Existing Project

```shell
vue add eslint
vue add cli-plugin-eslint
vue add apollo
vue add @foo/bar
vue add eslint --config airbnb --lintOn save
vue add router
vue add vuex
```

#### Rresets

Presets saved during `vue create` are stored in a configuration file in your user home directory (`~/.vuerc`). You can directly edit this file to tweak / add / delete the saved presets.



### VLI Service

```shell
npm run serve
# OR
yarn serve
```

```shell
npx vue-cli-service serve
```

```shell
vue-cli-service serve
```



### IV. Development







### References

[Vue CLI - doc](https://cli.vuejs.org/guide/#components-of-the-system)

