# Vue.js Training (Basic)

> Core Syntax, Templates, Directives (v-bind, v-html, v-on, v-show, v-if, v-else, v-for), Data, Methods, Computed Properties, Watchers

## What is Vue.js

Vue.js (Vue) is a **JavaScript framework** that makes building **interactive (交互式的)** and **reactive (响应式的)** web frontends (= browser-side web applications) easier.

But "Just JavaScript" May Not Be Ideal!

## Why Vue.js

jQuery:


```javascript
<ul class="js-items"></ul>

<script>
  $(function () {
    $.get('https://example.com/items.json')
      .then(function (data) {
        var $itemsUl = $('.js-items');

        if (!data.items.length) {
          var $noItems = $('li');
          $noItems.text('Sorry, there are no items.');
          $itemsUl.append($noItems);
        } else {
          data.items.forEach(function (item) {
            var $newItem = $('li');
            $newItem.text(item);

            if (item.includes('blue')) {
              $newItem.addClass('is-blue');
            }
            
            $itemsUl.append($newItem);
          });
        }
    });
  });
</script>
```

Vue

```vue
<ul class="js-items">
  <li v-if="!items.length">Sorry, there are no items.</li>
  <li v-for="item in items" :class="{ 'is-blue': item.includes('blue') }">
  {{ item }}</li>
</ul>

<script>
  new Vue({
  el: '.js-items',
  data (): {
    return {
      items: []
    }
  },

  created() {
    fetch('https://example.com/items.json')
      .then((res) => res.json())
      .then((data) => {
        this.items = data.items;
      });
    }
  });
</script>
```

## Different Ways to Utilizing Vue

1. Vue can be used to **control parts** of HTML pages or entire pages
   - **"Widget"** approach on a multi-page-application. (Some) pages are still rendered on and served by a backend server.
2. Vue can also be used to **control the entire frontend** of a web application
   - **Single-Page-Application (SPA)** approach. Server only sends one HTML page, thereafter, Vue takes over and controls the UI

## Interpolation and Data Binding

`v-bind`: directive, is basically an instruction

1. Interpolation. `{{ }}`

   Note: the double curly braces syntax is only available between opening and closing **HTML tags**

2. Pass in a dynamic value to an attribute. Use the Vue binding syntax.

   ```vue
   <a v-bind:href="vueLink">
   ```

## "methods" in Vue Apps

A Vue **method** is an object associated with the Vue instance. Functions are defined inside the **methods** object. Methods are useful when you need to perform some action with **v-on** directive on an element to handle events. Functions defined inside the methods object can be further called for performing actions.

**Syntax:**

```vue
methods : {
  // We can add our functions here
}
```

**Syntax for single file components:**

```vue
export default {
  methods: {
    // We can add our functions here
  }
}
```

## Working with Data inside of a Vue App

This is how we tell Vue what data we want to display in the template.

```vue
  data () {
    return {
      activeTabIdInfo: 0,
      expandedKeys: {},
      platforms: [],
      visibleLeft: false,
      projects: [],
      projectItems: [
        {
          label: 'All',
          to: '/platform/job',
          icon: 'pi pi-fw pi-th-large'
        }
      ]
    }
```

## Outputting Raw HTML Content with v-html

```vue
<p v-html="outputGoal()"></p>

<p v-html="courseGoalB"></p>
```

## Event Binding

Directive: `v-on`

```vue
<button @click="counter++"></button>
```

## Events and Methods

*basics-04-events-methods*

```vue
// Directive: v-on
<button v-on:click="counter++"></button>

// Put logic in methods function
<button v-on:click="add"></button>

// Event Arguments
<button v-on:click="add(5)"></button>

```

## Using the Native Event Object

*basics-05-using-the-native-event-object*

HTML

```html
<input type="text" v-on:input="setName">
<input type="text" v-on:input="setName($event, ‘Wang’)">

<p>Your name: {{ name }}</p>
```

JS

```vue
setName(event) {
	this.name = event.target.value
}

setName(event, lastName) {
	this.name = event.target.value + lastName
}
```

## Exploring Event Modifiers

And the reason for that is that the browser default for a form with a button like this is to submit that form and send an HTTP request to the server serving this app.

but typically with frameworks like Vue, you wanna prevent this browser default and instead you wanna handle this manually in JavaScript with help of Vue in your Vue app and you wanna read the user input there, validate it and then send it manually to a backend server and store it in a database, for example.

`v-on:click` == `@click`

```html
<form v-on:submit.prevent="submitForm">
  <input type="text">
  <button>
    Sign Up
  </button>
</form>

<button v-on:click.right="add(10)">
  Add 10
</button>

<input 
  type="text"
  v-on:input="setName($event, 'Zac')"
  v-on:keyup:enter="confirmInput"
>
```

## Locking Content with v-once

```html
<p v-once>Starting Counter: {{ counter }}</p>
<p> Result: {{ counter }}</p>
```

## Data Binding + Event Binding = Two-Way Binding

*basics-06-two-way-binding*

`v-model`  = `v-bind:value` + `v-on:input`

`v-bind:lastName` == `:lastName` 

And this is a concept called two-way binding because we're communicating in both ways, we are listening to an event coming out of the input element you could say to the input event. And at the same time, we're writing the value back to the input element through its value attribute.

That's why it's called two-way binding because we communicate in both directions.

```html
<input type="text" v-model="name">
```

## Methods used for Data Binding: How it Works

## Introducing Computed Properties

*basics-07-introducing-computed-properties*

Computed properties are essentially like methods with one important difference view will be aware of their dependencies and **only reexecute them if one of the dependencies changed**. We use it just like we use **data** properties.

You still need **methods** either because you have something that you really want to reexecute whenever anything changed.

or more often, because you have events and you wanna trigger certain methods when an event occurs, that does not change.

- You still bind your events to methods.
- You don't bind events to computer properties.
- You really only use computer properties for outputting something like here, the full name.

```javascript
<p>Your Name: {{ fullname }}</p>


computed: {
    fullname() {
        if (this.name == '') {
            return '';
        }
        return this.name + ' ' + 'Wang';
    }
}
```

## Working with Watchers

*basics-08-working-with-watchers*

Let's say when the counter exceeds 50, anything like that, we wanna reset it.

That's the type of task where a watcher shines. We wanna change counter when something happens.

Another example, would be HTTP requests, which you wanna send if certain data changes, or timers which you wanna set, if certain values change.

- If you wanna do that execute code, because something changed, then watchers can be helpful.
- If you just want to calculate some output value dynamically, computed properties are your friend.

```javascript
watch: {
    name() {
        this.fullName = this.name + ' ' + 'Wang';
    }
    
    name(value) {
        this.fullName = value + ' ' + 'Wang';
    }
    
    name(newValue, oldValue) {
        this.fullName = newValue + ' ' + 'Wang';
    }
}
```

## Methods vs Computed vs Watch

#### When to use methods

- To react on some event happening in the DOM
- To call a function when something happens in your component. You can call a methods from computed properties or watchers.

#### When to use computed properties

- You need to compose new data from existing data sources
- You have a variable you use in your template that’s built from one or more data properties
- You want to reduce a complicated, nested property name to a more readable and easy to use one, yet update it when the original property changes
- You need to reference a value from the template. In this case, creating a computed property is the best thing because it’s cached.
- You need to listen to changes of more than one data property

#### When to use watchers

- You want to listen when a data property changes, and perform some action
- You want to listen to a prop value change
- You only need to listen to one specific property (you can’t watch multiple properties at the same time)
- You want to watch a data property until it reaches some specific value and then do something

## Dynamic Styling with Inline Styles

*basics-10-styling-starting-setup*

```html
<div>
    class="demo"
    :style="{borderColor: boxASelected ? 'red' : '#ccc'}"
    @click="boxSelected('A')"
</div>
```

## Adding CSS Classes Dynamically

*basics-11-dynamic-styling-inline-styles*

```html
<div>
    <!-- :class="boxASelected ? 'demo active' : 'demo'" -->
    :class="{demo: true, active: boxASelected}"
    @click="boxSelected('A')"
</div>

<div>
    class="demo"
    :class="{active: boxASelected}"
    @click="boxSelected('A')"
</div>
```

## Classes & Computed Properties

## Dynamic Classes: Array Syntax

*basics-13-dynamic-classes-array-syntax*

```
<div>
    :class="['demo', {active: boxASelected}]"
    @click="boxSelected('A')"
</div>
```

## Rendering Content Conditionally

*lists-cond-02-v-if-v-else-v-else-if*

`v-if, v-else-if, v-else`

So, v-if, v-else-if and v-else can be used to control which parts of your HTML code are really showing up on the final page.

And, I want to emphasize that it's not just about showing and hiding stuff.

It's really about including those elements in the DOM or not including them.

It's really about attaching and detaching.

```vue
<p v-if="goals.length === 0">No goals, please add one</p>
```

## Using v-show instead of v-if

adding and removing elements can cost performance. On the other hand,

Therefore, as a rule of thumb, you should typically use v-if and only fall back to v-show if you have an element,

which visibility status changes a lot. 

So, if you have like a button that toggles an element and it's switching between visibility and being hidden all the time,

that is when you might want to consider v-show in other cases use v-if.

## Rendering List of Data

*lists-cond-03-rendering-lists-of-data*

*lists-cond-04-diving-deeper-into-v-for*

`v-for`

```vue
// loop with Array
<ul v-else>
    <li v-for="goal in goals">{{ goal }}</li>
</ul>

<ul v-else>
    <li v-for="(goal, index) in goals">{{ goal }} - {{ index }}</li>
</ul>

// loop with Object
<ul v-else>
    <li v-for="(value, key, index) in {name: 'Zac', age: 31}">{{ key }}: {{ value }} - {{ index }}</li>
</ul>

// loop with numbers
<ul v-else>
    <li v-for="num in 10">{{ num }}</li>
</ul>
```

## Lists and Keys

*lists-cond-06-lists-keys*

```vue
<ul v-else>
    <li v-for="(goal, index) in goals" :key="goal">{{ goal }} - {{ index }}</li>
</ul>
```

# Vue.js Training (Intermediate)

> Vue CLI, Components, Component Communication, Vue Instance Lifecycle hooks, Behind the Scenes, Forms, HTTP (axios), Routing, Animations

## Vue CLI

https://cli.vuejs.org/

1. Install Nodejs
2. Install **Vue CLI**: `npm install -g @vue/cli`
3. `vue create vue-first-app`
4. `cd vue-first-app; npm run serve`
5. Add `vetur` extension to VSC

## Vue Instance Lifecycle

https://v3.vuejs.org/api/options-lifecycle-hooks.html

https://learnvue.co/2020/12/how-to-use-lifecycle-hooks-in-vue3/

![img](https://learnvue.co/wp-content/uploads/2020/12/2020-12-26-Vue3-Lifecycle-1024x544.png)

## Component

Components are reusable pieces of HTML with connected data and logic.

You can consider it's a **mini** app which in the **main** app

- Reusable
- Encapsulation
- Manageable

## Component Communication

> Pass data between components

**Components offer certain communication mechanisms** that allow you to exchange data between them. Hence you can build one connected UI if you work with one root app that holds multiple components (SPA).

### Parent component -> Child component

- **props**: Props is short for properties and you can think of props like custom HTML attributes.
- **unidirectional data flow**: Data passed from app to child component should only be changed in app, not in the child component. Otherwise it will error like `Unexpected mutation of "abc" prop`
  - But you can just take a prop as an initial value and then change the component exclusive property and state.
- Three format to provide props
  - []
  - {}

### Child component -> Parent component

`this.$emit('custom-event', this.id)`

```
emits: ['add-contact']
```

### provide + inject

## Sending HTTP request

native `fetch()` API

```javascript
fetch('https://vue-http-demo-85e9e.firebaseio.com/surveys.json', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: this.enteredName,
    rating: this.chosenRating,
  }),
});
```

**Axios**

```javascript
import axios from 'axios'; // at the start of your <script> tag, before you "export default ..."
...

axios.post('https://vue-http-demo-85e9e.firebaseio.com/surveys.json', {
  name: this.enteredName,
  rating: this.chosenRating,
});
```

It automatically sets the `Content-Type` header for you, it also automatically converts the body data to JSON.

**Promise**

fetch() returns a Promise, hence we can use then(), catch() and async/ await there. For Axios, it's just the same - it also returns a Promise.

Promise 对象用于表示一个异步操作的最终完成 (或失败)及其结果值。

发送一个HTTP请求将会耗时几毫秒甚至几秒， JavaScript不会等待其他code的执行直到返回结果。Fetch返回一个Promise，可以通过添加then方法来监听这个Promise, then方法接受一个函数，当结果返回时该函数自动触发执行。

**Enhance**

- Loading Data when a Component Mounts
- Showing a "Loading" message
- Handling the "No Data" state
- Handling technical / Browser-side errors
- Handling Error responses

# Vue.js Training (Advanced)

> Vuex, Authentication, Deployment & Optimizations, Composition API, Re-Using Code, Debugging

## Vuex

Vuex is a **state management pattern + library** for Vue.js applications. It serves as a centralized store for all the components in an application, with rules ensuring that the state can only be mutated in a predictable fashion.

