<script setup>

import { inject } from 'vue';

import StarshipList from '@/components/StarshipList.vue';

const {goToNext, backToPrev, queryStarships, searchQuery} = inject('starship')



</script>

<template>
    <div class="flex justify-between items-center mb-10">
        <h1 class="text-3xl font-bold">Все корабли</h1>
        <div class="flex items-center gap-2 ">
            <input type="text" v-model="searchQuery"
                class="border border-gray-200 rounded-md py-2 px-4 focus:outline-none focus:border-gray-400"
                placeholder="Поиск..." />
        </div>
    </div>
    <div class="flex flex-col gap-3">
        <StarshipList v-for="starship in queryStarships" :starship="starship" :key="starship"/>
    </div>
    <div class="flex gap-3 justify-center mt-5 text-white">
        <button class="py-2 px-5 bg-slate-600 rounded-md" @click="backToPrev">Prev</button>
        <button class="py-2 px-5 bg-slate-600 rounded-md" @click="goToNext">Next</button>
    </div>
</template>
