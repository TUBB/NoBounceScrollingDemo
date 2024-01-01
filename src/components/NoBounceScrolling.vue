<script setup lang="ts">
import { NoBounceScrolling } from 'nobounce-scrolling';
import { onMounted, onUnmounted, ref, watchEffect } from 'vue';

  const items = ref<string[]>([])
  for (let index = 0; index < 100; index++) {
    items.value.push(`Item ${index}`)
  }

  const scrollableElement = ref(null)

  const noBounceScrolling = ref<NoBounceScrolling | null>(null)


  watchEffect(() => {
    if (scrollableElement.value) {
      noBounceScrolling.value?.destroy()
      noBounceScrolling.value = NoBounceScrolling.create(scrollableElement.value, {
        flingDelay: 200,
        resistance: 0.6,
        onScrollY(y) {
          console.log('onScrollY', y)
        },
      })
      noBounceScrolling.value.startScroll()
    }
  })

  onUnmounted(() => {
    noBounceScrolling.value?.destroy()
  })

  
</script>

<template>
  <div class="no-bounce-scrolling">
    <div>No bounce scrolling demo</div>
    <ul ref="scrollableElement">
      <li v-for="item in items" :key="item">{{ item }}</li>
    </ul>
  </div>
</template>

<style scoped>
  ul {
    height: 90vh;
    overflow-y: scroll;
    -webkit-overflow-scrolling: touch;
  }
</style>
