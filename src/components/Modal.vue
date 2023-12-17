<script setup>
import { computed, ref } from 'vue';

const { id } = defineProps(["id"])

const movie = ref(null)

const key = computed(() => movie.value.videos.results.find(video => video.type == "Trailer").key)
const cast = computed(() => movie.value.credits.cast.slice(0, 3).map(cast => cast.name).join(", "))
const genres = computed(() => movie.value.genres.map(genre => genre.name).join(", "))

const runtime = computed(() => {
    let hours = Math.floor(movie.value.runtime / 60),
        minutes = Math.floor(movie.value.runtime % 60)

    return `${hours}h ${minutes}m`
})

fetch(`http://api.themoviedb.org/3/movie/${id}?api_key=664b88830136f3ffbf5fd056348144bc&append_to_response=videos,credits,images`)
    .then(response => response.json())
    .then(data => movie.value = data)
</script>

<template>
    <div v-if="movie" class="bg-black/10 backdrop-blur-sm fixed inset-0 z-30 grid place-items-center">
        <div class="bg-[#181818] max-w-4xl w-full text-white drop-shadow-lg rounded-3xl overflow-hidden">
            <div class="relative">
                <iframe v-if="key" class="w-full aspect-video" :src="`https://www.youtube.com/embed/${key}?si=6e6kUuw7THLbTyfs&amp;controls=0&autoplay=1`" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
                <img v-else :src="`https://image.tmdb.org/t/p/original${movie.backdrop_path}`">
                <button @click="$emit('close')" class="bg-[#141414]/50 absolute top-12 right-12 w-10 aspect-square rounded-full">
                    <i class="fa-solid fa-xmark"></i>
                </button>
                <div class="absolute bottom-12 grid gap-4 inset-x-12">
                    <img class="w-36 drop-shadow-md" :src="`https://image.tmdb.org/t/p/original${movie.images.logos[0].file_path}`" v-if="movie.images.logos.length">
                    <div class="flex gap-4">
                        <button class="bg-white text-[#141414] px-4 rounded-md font-bold">
                            <i class="fa-solid fa-play"></i> Play
                        </button>
                        <button class="bg-[#141414]/50 w-10 aspect-square rounded-full">
                            <i class="fa-solid fa-plus"></i>
                        </button>
                        <button class="bg-[#141414]/50 w-10 aspect-square rounded-full">
                            <i class="fa-solid fa-thumbs-up"></i>
                        </button>
                        <button class="bg-[#141414]/50 w-10 aspect-square rounded-full ml-auto">
                            <i class="fa-solid fa-volume-high"></i>
                        </button>
                    </div>
                </div>
            </div>
            <div class="p-12 grid grid-cols-[2fr_1fr] gap-12 items-start">
                <div class="grid gap-4">
                    <div class="flex items-center gap-2 font-semibold">
                        <div class="text-[#46d369]">100% Match</div>
                        <div>{{ movie.release_date }}</div>
                        <div>{{ runtime }}</div>
                        <div class="border border-white text-xs px-1 rounded">HD</div>
                    </div>
                    <div class="flex items-center gap-2 text-lg">
                        <i class="fa-solid fa-star text-[#e50914]"></i>
                        <p class="font-semibold">No.4 in Films Today</p>
                    </div>
                    <p>{{ movie.overview }}</p>
                </div>
                <div class="text-sm grid gap-4">
                    <div>
                        <span class="text-[#777]">Cast:</span> {{ cast }}
                    </div>
                    <div>
                        <span class="text-[#777]">Genres:</span> {{ genres }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>