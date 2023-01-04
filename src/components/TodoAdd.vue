<template>
  <v-row class="fill-height">
    <v-form ref="form" class="w-100">
      <v-row class="fill-height">
        <v-col :cols="12">

          <v-text-field  v-model="title" label="Title" :rules="rules" hide-details="auto" required></v-text-field>

          <v-textarea v-model="description" label="Description"></v-textarea>

        </v-col>
        <v-col :cols="12" align-self="end">

          <v-btn block @click="validate" style="margin-bottom: -30px;">Add Todo</v-btn>

        </v-col>
      </v-row>
    </v-form>
  </v-row>
</template>

<script lang="ts" setup>
import { ref, ComponentOptions } from 'vue'

// Declare a reactive variable to store the form component
const form = ref<ComponentOptions | null>(null)

// Declare a reactive variable to store the validation rules for the form
const rules = ref([
  (value: string) => !!value || 'Required.',
])

// Declare reactive variables to store the form data
const title = ref('')
const description = ref('')
const done = ref(false)
var todoData = ref([{}])

// Connection with TodoAddContainer
const emit = defineEmits(['response'])

// Declare a function to validate the form and save the data to local storage
async function validate() {
  // Check if the form is not null
  if (form.value) {
    // Validate the form
    const { valid } = await form.value.validate()

    // If the form is valid, proceed to save the data
    if (valid) {
      // Check if the "ls_todo_data" array exists in local storage
      // If it does not, create it as an empty array
      if (!localStorage.getItem('ls_todo_data')) {
        localStorage.setItem('ls_todo_data', '[]')
      }

      // Retrieve the "ls_todo_data" array from local storage and store it in a variable
      todoData.value = JSON.parse(localStorage.getItem('ls_todo_data') || '{}')

      // Push the form data to the "ls_todo_data" array
      todoData.value.push({
        title: title.value,
        description: (description.value === '') ? 'No description' : description.value,
        done: done.value
      })

      // Save the updated "ls_todo_data" array back to local storage
      localStorage.setItem('ls_todo_data', JSON.stringify(todoData.value))

      // Send the form data to the parent component (final destination is TodoList component)
      emit('response', JSON.parse(localStorage.getItem('ls_todo_data') || '{}'))
    }
  }
}

// Send the form data to the parent component (final destination is TodoList component)
emit('response', JSON.parse(localStorage.getItem('ls_todo_data') || '{}'))

</script>
