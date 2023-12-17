<script setup>
import { ref } from 'vue';
import Banner from './components/Banner.vue';
import Navbar from './components/Navbar.vue';
import Slide from './components/Slide.vue';
import Modal from './components/Modal.vue';

const slides = ref([
  { id: 28, title: "Action" },
  { id: 35, title: "Comedy" },
  { id: 27, title: "Horror" },
])

const id = ref(null)
</script>

<template>
  <div class="bg-[#141414] min-h-screen">
    <Navbar/>
    <Banner/>
    <Modal v-if="id" :id="id" @close="id = null" />
    <main class="py-16 grid gap-16">
      <Slide v-for="slide in slides" :key="slide.id" v-bind="slide" @view="id = $event" />
    </main>
  </div>
</template>