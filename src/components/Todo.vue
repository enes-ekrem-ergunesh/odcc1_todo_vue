<template>
  <v-container class="fill-height">
    <v-btn>Hello</v-btn>
    <v-row class="fill-height justify-center">
      <v-col class="" :cols="12" sm="6" lg="5" xl="3" style="min-width: 450px;">
        <TodoListContainer :msg="childMsg" />
      </v-col>
      <v-col :cols="12" sm="5" lg="7" xl="" style="min-width: 430px;">
        <TodoAddContainer @response="onResponse" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import TodoListContainer from '@/components/TodoListContainer.vue'
import TodoAddContainer from '@/components/TodoAddContainer.vue'

// Initialize the local host storage
if (!localStorage.getItem('ls_todo_data')) {
  localStorage.setItem('ls_todo_data', '[]')
}

// Connection between TodoAddContainer and TodoListContainer
const childMsg = ref<{ title: string, description: string, done: boolean }[]>([])
function onResponse(msg: { title: string; description: string, done: boolean }[]) {
  // Send the form data to the parent component (final destination is TodoList component)
  childMsg.value = msg
}

</script>