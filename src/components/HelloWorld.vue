<template>
  <h1>CodeWars Ranking</h1>
  <div v-for="user in state.info">
    <p>{{state.names[user.username]}}: {{ user.ranks.overall.score }}</p>
  </div>
</template>

<script setup>
import axios from 'axios'
import { reactive } from 'vue'


const baseUrl = "https://www.codewars.com/api/v1/users"
const users = [
    "magdalenak",
    "radekwlsk",
    "Nerjan",
    "MKaras93",
    "arudzinska",
    "Mc01",
]
const names = {
  "magdalenak": "Magda",
  "radekwlsk": "Radek",
  "Nerjan": "Janek",
  "MKaras93": "Michał",
  "arudzinska": "Ania",
  "Mc01": "Marcin",
}

let state = reactive({ info: {}, names: names })
for (let user of users) {
  axios.get(`${baseUrl}/${user}`).then((response) => {
    state.info[user] = response.data
  })
}
</script>

<style scoped></style>
