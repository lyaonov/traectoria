<script setup lang="ts">
import { defineEmits } from 'vue';


const emit = defineEmits(["remove",'sort']);

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


</script>

<template>
    <ul class="list" v-if="itemList.length !== 0">
        <li  class="list-item card" style="width: 505px; margin: 0 auto; border: none; box-shadow: none;">
            <p class="title">Номер</p>
            <p class="title">Название</p>
            <p class="title">Модель </p>
            <p v-on:click="() => emit('sort', 'year')" class="title sort" >Год &#8645;</p>
            <p v-on:click="() => emit('sort', 'price')" class="title sort" >Цена &#8645;</p>
            <p class="title">Цвет</p>
            <p class="title">Удаление</p>
        </li>
        <li class="list-item card" v-for="(item, i) in props.itemList ">
            <p class="counter">{{ i + 1 }}</p>
            <input class="change-field" style="border-left:1px rgba(104, 104, 104,0.5) solid;" type="text" v-model="item.name">
            <input class="change-field" type="text" v-model="item.model">
            <input type="text" readonly v-model="item.year">
            <input style="border-right:none" class="change-field" type="text" v-model="item.price">
            <div class="car-color" :style="{ background: item.color }"></div>
            <button class="danger btn" v-on:click="() => emit('remove', item.id)">Удалить</button>
        </li>
    </ul>
    <h1 v-else>Машин нет :(</h1>
</template>

<style>
.title{
    padding: auto 10px;
}

.sort:hover{
    cursor:pointer;
    box-shadow: 0px 3px  rgba(244, 242, 242, 0.2);
}

.car-color {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    margin-right: 15px;
}
</style>