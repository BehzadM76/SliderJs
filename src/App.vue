<script setup>
import Slider from "@/components/Slider.vue";

const imagesList = [
  '/images/colored-stone.jpg',
  '/images/projectBg.jpg',
  '/images/collection-page-bg.jpg',
  '/images/gallery-bg.jpg',
  '/images/projectsSectionBg.jpg'
]
</script>

<template>

  <main>
    <div class="wrapper min-h-screen h-screen p-10 w-full">
      <Slider class="w-full h-full" :images-list="imagesList"/>
    </div>
  </main>
</template>

<style scoped>

</style>
