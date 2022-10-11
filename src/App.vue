<script>
import {onMounted, ref, reactive} from 'vue'
import axios from 'axios'

export default {
  name: "Tasks",
  data() {
    return {
      tasks: [],
      task: {},
      title: '',
      details: '',
      id: '',
    }
  },
  mounted() {
      axios.get('http://localhost:8000/api/tasks').then(response => (this.tasks = response.data))
  },
  methods: {
      async makeTask() {
      try {
        const newApp = await axios.post(
          'http://localhost:8000/api/tasks',
          {
            title: this.title,
            details: this.details
          }
      ).then(axios.get('http://localhost:8000/api/tasks').then(response => (this.tasks = response.data)))
    } catch(err) {
        console.log(err);
      }
    },
      async cancelTask(id) {
        try {
          await axios.delete('http://localhost:8000/api/tasks/' + id).then(axios.get('http://localhost:8000/api/tasks').then(response => (this.tasks = response.data)))
        } catch(err){
          console.log(err)
    }
}
}
}
</script>

<template>
  <h1>
    Vue To-Do List
  </h1>
  <div class = 'taskCard' v-for='task in tasks'>
   <h2>{{task.title}}</h2>
   <p>{{task.details}}</p>
   <button @click='cancelTask(task.id)'>Complete</button>
  </div>
  <form v-on:submit.prevent='preventDefault'>
    <p>Title:<input type='text' v-model='title'></p>
    <p>Details:<input type='text' v-model='details'></p>
    <input type='submit' value='Add Task' @click='makeTask'>
  </form>
</template>

<style scoped>

.taskCard {
  background-color: grey;
  text-align: center;
  padding: 25px;
  margin: 20px;
  color: white;
}

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
