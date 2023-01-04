<template>
  <v-container class="rounded pb-0" v-resize="onResize" v-bind:style="{ height: containerHeightComputed }"
    style="min-height: 340px; border: 1px solid grey">
    <!-- <p>{{ todoAddMsg }}</p> -->
    <TodoAdd @response="onResponse" />
  </v-container>
</template>

<script lang="ts" setup>
import { ref, computed, watch } from 'vue'
import TodoAdd from '@/components/TodoAdd.vue'

// Declare a reactive variable to store the height of the container
const containerHeight = ref(0)

// Declare a reactive variable to store the value of the remaining size to be subtracted from the height of the container
const remainingSize = ref(100)

// Declare a function to update the value of the reactive variable
const setContainerHeight = (height: number) => {
  containerHeight.value = height
}

// Declare a computed property to return the height of the container in pixels
const containerHeightComputed = computed(() => {
  return containerHeight.value + 'px'
})

// Declare a function to update the height of the container based on the window height and the remaining size
async function onResize() {
  setContainerHeight(window.innerHeight - remainingSize.value)
}

// Watch for changes to the value of remainingSize and update the height of the container accordingly
watch(() => remainingSize.value, (newValue) => {
  setContainerHeight(window.innerHeight - newValue)
})

// Call the function to set the initial height of the container
onResize()

// Connection with TodoAdd
const todoAddMsg = ref({})

// Connection with Todo
const emit = defineEmits(['response'])

function onResponse(msg: { title: string, description: string, done: boolean}) {
  todoAddMsg.value = msg

  // Send the form data to the parent component (final destination is TodoList component)
  emit('response', msg)
}


</script>