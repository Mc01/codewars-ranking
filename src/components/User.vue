<template>
  <h1>CodeWars Ranking</h1>
  <p>{{ state.info }}</p>
  <p>{{ state.info }}</p>
  <p>Test</p>
</template>

<script setup>
import axios from 'axios'
import { defineProps, reactive } from 'vue'

defineProps({
  user: String
})

const baseUrl = "https://www.codewars.com/api/v1/users"
let state = reactive({ info: {} })
axios.get(`${baseUrl}/${user}`).then((response) => {
  state.info[user] = response.data
})
</script>

<style scoped></style>
