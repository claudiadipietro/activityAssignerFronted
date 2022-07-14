<template>
  <div id="input">
    <h1>
      Enter your tasks below!
    </h1>
    <input type="text" v-model="itemToPush">
    <button v-on:click="pushItem">+</button>
  </div>
  <br>
  <div v-for="(item, index) in items" v-bind:key="index">
    <p>{{item}}</p>
  </div>
  <br>
  <button v-on:click="showTasks">Show your tasks!</button>
</template>

<script>
import axios from 'axios'; 

export default {
  name: 'TasksVue',
  data() {
    return {
      items: []
    }
  },
  methods: {
    showTasks: function() {
      axios.get('http://localhost:8080/api/getList').then(response => {
      const listOfTasks = response.data
      console.log(listOfTasks)
});       
    },
    pushItem() {
      if(this.itemToPush){
        this.items.push(this.itemToPush)
        const axiosPromise = axios.post('http://localhost:8080/api/createItem', {
        item: this.itemToPush.toString()
      })
      this.itemToPush = ""
      axiosPromise.then(response => console.log(response))
      }else{
        alert("The input can't be empty")
      }
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
p{
  font-family: 'Courier New', Courier, monospace;
  font-size: medium;
}
h1 {
  margin: 40px;
  font-family: 'Courier New', Courier, monospace;
}
input{
  padding: 1rem;
  font-family: 'Courier New', Courier, monospace;
  padding: 1rem;
  font-family: 'Courier New', Courier, monospace;
  width: 50rem;
  border-radius: 10px;
}
button{
  border-radius: 10px;
  background-color: papayawhip;
  border-color: papayawhip;
  margin-left: 1rem;
  padding: 1rem;
  font-family: 'Courier New', Courier, monospace;
  font-size: medium; 
}
</style>
