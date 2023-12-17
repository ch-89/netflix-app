<script setup>
import { computed, ref } from 'vue';

const movies = ref([])
const index = ref(0)
const duration = 7000

const movie = computed(() => movies.value[index.value])

const next = e => index.value == movies.value.length - 1 ? index.value = 0 : index.value++

let timer = setInterval(next, duration)

const visible = e => e.target.hidden ? clearInterval(timer) : timer = setInterval(next, duration)

addEventListener("visibilitychange", visible)

fetch("https://api.themoviedb.org/3/trending/all/week?api_key=664b88830136f3ffbf5fd056348144bc")
    .then(response => response.json())
    .then(data => movies.value = data.results)
    .catch(error => alert(error.message))
</script>

<template>
    <div v-if="movie" class="relative">
        <Transition enter-from-class="opacity-0" leave-to-class="opacity-0" leave-active-class="absolute">
            <div class="transition-opacity duration-[2s]" :key="index">
                <img :src="`https://image.tmdb.org/t/p/original${movie.backdrop_path}`" class="h-[60vh] w-screen object-cover">
                <div class="absolute bottom-16 left-16 max-w-4xl grid gap-4 z-10">
                    <h2 class="text-white text-6xl font-bold drop-shadow-md" style="text-wrap: balance;">{{ movie.title || movie.name }}</h2>
                    <p class="text-white text-xl drop-shadow-md line-clamp-3">{{ movie.overview }}</p>
                    <div class="flex gap-4 font-bold">
                        <button class="bg-white px-6 py-3 rounded-lg text-[#141414] drop-shadow-md">
                            <i class="fa-solid fa-play"></i> Play
                        </button>
                        <button class="bg-[#141414]/20 backdrop-blur-sm px-6 py-3 rounded-lg text-white drop-shadow-md">
                            <i class="fa-solid fa-info-circle"></i> More Info
                        </button>
                    </div>
                </div>
                <div class="bg-gradient-to-t from-[#141414] h-36 absolute w-full bottom-0"></div>
            </div>
        </Transition>
    </div>
</template>