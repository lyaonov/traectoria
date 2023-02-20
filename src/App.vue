<script setup lang="ts">
import { ref, onMounted, onBeforeMount } from 'vue';
import axios from "axios";
import CardList from './components/CardList.vue';
import Map from './components/Map.vue';

let changeMap = ref(true as boolean)

const options = {
  method: 'GET',
  url: 'https://test.tspb.su/test-task/vehicles'
}

interface RequestItem {
  "id": number,
  "name": string,
  "model": string,
  "year": number,
  "color": string,
  "price": number,
  "latitude": number,
  "longitude": number
}

const cars = ref([] as RequestItem[])

axios.request(options).then(({ data }) => {
  data.forEach((item: RequestItem) => {
    cars.value.push(item)
  });
})

function remove(id: number) {
  let index: number = cars.value.findIndex(el => el.id === id)
  cars.value.splice(index, 1)
}

function sort(titleID: string) {
  if (titleID === 'price') {
    (JSON.stringify(cars.value) == JSON.stringify(cars.value.sort((a, b) => a.price - b.price)))
      ? cars.value.reverse()
      : cars.value.sort((a, b) => a.price - b.price)
  } else {
    (JSON.stringify(cars.value) == JSON.stringify(cars.value.sort((a, b) => a.year - b.year)))
      ? cars.value.reverse()
      : cars.value.sort((a, b) => a.year - b.year)
  }
}
</script>
  
<template>
  <button v-on:click="() => changeMap = !changeMap">{{ changeMap ? 'Карта' : 'Назад'}}</button>

  <CardList v-if="changeMap" :itemList="cars" @remove="remove" @sort="sort" />
  <Map :itemList="cars" v-else />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
