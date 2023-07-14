<template>
  <div ref="body">
    <div class="imgs grid grid-cols-2 gap-4">
      <div class="flex b justify-center align-middle" v-for="(item, index) in 4" :key="index">
        <img
          class="block w-6/12 object-cover active:scale-90 transition-all duration-300 ease-in-out"
          :src="`https://picsum.photos/200?index=${index}`"
          alt=""
      /></div>
    </div>
    <div @click="add" @mousedown="add">{{ data.name }}</div>
  </div>
</template>

<script setup>
import { onMounted, reactive, ref } from 'vue'
import { ElCard, ElButton } from 'element-plus'

import ColorThief from 'color-thief'
let body = ref(null)

let data = reactive({
  name: 1
})
let add = () => {
  console.log(new ColorThief().getPalette())
}

onMounted(() => {
  console.log(body.value)
  body.value.style.backgroundImage = `linear-gradient(to right, #fff, #fff, #fff)`
  body.value.style.transition = 'background-image 3s ease-in-out'
  body.value.querySelectorAll('.imgs img').forEach((item) => {
    item.crossOrigin = ''
    item.onload = function () {
      let [colors1, colors2, colors3] = new ColorThief().getPalette(item, 3)
      console.log(this, this.parentNode)
      this.parentNode.style.backgroundImage = `linear-gradient(to right, rgb(${colors1[0]},${colors1[1]},${colors1[2]}), rgb(${colors2[0]},${colors2[1]},${colors2[2]}), rgb(${colors3[0]},${colors3[1]},${colors3[2]}))`
    }

    item.addEventListener('mouseenter', () => {
      let [colors1, colors2, colors3] = new ColorThief().getPalette(item, 3)
      body.value.style.backgroundImage = `linear-gradient(to right, rgb(${colors1[0]},${colors1[1]},${colors1[2]}), rgb(${colors2[0]},${colors2[1]},${colors2[2]}), rgb(${colors3[0]},${colors3[1]},${colors3[2]}))`
    })
    item.addEventListener('mouseleave', () => {
      body.value.style.backgroundImage = `linear-gradient(to right, #fff, #fff, #fff)`
    })
  })
})
</script>
