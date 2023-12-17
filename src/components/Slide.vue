<script setup>
import { ref } from 'vue';

const { id, title } = defineProps(["id", "title"])

const movies = ref([])
const row = ref(null)

let scroll = direction => {
    let { scrollLeft, clientWidth } = row.value
    let { paddingLeft, paddingRight } = getComputedStyle(row.value)

    let width = clientWidth - parseInt(paddingLeft) - parseInt(paddingRight)

    row.value.scrollTo({
        left: scrollLeft + width * direction,
        behavior: "smooth"
    })
}

fetch(`https://api.themoviedb.org/3/discover/movie?api_key=664b88830136f3ffbf5fd056348144bc&with_genres=${id}`)
    .then(response => response.json())
    .then(data => movies.value = data.results)
    .catch(error => alert(error.message))
</script>

<template>
    <div>
        <h2 class="text-white ml-16 text-lg font-bold">{{ title }}</h2>
        <div class="relative group">
            <button @click="scroll(-1)" class="text-white bg-[#141414]/50 absolute inset-y-2 w-12 rounded-r-md opacity-0 group-hover:opacity-100 transition-opacity text-xl">
                <i class="fa-solid fa-angle-left"></i>
            </button>
            <div ref="row" class="flex px-14 overflow-auto scrollbar-hide">
                <div class="w-1/6 shrink-0 p-2" v-for="{ backdrop_path, id } in movies" :key="id">
                    <img @click="$emit('view', id)" class="rounded-md hover:scale-105 transition-transform cursor-pointer" :src="`https://image.tmdb.org/t/p/original${backdrop_path}`">
                </div>
            </div>
            <button @click="scroll(1)" class="text-white bg-[#141414]/50 absolute inset-y-2 right-0 w-12 rounded-l-md opacity-0 group-hover:opacity-100 transition-opacity text-xl">
                <i class="fa-solid fa-angle-right"></i>
            </button>
        </div>
    </div>
</template>