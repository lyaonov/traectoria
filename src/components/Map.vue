<script setup lang="ts">
import { onMounted } from 'vue';
import 'leaflet/dist/leaflet.css'
import L from 'leaflet'

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

interface Props {
    itemList: RequestItem[];
    sticky?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
    sticky: false,
});



onMounted(() => {
    createMapLayer()
})

const createMapLayer = () => {
    let map = L.map('map').setView([59.98, 30.32], 12)

    L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
    }).addTo(map);
    props.itemList.forEach(item => L.marker([item.latitude,item.longitude]).addTo(map).bindPopup(item.name + ' ' + item.model))
}
</script>

<template>
    <div id="map"></div>
</template>
  
<style scoped>
#map {
  height: 650px;
  width: 700px;
  margin-top: 20px;
  border-radius: 25px;
}
</style>