# Vue Project Note

### Content

- Installation Environment
  - Installing Node.js and npm
  - Installing Yarn
  - Installing Vue-cli
  - Other Tools
-  Create a Vue Project
  - Vue CLI v3
  - Vue CLI v2
- Run a Vue Project
  - Yarn
  - npm

### Main



## 1. Installation Environment



### 1.1 Installing Node.js and npm

- Download latest version node.js. <https://nodejs.org/en/download/> 

- Installation

- View node.js version

```
$ node -v
```

- View npm version

```shell
$ npm -v
```



### 1.2 Installing Yarn

- Download latest version yarn. <https://yarnpkg.com/lang/en/docs/install/#windows-stable>
- View Yarn version

```shell
$ yarn --version
```

[Yarn doc](https://yarnpkg.com/en/)



### 1.3 Installing Vue-cli

- Install Vue-CLI

```shell
$ npm install -g @vue/cli
# OR
$ yarn global add @vue/cli
```

- View Vue-CLI version

```shell
$ vue --version
```

- Remove Old version vue-cli

```shell
$ npm uninstall vue-cli -g
$ yarn global remove vue-cli
```

[Vue CLI doc](https://cli.vuejs.org/guide/)



### 1.4 Other Tools

Webpack 

[Webpack doc](https://webpack.js.org/guides/)

Babel

[Babel doc](https://babeljs.io/)



## 2. Create a Vue Project

List all vue-cli command

```shell
$ vue list
```

### Vue CLI v3

`vue create`, `vue ui`

Create a vue project

```shell
$ vue create {my-project}
# OR
$ vue create .
# OR
$ vue ui
```

### Vue CLI v2

`vue init`

Create a vue project from a tempalte

```shell
$ vue init webpack my-project
# OR
$ vue init webpack .
```

If you have Vue CLI v3 installed, you can still use `vue init`



## 3. Install Plugins

Install a plugin in an already created project. `vue add`, `vue ui`

```shell
vue add eslint
vue add cli-plugin-eslint
vue add apollo
vue add @foo/bar
vue add eslint --config airbnb --lintOn save
vue add router
vue add vuex
```

```
vue ui
```

Common Plugins

- router. `@vue/cli-plugin-router`
- vuex. `@vue/cli-plugin-vuex`
- axios. `vue-cli-plugin-axios`
- jest. `@vue/cli-plugin-jest`, `@vue/cli-plugin-unit-jest`
- Other
  - eslint. `@vue/cli-plugin-eslint`
  - babel. `@vue/cli-plugin-babel`
  - apollo. GraphQL. `vue-cli-pugin-apollo`
  - e2e
  - i18n



## 4. Run a Vue Project

### Yarn

Run vue project

```
yarn run serve
yarn run dev
```

Project Setup

```
yarn install
```

Build project for product

```
yarn run build
```

Run test

```
yarn run test
```



### Npm

Run vue project







### References

[Real World Vue.js](https://www.vuemastery.com/courses/real-world-vue-js/vue-cli/)