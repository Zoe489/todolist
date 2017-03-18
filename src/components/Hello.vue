<template>
  <div class="hello">
    <h1>{{title}}</h1>
    <input v-model="newItem" v-on:keyup.enter="addNew">
    <ul>
      <li v-for="item in items" v-bind:class="{finished:item.isFinished}" >
        {{ item.label }}<button v-on:click="toggleFinish(item)">x</button>
      </li>
    </ul>
    <p>son tells me : {{childwords}}</p>
    <componentA msgfromfather="helloson" v-on:child-tell-me-something="listenToMyBoy"></componentA>
  </div>
</template>

<script>
import Store from '../store.js'
import componentA from './componentA.vue'
export default {
  name: 'hello',
  data :function() {
    return {
      title: 'this is a todo list',
      items: Store.fetch(),
      newItem : '',
      childwords:''
    }
  },
  components: {
    componentA
  },
  watch:{
    items:{
      handler : function (items) {
        Store.save(items)
      },
      deep:true
    }
  },
  methods: {
    toggleFinish: function(item){
      item.isFinished = !item.isFinished
    },
    addNew: function(){
      this.items.push({
        label: this.newItem,
        isFinished:false
      })
      this.newItem = ''
    },
    listenToMyBoy:function(msg) {
      this.childwords = msg
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.finished{
  text-decoration: underline;
}
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
