<template>
  <div class="home">
    <v-text-field
      outlined
      label="Add"
      append-icon="mdi-plus"
      class="pa-3"
      hide-details
      clearable
      v-model="newTaskTitle"
      @click:append="addTask"
      @keyup.enter="addTask"
    ></v-text-field>
    <v-list
      two-line
      flat
    >
      <div v-for="task in tasks" :key="task.id">
        <v-divider></v-divider>
        <v-list-item
          @click="doneTask(task.id)"
          :class="{ 'blue lighten-5': task.done }"
        >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox
                :input-value="task.done "
                color="primary"
              ></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{ 'text-decoration-line-through': task.done }"
              >
                {{ task.title }}
              </v-list-item-title>
            </v-list-item-content>
            <v-list-item-action>
              <v-btn icon @click.stop="deleteTask(task.id)">
                <v-icon color="red lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
      </div>
    </v-list>
  </div>
</template>

<script>
  import { uuid } from 'vue-uuid'; 
  export default {
    name: 'Todo',
    data() {
      return {
        newTaskTitle: "",
        tasks:[]
      }
    },
    mounted() {
      this.tasks = localStorage.getItem('tasksList') ? JSON.parse(localStorage.getItem('tasksList')) : [];
    },
    methods: {
      doneTask(id) {
        let task = this.tasks.filter((task) => {return task.id === id})[0]
        task.done = !task.done

        localStorage.setItem('tasksList', JSON.stringify(this.tasks))
      },
      deleteTask(id) {
        this.tasks = this.tasks.filter((task) => {return task.id !== id})

        localStorage.setItem('tasksList', JSON.stringify(this.tasks))
      },
      addTask() {
        this.tasks.push({
          id: uuid.v1(),
          title: this.newTaskTitle,
          done: false
        })
        localStorage.setItem('tasksList', JSON.stringify(this.tasks))
        this.newTaskTitle = "";
      }
    }
  }
</script>
