<script lang="ts" setup>

import {ref} from "vue";

const props = defineProps({
  slidesPerView: {
    type: Number,
    default: 3
  },

  imagesList: {
    type: Array,
    required: true
  }
})
const range = (start: number, end: number) => Array.from({length: end - start}, (_, i) => start + i);
const activeImgIndex = ref(0)


const prevIconAction = () => {
  if (activeImgIndex.value !== 0) {
    activeImgIndex.value--
  }

  if (activeImgIndex.value !== 0 && activeImgIndex.value === firstSlideIndex.value) {
    lastSlideIndex.value--
    firstSlideIndex.value--
  }
}

const nextIconAction = () => {
  if (activeImgIndex.value !== props.imagesList.length - 1) {
    activeImgIndex.value++
  }

  if (activeImgIndex.value !== props.imagesList.length - 1 && activeImgIndex.value === lastSlideIndex.value - 1) {
    firstSlideIndex.value++
    lastSlideIndex.value++
  }
}

const clickSlideImg = (index: number) => {
  if (index !== 0 && index === firstSlideIndex.value) {
    firstSlideIndex.value--
    lastSlideIndex.value--
  } else if (index !== props.imagesList.length - 1 && index === lastSlideIndex.value - 1) {
    firstSlideIndex.value++
    lastSlideIndex.value++
  }
  activeImgIndex.value = index
}

const firstSlideIndex = ref(0)
const lastSlideIndex = ref(props.imagesList.length > props.slidesPerView ? props.slidesPerView : props.imagesList.length)

</script>

<template>
  <div class="w-full h-full relative overflow-hidden rounded-[inherit]">

    <!-- Animated background slide -->
    <transition mode="out-in" name="slide">
      <div
          :key="props.imagesList[activeImgIndex]"
          :style="{ backgroundImage: `url(${props.imagesList[activeImgIndex]})` }"
          class="absolute inset-0 bg-cover bg-center z-0"
      />
    </transition>

    <!-- Foreground content -->
    <div class="relative z-[2] h-full flex flex-col justify-end p-5 sm:py-10 sm:px-8">

      <!-- Controls -->
      <div class="absolute w-full flex justify-between px-5 sm:px-8 top-1/2 left-0">
        <div
            class="swiper-button-prev-custom cursor-pointer bg-white/70 flex items-center justify-center h-16 w-21 rounded-[20px] py-7 px-5"
            @click="prevIconAction">
          <img src="/icons/ArrowL.svg">
        </div>
        <div
            class="swiper-button-next-custom cursor-pointer bg-white/70 flex items-center justify-center h-16 w-21 rounded-[20px] py-7 px-5"
            @click="nextIconAction">
          <img src="/icons/ArrowR.svg">
        </div>
      </div>

      <!-- Thumbnails -->
      <div
          class="bg-white/70 rounded-2xl gap-4 sm:gap-8 w-full py-4 px-3.5 sm:px-4 sm:py-2 flex justify-center">
        <img
            v-for="i in range(firstSlideIndex, lastSlideIndex)"
            :key="i"
            :src="props.imagesList[i]"
            alt=""
            class="w-36 max-h-[60px] rounded-2xl sm:max-h-20 object-cover"
            @click="clickSlideImg(i)">
      </div>
    </div>
  </div>
</template>

<style scoped>
.slide-enter-active, .slide-leave-active {
  transition: all 0.6s ease;
  position: absolute;
  width: 100%;
  height: 100%;
}

.slide-enter-from {
  transform: translateX(100%);
  opacity: 0;
}

.slide-leave-to {
  transform: translateX(-100%);
  opacity: 0;
}
</style>
