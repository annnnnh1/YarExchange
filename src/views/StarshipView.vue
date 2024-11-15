<script setup>
import { useRoute } from 'vue-router';
import axios from 'axios';
import { onMounted, ref, inject } from 'vue';

const route = useRoute()
const starship = ref([])
const films = ref([])
const date = [{
    created: null,
    edited: null
}]
const {currentPage} = inject('page')

const fetchStarship = async () => {
    try {
        const { data } = await axios.get(`https://swapi.dev/api/starships/?page=${currentPage.value}`)
        
        starship.value = data.results.find((starship) => starship.name == route.params.name)
        date.created = new Date(starship.value.created).toLocaleDateString()
        date.edited = new Date(starship.value.edited).toLocaleDateString()

    } catch(err) {
        console.log(err)
    }
}

const fetchFilms = async () => {
    try {
        const { data } = await axios.get(`${starship.value.films}`)
        films.value = data.title

    } catch(err) {
        console.log(err)
    }
}


onMounted(async () => {
    await fetchStarship()
    await fetchFilms()
})


</script>


<template>
    <h1 class="flex justify-center text-[35px] mb-5">{{starship.name}}</h1>
    <ul class="ul">
        <li><span>Model:</span> {{ starship.model }}</li>
        <li><span>Manufacturer:</span> {{ starship.manufacturer }}</li>
        <li><span>Cost in credits:</span> {{ starship.cost_in_credits }}</li>
        <li><span>Length:</span> {{ starship.length }} meters</li>
        <li><span>Max atmosphering speed:</span> {{ starship.max_atmosphering_speed }}</li>
        <li><span>Crew:</span> {{ starship.crew }}</li>
        <li><span>Passengers:</span> {{ starship.passengers }}</li>
        <li><span>Cargo capacity:</span> {{ starship.cargo_capacity }}</li>
        <li><span>Consumables:</span> {{ starship.consumables }}</li>
        <li><span>Hyperdrive rating:</span> {{ starship.hyperdrive_rating }}</li>
        <li><span>MGLT:</span> {{ starship.MGLT }}</li>
        <li><span>Starship class:</span> {{ starship.starship_class }}</li>
        <li><span>Pilots:</span> {{ starship.name }}</li>
        <li><span>Films:</span> {{ films }}</li>
        <li><span>Created:</span> {{ date.created }}</li>
        <li><span>Edited:</span> {{ date.edited }}</li>
    </ul>
</template>

<style scoped>

span {
    font-size: 25px;
}

</style>
