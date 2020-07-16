# Note of Vue Router

```sh
# install vue-cli
$ npm install --global vue-cli
# create a new project using the "webpack" template
$ vue init webpack <router-app> 
or
$ vue init webpack .
```

```sh
 This will install Vue 2.x version of the template.

 For Vue 1.x use: vue init webpack#1.0 router-app

? Project name router-app <Enter>
? Project description A Vue.js project  <Enter>
? Author  <Enter>
? Vue build standalone  <Enter>
? Install vue-router? Yes
? Use ESLint to lint your code? No
? Setup unit tests with Karma + Mocha? No
? Setup e2e tests with Nightwatch? No
```

```sh
# install dependencies and go!
$ cd router-app
$ npm install
$ npm run dev
```



### Vue Router HelloWorld

src/App.vue

```javascript
<template>
  <div id="app">
	<nav>
		<router-link to="/foo">Go to Foo</router-link>
		<router-link to="/bar">Go to Bar</router-link>
	</nav>
	
    <router-view />
  </div>
</template>
```

src/main.js

```javascript
import Vue from 'vue'
import App from './App'
import router from './router'

Vue.config.productionTip = false

new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```



src/router/index.js

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import Foo from '@/components/Foo'
import Bar from '@/components/Bar'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'Foo',
      component: Foo
    },
	{
      path: '/foo',
      name: 'Foo',
      component: Foo
    },
	{
      path: '/bar',
      name: 'Bar',
      component: Bar
    }
  ]
})
```

src/components/Foo.vue

```javascript
<template>
	<div id="foo">
		Foo
	</div>
</template>

<script>
export default {
  name: 'Foo',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>
```



src/components/Bar.vue

```javascript
<template>
	<div id="bar">
		Bar
    </div>
</template>

<script>
export default {
  name: 'Bar',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>
```



### References

[Vue Router](https://router.vuejs.org/)

[Getting Started With Vue Router](https://scotch.io/tutorials/getting-started-with-vue-router)

