<template>
  <div v-for="user in state.info">
    <p>
      <b>{{state.names[user.username]}}</b><br/>
      Level: {{ user.rank.rank }}	&nbsp;	&nbsp;	&nbsp;	&nbsp;
      Score: {{ user.score }}	&nbsp;	&nbsp;	&nbsp;	&nbsp;
      Next level in: {{ user.nextRank }}<br/>
    </p>
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
    "Liman223",
]
const names = {
  "magdalenak": "Magda",
  "radekwlsk": "Radek",
  "Nerjan": "Janek",
  "MKaras93": "Michał",
  "arudzinska": "Ania",
  "Mc01": "Marcin",
  "Liman223": "Piotrek",
}
const ranks = [
  {rank: "8 kyu", score: 0},
  {rank: "7 kyu", score: 20},
  {rank: "6 kyu", score: 76},
  {rank: "5 kyu", score: 229},
  {rank: "4 kyu", score: 643},
  {rank: "3 kyu", score: 1768},
  {rank: "2 kyu", score: 4829},
  {rank: "1 kyu", score: 13147},
  {rank: "1 dan", score: 35759},
  {rank: "2 dan", score: 97225},
]

function determineRank(score) {
  let highestRank = ranks[0]
  for (let level of ranks) {
    if (score >= level.score && level.score > highestRank.score) {
      highestRank = level
    }
  }
  return highestRank
}

function nextRank(rank, score) {
  for (let level of ranks) {
    if (level.score > rank.score) {
      return level.score - score
    }
  }
}

let state = reactive({ info: {}, names: names })
for (let user of users) {
  axios.get(`${baseUrl}/${user}`).then((response) => {
    let score = response.data.ranks.overall.score
    let rank = determineRank(score)
    state.info[user] = {
      username: response.data.username,
      score: score,
      rank: rank,
      nextRank: nextRank(rank, score),
    }
  })
}
</script>

<style scoped></style>
