# Note of Vue.js

### Content

- Introduction
  - Declarative Rendering
  - Conditionals and Loops
  - Handling User Input
  - Composing with Components
- The Vue Instance
  - Creating a Vue Instance
  - Data and Methods
  - Instance Lifecycle Hooks
- Template Syntax
  - Interpolations
  - Directives
  - Shorthands
- Computed Properties and Watchers
  - Computed Properties
  - Watchers
- Class and Style Bindings
  - Binding HTML Classes
  - Binding Inline Styles
- Conditional Rendering
- List Rendering
- Event Handling
  - Listening to Events
  - Method Event Handlers
  - Methods in Inline Handlers
  - Event Modifiers
- Form Input Bindings
  - Basic Usage
  - Value Bindings
- Components Basics



### Main



### Introduction

#### Declarative Rendering

```javascript
<div id="app">
  {{ message }}
</div>

var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

#### Conditionals and Loops

Conditionals: `v-if`

```javascript
<div id="app-3">
  <span v-if="seen">Now you see me</span>
</div>

var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

Loops: `v-for`

```javascript
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>

var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```

#### Handling User Input

Event: `v-on:click`

```javascript
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>

var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

Two-way binding: `v-model`

```javascript
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>

var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```

#### Composing with Components

 Vue components are very similar to **Custom Elements**

1.

Definite a component

```javascript
<ol>
  <!-- Create an instance of the todo-item component -->
  <todo-item></todo-item>
</ol>

// Define a new component called todo-item
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})
```

2.

Component accept a prop by field `props:<props_name>`

pass prop by `v-bind:<props_name>`

```javascript
<div id="app-7">
  <ol>
    <!--
      Now we provide each todo-item with the todo object
      it's representing, so that its content can be dynamic.
      We also need to provide each component with a "key",
      which will be explained later.
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>

Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```

### The Vue Instance

#### Creating a Vue Instance

```javascript
var vm = new Vue({
  // options
})
```

- When you create a Vue instance, you pass in an **options object**. The majority of this guide describes how you can use these options to create your desired behavior. For reference, you can also browse the full list of options in the [API reference](https://vuejs.org/v2/api/#Options-Data).

- A Vue application consists of a **root Vue instance** created with `new Vue`, optionally organized into a tree of nested, reusable components. For example, a todo app’s component tree might look like this:

  ```
  Root Instance
  └─ TodoList
     ├─ TodoItem
     │  ├─ DeleteTodoButton
     │  └─ EditTodoButton
     └─ TodoListFooter
        ├─ ClearTodosButton
        └─ TodoListStatistics
  ```

- All Vue **components** are also Vue instances

#### Data and Methods

- `data` object to Vue’s **reactivity system**.

```javascript
// Our data object
var data = { a: 1 }

// The object is added to a Vue instance
var vm = new Vue({
  data: data
})

// Getting the property on the instance
// returns the one from the original data
vm.a == data.a // => true

// Setting the property on the instance
// also affects the original data
vm.a = 2
data.a // => 2

// ... and vice-versa
data.a = 3
vm.a // => 3
```

- `Object.freeze()`

```javascript
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
```

- Call instance properties and methods using `$`

```javascript
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch is an instance method
vm.$watch('a', function (newValue, oldValue) {
  // This callback will be called when `vm.a` changes
})
```

#### Instance Lifecycle Hooks

```javascript
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` points to the vm instance
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

- `created`, `mounted`, `updated`, `destroyed`

### Template Syntax

#### Interpolations

Text: `{{ your_text }}`

```javascript
<span>Message: {{ msg }}</span>
<span v-once>This will never change: {{ msg }}</span>
```

Raw HTML: `v-html`

```javascript
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

Attributes: `v-bind:id`

```javascript
<div v-bind:id="dynamicId"></div>
```

Boolean Attributes: `v-bind:disabled`

```javascript
<button v-bind:disabled="isButtonDisabled">Button</button>
```

Using JavaScript Expressions

```javascript
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```



#### Directives

```javascript
<p v-if="seen">Now you see me</p>
```

Arguments

```javascript
<a v-bind:href="url"> ... </a>
<a v-on:click="doSomething"> ... </a>
```

Dynamic Arguments

```javascript
<a v-bind:[attributeName]="url"> ... </a>
<a v-on:[eventName]="doSomething"> ... </a>
<a v-bind:['foo' + bar]="value"> ... </a>
```

Modifiers

```javascript
<form v-on:submit.prevent="onSubmit"> ... </form>
```



#### Shorthands

v-bind Shorthand

```javascript
<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a :[key]="url"> ... </a>
```

v-on Shorthand

```javascript
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```



### Computed Properties and Watchers



#### Computed Properties



##### Basic Example

Putting too much logic in your templates can make them bloated and hard to maintain. For example:

```javascript
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

For any complex logic, you should use a **computed property**.

```javascript
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>

var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
```



##### Computed vs Watched Property

Watch

```javascript
<div id="demo">{{ fullName }}</div>

var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

Computed

```javascript
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```



##### Computed Setter

Computed properties are by default getter-only, but you can also provide a setter when you need it.

```javascript
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```



#### Watchers

```javascript
<div id="demo">{{ fullName }}</div>

var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```



### Class and Style Bindings

#### Binding HTML Classes

##### Object Syntax

1.  Bind fields in the object

```javascript
<div v-bind:class="{ active: isActive }"></div>
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>

data: {
  isActive: true,
  hasError: false
}
```

2. The bound object

```javascript
<div v-bind:class="classObject"></div>
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

3. Bind to a [computed property](https://vuejs.org/v2/guide/computed.html) 

```javascript
<div v-bind:class="classObject"></div>
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

##### Array Syntax

```javascript
<div v-bind:class="[activeClass, errorClass]"></div>

data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

```javascript
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

##### With Components

```javascript
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
<my-component class="baz boo"></my-component>

// result
<p class="foo bar baz boo">Hi</p>
```

```javascript
<my-component v-bind:class="{ active: isActive }"></my-component>
```



#### Binding Inline Styles

Object Syntax

```javascript
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

data: {
  activeColor: 'red',
  fontSize: 30
}
```

```javascript
<div v-bind:style="styleObject"></div>

data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

Array Syntax

```javascript
div v-bind:style="[baseStyles, overridingStyles]"></div>
```



### Conditional Rendering

`v-if`

```javascript
<h1 v-if="awesome">Vue is awesome!</h1>
```

```javas
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

```javascript
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

`v-else-if`

```javascript
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

`v-show`

In comparison, `v-show` is much simpler - the element is always rendered regardless of initial condition, with CSS-based toggling.

```javascript
<h1 v-show="ok">Hello!</h1>
```



### List Rendering

`v-for` with an Array

```javascript
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>

var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

```javascript
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>

var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

`v-for` with an Object

```javascript
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>

new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
```

```javascript
<div v-for="(value, name) in object">
  {{ name }}: {{ value }}
</div>
```

```javascript
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```

Filtered/Sorted Results

```javascript
<li v-for="n in evenNumbers">{{ n }}</li>

data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

```javascript
<li v-for="n in even(numbers)">{{ n }}</li>

data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

`v-for` with a Range

```javascript
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```

`v-for` with `v-if`

```javascript
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```

```javascript
<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>
```

`v-for` with a Component

```javascript
<my-component v-for="item in items" :key="item.id"></my-component>
```

```javascript
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index"
  v-bind:key="item.id"
></my-component>
```

a complete example of a simple todo list:

```javascript
<div id="todo-list-example">
  <form v-on:submit.prevent="addNewTodo">
    <label for="new-todo">Add a todo</label>
    <input
      v-model="newTodoText"
      id="new-todo"
      placeholder="E.g. Feed the cat"
    >
    <button>Add</button>
  </form>
  <ul>
    <li
      is="todo-item"
      v-for="(todo, index) in todos"
      v-bind:key="todo.id"
      v-bind:title="todo.title"
      v-on:remove="todos.splice(index, 1)"
    ></li>
  </ul>
</div>
```

```javascript
Vue.component('todo-item', {
  template: '\
    <li>\
      {{ title }}\
      <button v-on:click="$emit(\'remove\')">Remove</button>\
    </li>\
  ',
  props: ['title']
})

new Vue({
  el: '#todo-list-example',
  data: {
    newTodoText: '',
    todos: [
      {
        id: 1,
        title: 'Do the dishes',
      },
      {
        id: 2,
        title: 'Take out the trash',
      },
      {
        id: 3,
        title: 'Mow the lawn'
      }
    ],
    nextTodoId: 4
  },
  methods: {
    addNewTodo: function () {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText
      })
      this.newTodoText = ''
    }
  }
})
```



### Event Handling

#### Listening to Events

```javascript
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>

var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
```

#### Method Event Handlers

```javascript
<div id="example-2">
  <!-- `greet` is the name of a method defined below -->
  <button v-on:click="greet">Greet</button>
</div>

var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  // define methods under the `methods` object
  methods: {
    greet: function (event) {
      // `this` inside methods points to the Vue instance
      alert('Hello ' + this.name + '!')
      // `event` is the native DOM event
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})

// you can invoke methods in JavaScript too
example2.greet() // => 'Hello Vue.js!'
```

#### Methods in Inline Handlers

```javascript
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>

new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
```

Pass DOM

```javascript
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>

methods: {
  warn: function (message, event) {
    // now we have access to the native event
    if (event) event.preventDefault()
    alert(message)
  }
}
```

#### Event Modifiers

```javascript
<!-- the click event's propagation will be stopped -->
<a v-on:click.stop="doThis"></a>

<!-- the submit event will no longer reload the page -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- modifiers can be chained -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- just the modifier -->
<form v-on:submit.prevent></form>

<!-- use capture mode when adding the event listener -->
<!-- i.e. an event targeting an inner element is handled here before being handled by that element -->
<div v-on:click.capture="doThis">...</div>

<!-- only trigger handler if event.target is the element itself -->
<!-- i.e. not from a child element -->
<div v-on:click.self="doThat">...</div>
```



### Form Input Bindings

#### Basic Usage

##### Text

```javascript
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

##### Checkbox

```javascript
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

```javascript
<div id='example-3'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>

new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
```

##### Radio

```javascript
<input type="radio" id="one" value="One" v-model="picked">
<label for="one">One</label>
<br>
<input type="radio" id="two" value="Two" v-model="picked">
<label for="two">Two</label>
<br>
<span>Picked: {{ picked }}</span>
```

##### Select

```javascript
<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<span>Selected: {{ selected }}</span>

new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
```

Dynamic options rendered with `v-for`:

```javascript
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Selected: {{ selected }}</span>

new Vue({
  el: '...',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
```

#### Value Bindings

- bind the value to a dynamic property on the Vue instance

- bind the input value to non-string values.

Checkbox 

```javascript
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no"
>

// when checked:
vm.toggle === 'yes'
// when unchecked:
vm.toggle === 'no'      
```

Radio

```javascript
<input type="radio" v-model="pick" v-bind:value="a">
    
// when checked:
vm.pick === vm.a
```

Select Options

```javascript
<select v-model="selected">
  <!-- inline object literal -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>

// when selected:
typeof vm.selected // => 'object'
vm.selected.number // => 123
```



#### Modifiers

`v-model` syncs the input with the data after each `input` event. You can add the `lazy` modifier to instead sync after `change` events

```javascript
<input v-model.lazy="msg" >
```

user input to be automatically typecast as a number

```javascript
<input v-model.number="age" type="number">
```

user input to be trimmed automatically

```javascript
<input v-model.trim="msg">
```



### Components Basics

##### Base Example

```javascript
<div id="components-demo">
  <button-counter></button-counter>
</div>

Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})

new Vue({ el: '#components-demo' })
```

##### Reusing Components

```javascript
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>

Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})

new Vue({ el: '#components-demo' })
```

each component is a new **instance** of it is created.  each one maintains its own properties.

**a component’s data option must be a function**, so that each instance can maintain an independent copy of the returned data object:

Organizing Components

Registered components.



##### Passing Data to Child Components with Props

```javascript
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>

Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```

```javascript
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>

new Vue({
  el: '#blog-post-demo',
  data: {
    posts: [
      { id: 1, title: 'My journey with Vue' },
      { id: 2, title: 'Blogging with Vue' },
      { id: 3, title: 'Why Vue is so fun' }
    ]
  }
})
```



A Single Root Element

Listening to Child Components Events

Content Distribution with Slots

Dynamic Components

DOM Template Parsing Caveats