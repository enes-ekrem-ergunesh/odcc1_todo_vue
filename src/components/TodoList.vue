<template>
  <v-row class="align-start">
    <v-col v-for="todo in msg" class="v-col-12 pb-0 pt-2">
      <v-card class="">
        <v-container fluid class="pa-0">
          <v-row dense>

            <v-checkbox-btn v-model="todo.done" class="ml-3" color="" value="true"></v-checkbox-btn>

            <v-card-item>
              <v-card-title v-bind:class="{ 'line-through': todo.done }">{{ todo.title }}</v-card-title>
              <v-card-subtitle v-bind:class="{ 'line-through': todo.done }">{{ todo.description }}</v-card-subtitle>
            </v-card-item>

            <v-spacer></v-spacer>

            <v-btn @click="deleteTodo(todo)" icon="mdi-delete" color="red" class="mt-3 mr-3"></v-btn>

          </v-row>
        </v-container>
      </v-card>
    </v-col>
  </v-row>
</template>

<script lang="ts" setup>
import { watch } from 'vue'

// Connection with TodoListContainer
const props = defineProps({
  msg: {
    type: Array as () => { title: string, description: string, done: boolean }[]
  }
})

// Delete Todo
function deleteTodo(todo: { title: string, description: string, done: boolean }) {
  // Remove the todo from the "msg" prop
  if (props.msg) {
    const msgIndex = props.msg.indexOf(todo)
    if (msgIndex !== -1) {
      props.msg.splice(msgIndex, 1)
    }
  }
  // Save the updated "ls_todo_data" array back to local storage
  localStorage.setItem('ls_todo_data', JSON.stringify(props.msg))
}

watch(() => props.msg, (newValue) => {
  localStorage.setItem('ls_todo_data', JSON.stringify(newValue))
}, {
  deep: true
})

</script>

<style>
.line-through {
  text-decoration: line-through;
}
</style>