<script setup>
import { ref, watch, computed, reactive } from 'vue'
import BlogPost from './components/BlogPost.vue'
import AlertBox from './components/AlertBox.vue'
import Home from './components/Home.vue'
import About from './components/About.vue'

//Routes defined for router components
const routes = {
  '/': Home,
  '/about': About
}

const currentPath = ref(window.location.hash)

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
})

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/'] || NotFound
})

const rawHtml = ref('<span style="color: red">This should be red.</span>')
const msg= ref('Hello')

const objectOfAttrs = { id: 'container', class: 'wrapper' }

//Defining a method
const sayHello = () => {
  alert('Hello there!')
}

//Reactivity Fundamentals - creating a reactive variable with ref()
const count = ref(0)

//Method to increment the count
const incrementCount = () => {
  count.value++
}

//Constants defined for computing properties
const firstName = 'Jessica'
const lastName = 'W'
// Compute properties to combine first and last name
const fullName = computed(() => { return `${firstName} ${lastName}`})

//Define for binding HTML class, v-bind:class
const isActive = true
const hasError = false

//Define for binding HTML class, v-bind:style
const textColor = 'pink';
const textSize = 18;

//Define for v-for list rendering with an Object
const myObject = reactive({
  title: 'How to do lists in Vue',
  author: 'Jane Doe',
  publishedAt: '2019-09-06'
})

//Define for v-for and v-if list rendering on <template>
    const items = [
    { id: 1, name: 'Item 1', visible: true },
    { id: 2, name: 'Item 2', visible: false },
    { id: 3, name: 'Item 3', visible: true }
];

//Define event handler for listening to events using v-on:click
const handleClick = () => {
  alert('The event handler was listened via v-on:click!')
}

//Define constant and function for element to be dispatched via the alert
const name = ref('Vue.js')

function greet(event) {
  alert(`Hello ${name.value}!`)
  // `event` is the native DOM event
  if (event) {
    alert(event.target.tagName)
  }
}

const message = ref('This is for textarea input.')
const picked = ref('')
const selected = ref('')
const age = ref(0)

const question = ref('')
const answer = ref('Questions usually contain a question mark.')

//With Composition API, the watch function is used to trigger a callback when there is a change of state
watch(question, async (newQuestion) => {
    if (newQuestion.indexOf('?') > -1) {
        answer.value = 'Thinking...'
    }
    else {
        answer.value = "Please ask the question!"
    }

})

const props = defineProps(['title'])
console.log(props.title)

const posts = ref([
  { id: 1, title: 'Blogpost Title 1' },
  { id: 2, title: 'Blogpost Title 2' },
  { id: 3, title: 'Blogpost Title 3' }
])

const postFontSize = ref(1)

// const routes = {
//   '/': Home,
//   '/about': About
// }

// const currentPath = ref(window.location.hash)

// window.addEventListener('hashchange', () => {
//   currentPath.value = window.location.hash
// })

// const currentView = computed(() => {
//   return routes[currentPath.value.slice(1) || '/'] || NotFound
// })

</script>

<template>

    <!--{{ }} are used for JavaScript expressions inside syntax-->
    <div>

        <!--Router for Home and About page-->
        <a href="#/">Home</a>  |  
        <a href="#/about">About</a>
        <component :is="currentView" />

        <br><br>

        <!--Text interpolation-->
        <p>Using text interpolation: {{ rawHtml }}</p><br>

        <!--v-html directive-->
        <p>Using v-html directive: <span v-html="rawHtml"></span></p><br>

        <!--v-bind shorthand-->
        <div :id="objectOfAttrs">This element uses v-bind shorthand containing dynamic ID</div><br>

        <!--Methods-->
        <button @click="sayHello">Click to say hello</button>

        <br><br>

        <!--Reactivity Fundamentals-->
        <p>{{ count }}</p>
        <button @click="incrementCount">Click to +</button>

        <br><br>

        <!--Computed Properties-->
        <h2>{{ fullName }}</h2>

        <br><br>

        <!--Class and Style Bindings using v-bind:class-->
        <div :class="{'active': isActive, 'error': hasError}">Conditional Classes (v-bind:class when isActive is true and hasError is false)</div>

        <br><br>

        <!--Class and Style Bindings using v-bind:style-->
        <div :style="{ color: textColor, fontSize: textSize + 'px' }">Inline Styles using v-bind:style</div>

        <br><br>

        <!--List rendering using v-for with an object-->
        <ul>
            <li v-for="value in myObject">
                {{ value }}
            </li>
        </ul>

        <br><br>

        <!--List rendering using v-for with a Range-->
        <span v-for="n in 50">{{ n }}</span>

        <br><br>

        <!--List rendering using v-for on <template>-->
            <ul>
                <template v-for="item in items">
                    <li>{{ item.name }}</li>
                </template>
            </ul>

        <br><br>

        <!--List rendering using v-for with v-if, in this case Item 2 is false for visible-->
          <template v-for="item in items">
            <ul>
              <li v-if="!item.visible">
                {{ item.name }}
              </li>
            </ul>
          </template>

        <br><br>

        <!--Listening to event handler with v-on click-->
        <button @click="handleClick">Click me</button>

        <br><br>

        <!--Listening to event inline handlers-->
        <button @click="count++">Add 1</button>
        <p>Count is: {{ count }}</p>

        <br><br>

        <!--Listening to event method handlers-->
        <button @click="greet">Greet</button>

        <br><br>

        <!--Form input binding where v-model was used with <input type="text">-->
        Text input with v-model: <br>
        <input type="text" v-model="msg">

        {{ msg }} <!--Text interpolation with v-model where msg property within the curly brackets is replaced from the constant defined in script-->

        <br><br>

        <!--Form input binding where v-model was used with <textarea>-->
        Multiline text input with v-model: <br>
        <textarea v-model.trim.lazy="message" placeholder="Add multiple lines"></textarea> <!--Trim and lazy modifiers are added for whitespace to be trimmed and let the changed input to be bind-->

        {{  message }} <!--Text interpolation with v-model where message property within curly brackets is replaced from the constant defined in script-->

        <br><br>

         <!--Form input binding where v-model was used with <input type="checkbox">-->
        Single checkbox with v-model:
        <input type="checkbox" v-model="checked" />
        {{ checked }}

        <br><br>

         <!--Form input binding where v-model was used with <input type="radio">-->
        Radio with v-model:
        <input type="radio" id="one" value="One" v-model="picked" />
        <label for="one">Pick</label>

        <br><br>

        <!--Form input binding where v-model was used with <select>-->
        Select with v-model:
        <select v-model="selected">
          <option disabled value="">Please select one</option>
          <option>Coffee</option>
          <option>Tea</option>
        </select>

        <br><br>

        <!--Number modifier is added to typecase input as a number-->
        Enter age:
        <input type="number" id="age" v-model.number="age">

        <br><br>

        <!--Watchers-->
          Ask a yes or no question:
          <input v-model="question">
        <br>
          {{  answer }}

        <br><br>

        <!--Props and events [$emits] attributes created in BlogPost.vue component is passed into here, together with v-for on Component-->
        <div :style="{ fontSize: postFontSize + 'em' }">
            <BlogPost
            v-for="post in posts"
            :key="post.id"
            :title="post.title"
            @enlarge-text="postFontSize += 0.1"
            ></BlogPost>
        </div>

        <br><br>

        <!--Slots attributes in AlertBox.vue component are passed into here -->
        <AlertBox>
            Something might have gone wrong :o
        </AlertBox>

    </div>

</template>

<style scoped>

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
