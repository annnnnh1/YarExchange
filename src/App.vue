<script setup>
import axios from 'axios';
import { ref, onMounted, provide, watch, computed } from 'vue';

const starships = ref([])
const pagination = ref()
const currentPage = ref(1)
const loadingPage = ref(false)
const searchQuery = ref('')



const fetchStarships = async () => {
    try {
        loadingPage.value = false

        const { data } = await axios.get(`https://swapi.dev/api/starships/?page=${currentPage.value}`)
        starships.value = data.results
        pagination.value = data.count

        loadingPage.value = true
        console.log(data.results[1].name)

    } catch(err) {
        console.log(err)
    }
} 

const goToNext = () => {
    if(currentPage.value < pagination.value / 10 && loadingPage.value == true ) {
        currentPage.value++
    }
}

const backToPrev = () => {
    if(currentPage.value > 1 && loadingPage.value == true ) {
        currentPage.value--
    }
}

const queryStarships = computed(() => {
    let strshp = starships.value
    let search = searchQuery.value

    if(searchQuery.value) {
        strshp = strshp.filter((starship) => {
            return starship.name.toLowerCase().includes(search.toLowerCase()) ||
            starship.model.toLowerCase().includes(search.toLowerCase())
        })
    }
    return strshp
})

watch(currentPage, () => {
    fetchStarships()
    localStorage.setItem('page', JSON.stringify(currentPage.value))
})

onMounted(async () => {
   await fetchStarships()
   const localCart = localStorage.getItem('page');
   currentPage.value = localCart ? JSON.parse(localCart) : 1;
})

provide('starship', {
    starships,
    queryStarships,
    searchQuery,
    backToPrev,
    goToNext,
})

provide('page',{
    currentPage
})
</script>

<template>
    <router-link to="/">
        <header class="text-white text-[50px] flex justify-center">Starships</header>
        
    </router-link>
    <div class="w-4/5 bg-white m-auto mt-[50px] shadow-xl p-10">
        <router-view/>
    </div>
</template>

