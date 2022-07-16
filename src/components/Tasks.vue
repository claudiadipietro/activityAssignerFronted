<template>
  <div id="input">
    <h1>
      Enter your tasks below!
    </h1>
    <input type="text" v-model="itemToPush">
    <button class="bigButton" v-on:click="pushItem">+</button>
  </div>
  <br>
  <div v-for="(item, index) in items" v-bind:key="index">
    <p>{{item}}</p>
  </div>
  <br>
  <button class="bigButton" v-on:click="showTasks">Show your tasks!</button>
  <div class="taskDiv" v-for="task in tasks" v-bind:key="task._id">
    <p v-if="!isEditing" :class="task.taskStatus?'tasksSorted':null">{{task.item}}</p>
    <input class ="editInput" type="text" v-if="isEditing" v-model="itemToUpdate">
    <button class="deleteButton" v-if="!isEditing" v-on:click="deleteTask(task._id)">Delete</button>
    <button class="editButton" v-on:click="editTask">Edit</button>
    <button class="editButton" v-if="isEditing" v-on:click="updateTask(task._id)">Update</button>
  </div>
  <button class="bigButton" v-if="this.tasks?.length > 0" v-on:click="sortTasks">Sort your tasks!</button>
</template>

<script>
import axios from 'axios'; 

export default {
  name: 'TasksVue',
  data() {
    return {
      items: [],
      tasks: null,
      isEditing: false
    }
  },
  methods: {
    showTasks() {
      axios.get('http://localhost:8080/api/getList').then(response => {
      const listOfTasks = response.data
      this.tasks = listOfTasks
      this.items = []
    });       
    },
    pushItem() {
      if(this.itemToPush){
        this.items.push(this.itemToPush)
        axios.post('http://localhost:8080/api/createItem', {
        item: this.itemToPush.toString()
      }).then(()=>{
        if(this.tasks){
          this.items = []
          this.showTasks()
        }
      })
      this.itemToPush = ""
      }else{
        alert("The input can't be empty")
      }
    },
      sortTasks(){
        const notPicked = this.tasks.filter(task => task.taskStatus === false)
        if (notPicked){
          const randomPick = Math.floor(Math.random() * notPicked.length)
          const randomTask = notPicked[randomPick]
          axios.put(`http://localhost:8080/api/taskStatusChanged/${randomTask._id}`, {}).then(() => {
            this.showTasks()
          }
          )
          alert(`Your task is ${randomTask.item}!`)
        }
      },
      deleteTask(id){
        axios.delete(`http://localhost:8080/api/deleteItem/${id}`, {}).then(() => {
            this.showTasks()
        })
      },
      editTask(){
        this.isEditing = !this.isEditing
        
      },
      updateTask(id){
        if(this.itemToUpdate){
          this.items.push(this.itemToUpdate)
          axios.put(`http://localhost:8080/api/updateItem/${id}`, {
          item: this.itemToUpdate.toString()
        }).then(()=>{
          if(this.tasks){
            this.items = []
            this.showTasks()
          }
        })
        this.itemToUpdate = ""
        this.isEditing = false
      }
    }
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
.bigButton{
  border-radius: 10px;
  background-color: papayawhip;
  border-color: papayawhip;
  margin-left: 1rem;
  padding: 1rem;
  font-family: 'Courier New', Courier, monospace;
  font-size: medium; 
}
.tasksSorted{
  text-decoration: line-through;
}
.deleteButton{
  border-radius: 5px;
  margin-left: 10px;
  border-color: rosybrown;
}
.editButton{
  border-radius: 5px;
  margin-left: 10px;
  border-color: rgb(143, 183, 188);
}
.taskDiv{
  display: flex;
  justify-content: center;
  align-items: center;  
}
.editInput{
  width: 150px;
  margin-bottom: 10px;
  margin-top: 10px;
  height: 10px;
}
</style>
