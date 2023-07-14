<template>
  <div style="inset: 20px; position: absolute" class="flex-column">
    <div class="flex align-center padding-y10 bg-blue-200"
      ><div class="w10 flex-shrink-0 flex-ju-start-al-center padding-left10">序号</div>
      <div class="w20 flex-shrink-0 flex-ju-start-al-center padding-left10">姓名</div
      ><div class="w30 flex-shrink-0 flex-ju-start-al-center padding-left10">姓名2</div
      ><div class="w40 flex-shrink-0 flex-ju-start-al-center padding-left10">姓名3</div></div
    >
    <swiper
      direction="vertical"
      :slides-per-view="15"
      :space-between="0"
      @swiper="onSwiper"
      @slideChange="onSlideChange"
      :modules="modules"
      :pagination="{
        clickable: true
      }"
      :keyboard="{
        enabled: true
      }"
      :mousewheel="true"
      :autoplay="{
        delay: 2500,
        disableOnInteraction: false,
        pauseOnMouseEnter: true
      }"
      loop
    >
      <swiper-slide
        :style="{ '--i': 360 * Math.random() + 1 + 'deg' }"
        v-for="(item, i) in 100"
        :key="i"
        ><div
          class="flex align-center w100-h100 hover-bg-blue text-black"
          :class="{ 'bg-cyan-100': i % 2 === 0, 'bg-orange-100': i % 2 === 1 }"
        >
          <div class="w10 flex-shrink-0 flex-ju-start-al-center padding-left10 border-box">{{
            i + 1
          }}</div
          ><div class="w20 flex-shrink-0 flex-ju-start-al-center padding-left10 border-box"
            ><div class="flex-shrink-0 ell w100">{{
              Math.random()
                .toString(16)
                .repeat(Math.ceil(Math.random() * 6))
            }}</div></div
          ><div class="w30 flex-shrink-0 flex-ju-start-al-center padding-left10 border-box"
            ><div class="flex-shrink-0 ell w100">{{
              Math.random()
                .toString(16)
                .repeat(Math.ceil(Math.random() * 6))
            }}</div></div
          >
          <div class="w40 flex-shrink-0 flex-ju-start-al-center padding-left10 border-box"
            ><div class="flex-shrink-0 ell w100" :title="'123456'">{{
              Math.random()
                .toString(16)
                .repeat(Math.ceil(Math.random() * 6))
            }}</div></div
          ></div
        ></swiper-slide
      >
    </swiper></div
  >
</template>
<script>
// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Mousewheel, Pagination, Autoplay, Keyboard } from 'swiper/modules'

import 'swiper/css/pagination'
// Import Swiper styles
import 'swiper/css'

export default {
  components: {
    Swiper,
    SwiperSlide
  },
  setup() {
    const onSwiper = (swiper) => {
      console.log(swiper)
    }
    const onSlideChange = () => {
      console.log('slide change')
    }
    return {
      onSwiper,
      onSlideChange,
      modules: [Mousewheel /*, Pagination*/, Autoplay, Keyboard]
    }
  }
}
</script>
<style lang="less" scoped>
.swiper {
  flex: 1;
  flex-shrink: 0;
  width: 100%;

  box-sizing: border-box;
}
.swiper-slide {
  // background-image: linear-gradient(135deg, #5433ff, #20bdff, #a5fecb);
  // filter: hue-rotate(var(--i));
  color: black;
  width: 100%;
  overflow: hidden;
}
.swiper-slide + .swiper-slide {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
