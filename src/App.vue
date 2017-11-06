<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <input v-model="newItem" v-on:keyup.enter="onEnter">
    <ul>
      <li v-bind:class="{finished:item.isFinished}" v-for="item in items" v-bind:key="item.label" v-on:click="toggleFinish(item)">
        {{ item.label }}
      </li>
    </ul>
  </div>
</template>

<script>
import Store from './store'
console.log(Store)
export default {
  name: 'app',
  data () {
    return {
      title: 'this is a todo list',
      newItem: '',
      items: Store.fetch()
    }
  },
  methods: { 
    onEnter:function(){
      this.items.push({
        label: this.newItem,
        isFinished: false
      })
      this.newItem = ''
    },
    toggleFinish: function(item) {
    item.isFinished = !item.isFinished
    }
  },
  watch: {
    items: {
      handler: function (items) {
        Store.save(items)
      },
      deep:true
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.finished {
  text-decoration: underline;
}
</style>
