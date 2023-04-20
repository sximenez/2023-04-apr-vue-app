# Vue

Source: [Vue](https://vuejs.org/guide/introduction.html)

Vue is a JavaScript library to develop user interfaces.

Like React, it is based on a component-based approach.

## Single-file component

Every component in Vue can be developed within a single `.vue` file : logic, content and style.

```Javascript
<!------------ Logic ------------>

<script>
export default {
  data() {
    return {
      count: 0
    }
  }
}
</script>

<!------------ Content ------------>

<template>
  <button @click="count++">Count is: {{ count }}</button>
</template>

<!------------ Style ------------>

<style scoped>
button {
  font-weight: bold;
}
</style>
```

## API declaration

Logic in Vue can be declared using either one of two styles :

- Option: we define an object whose options are functions modifying data.
- Composition: we import functions that modify data using the `setup` attribute.

Both styles can be used.

However, for readability and reusibility reasons, it is recommended to stick with the composition style.

## Declarative rendering

The core feature of Vue is `declarative rendering`.

By encapsulating HTML within `template`, any state change happening at the logic level with automatically render.

### State updates (composition style)

`reactive()` can be used for states triggering changes.

However, it only works with arrays or objects.

```Javascript
// We import the reactive() function
import { reactive } from 'vue'

// We expose the state and any methods we want (functions) using the setup() hook
export default {
  setup() {
    const state = reactive({ count: 0 })

    function increment() {
      state.count++
    }

    return {
      state,
      increment
    }
  }
}
```

```Javascript
// The exposed function can be used as an event listener in the template
<template>
  <button @click="increment">
    {{ state.count }}
  </button>
</template>
```

We can refactor using `<script setup>`:

```Javascript
<script setup>
import { reactive } from 'vue'

const state = reactive({ count: 0 })

function increment() {
  state.count++
}
</script>

<template>
  <button @click="increment">
    {{ state.count }}
  </button>
</template>
```

`ref()`, on the other hand, addresses the limitations of `reactive()` by working with everything else.

`ref()` creates a reference to any value and can be passed around different components.

For example, in `composable functions`.

```Javascript
<script setup>
import { reactive, ref } from 'vue'

const counter1 = reactive({ count: 1 })
const counter2 = ref(1)

function increment() {
  counter1.count*=5;
  counter2.value*=5;
}

const message = ref('Hello World!')
</script>

<template>
  <h1>{{ message }}</h1>
  <p>Count is: {{ counter1.count }}</p>
  <p>Count is: {{ counter2 }}</p>
  <button @click="increment">
    Click here
  </button>
</template>
```

## Data binding and directives

Mustaches `{{ hello }}` are the most basic form of binding state and data in Vue.

However, they can't be used as HTML attributes like this:

```Javascript
<div {{ hello }}>Hello World</div>
```

To address this limitation, Vue uses directives: special attributes starting with `v-`.

Directives can be applied to HTML tags.

```Javascript
<script setup>
const test = "<span style='color:red'>hello world</span>";
</script>

<template>
  <p>{{test}}</p> <!-- <span style='color:red'>hello world</span> -->
  <p v-html="test"></p> <!-- hello world -->
</template>
```

```Markdown
/!\ `v-html` renders HTML and can pose security risks (XSS vulnerabilities).

It is best to never use v-html on user-provided content.
```

### v-bind

The `v-bind` directive and its shorthand `:` are the most frequent data binding attribute in Vue.

We can bind an id, a class, a boolean, an object, even functions.

```Javascript
<div
  :id="hello"
  :class="world"
  :boolean="isTrue"
  v-bind="object"
  :function="function(data)"
>
  {{hello(data)}}
</div>
```

### Event listeners

To listen to events, we can use the `v-on:event="handler"` or its shorthand `@event="handler"`.

Handlers can apply inline expressions or functions declared in the logic of the component (via the `setup` hook):

```Javascript
<script setup>

  import { ref } from 'vue'

  const count = ref(0)

  function increment() {
    count++;
  }

</script>

// Inline

<p>{{ count }}</p>
<button @click="count++"></button>

// Method

<button @click="increment"></button>

```

Event have modifiers like `@submit.prevent="onSubmit"`, which will apply an `event.preventDefault();` after a form submit for example.

Modifiers can be chained, but word order is determinant.

### Form bindings

`v-model` allows two-way bindings: an event listener like `@input` and a `v-bind` for rendering combined.

Like input and output in one single step. This allows to refactor code a lot.

```Javascript
<script setup>
import { ref } from 'vue'

const text = ref('')
</script>

<template>
  <input v-model="text" placeholder="Type here">
  <p>{{ text }}</p>
</template>
```

### Conditionals

Conditionals can be declared using `v-if`, `v-else` or `v-else-if`.

### List iteration

We can render lists with `v-for="x in list" :key=id.x`.

To create an entry, we can `push` a new element to the array.

To delete an entrey, wen can `filter` the list and render the new array.

```Javascript
<script setup>
import { ref } from 'vue'

// give each todo a unique id
let id = 0

const newTodo = ref('')
const todos = ref([
  { id: id++, text: 'Learn HTML' },
  { id: id++, text: 'Learn JavaScript' },
  { id: id++, text: 'Learn Vue' }
])

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter(e => e !== todo)
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{ todo.text }}
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
</template>
```

### computed()

When we have an element meeting two or more criteria, we can use the `computed()` property to target it.

This notion is a bit more tricky to grasp.

### Lifecycle hooks

Components have a "lifecycle", meaning we can target them when they are mounted, updated or unmounted.

Some corresponding hooks are `onMounted`, `onUpdated` and `onUnmounted`.

### Watchers

### Components

Like in React, components in Vue have a similar logic and template syntax `<Component />`.

### Props

Like in React, components can accept `props` from a parent element.

We use the `defineProps()` macro to declare the props of the component.

And then we can access the props using `v-bind` or `:`.

```Javascript
<script setup>
const props = defineProps({
  msg: String
})
</script>
```

### Emits

The other way around is also possible: emitting data from a component to a parent element.

On the child component, we use `defineEmits()` and `emit()`:

```Javascript
<script setup>
const emit = defineEmits(['response'])

emit('response', 'hello from child')
</script>
```

On the parent, we can access the data via `@response="(e) => textComponent = e"`:

```Javascript
<template>
  <ChildComp @response="(msg) => childMsg = msg" />
  <p>{{ childMsg }}</p>
</template>
```

### Slots

A parent and a child can also share template fragments!

1. From parent to child:

```Javascript
// Child
<template>
  <slot/>
</template>

// Parent
<template>
  <ChildComp>hello</ChildComp>
</template>
```

2. And from child to parent:

```Javascript
// Child
<template>
  <slot>Hello</slot>
</template>

// Parent
<template>
  <ChildComp></ChildComp>
</template>
```

## Github Pages Deployment

Source: [Github](https://github.com/marketplace/actions/vue-to-github-pages)

### 1. Config file

```Javascript
// Using Vite + Vue 3, add to your vite.config.js
export default defineConfig({
  ... // Already existing configurations
  base: '/YourRepoName/'
});
```
