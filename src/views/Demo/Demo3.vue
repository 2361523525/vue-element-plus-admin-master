<template>
  <div class="w200px" ref="bodyref">
    <span class="text cursor-pointer cursor-progress"
      >水电费猎杀对决法律手段垃圾发啦随机发加大审计发啦受打击发啦较大覅偶国际法倒垃圾过啦电极法较大塑料袋会计分录时间到了飞机了受打击发啦受打击发啦</span
    >
    <section>
      <div class="info">
        <div class="name"></div>
      </div>
    </section>
  </div>
  <div id="colors">
    <el-button @click="currentColor = randomColor()" type="primary">生成</el-button>
    <input v-model="currentColor" type="color" /> <input v-model="currentColor" type="text" />
    <div class="flex">
      <div
        v-for="(el, idx) in 5"
        :key="idx"
        :class="[`hue-rotate${idx * 60}`]"
        class="colors flex1 relative"
        ><div
          v-for="(item, i) in generateColor(colorToHslString(currentColor))"
          @click="copyColor"
          @dblclick="copyColor2"
          :style="{
            backgroundColor: item
          }"
          :class="[
            `text-${getContrastColor(
              tinycolor(item)
                .spin(idx * 60)
                .toHexString()
            )}`,
            {
              current:
                tinycolor(item)
                  .spin(idx * 60)
                  .toHexString() == tinycolor(currentColor).toHexString()
            }
          ]"
          :data-bg="
            tinycolor(item)
              .spin(idx * 60)
              .toHexString()
          "
          :data-color="
            getContrastColor(
              tinycolor(item)
                .spin(idx * 60)
                .toHexString()
            )
          "
          class="h60px flex-center item relative"
          :key="i"
          >{{
            tinycolor(item)
              .spin(idx * 60)
              .toHexString()
          }}
          <div class="tips absolute">{{
            -(
              i - generateColor.history.map((i) => tinycolor(i).toHexString()).indexOf(currentColor)
            )
          }}</div>
        </div></div
      >
    </div>
  </div>
  <div class="jbs padding20" :style="{ backgroundImage: gradientColor, height: '800px' }">
    <el-button @click="gradientColor = randomGradientColor()" type="primary">生成</el-button>
    <el-button @click="copyGradientColor" type="primary">复制</el-button>
  </div>
  <div
    class="upload w400px h-fit-content padding20 border-radius10 bg-white grid grid-columns-4 gap20"
  >
    <div
      class="h300px w100 flex-center item relative"
      v-for="file in files"
      :key="file.time + file.status + file.size"
    >
      <img v-if="file.type == 'image'" :src="file.url" alt="" />

      <video v-else :src="file.url" controls></video>
      <div class="uploading" v-if="file.status == 'uploading'">上传中</div>
      <div class="error" v-if="file.status == 'error'">上传失败</div>
    </div>
    <div class="h300px w100 flex-center item uploadBtn" @click="handleUploadClick"
      >点击此处上传文件</div
    >
    <input class="none" @input="onfileInput" type="file" multiple />
  </div>
</template>
<script setup>
import { ElButton, ElMessage } from 'element-plus'
import tinycolor from 'tinycolor2'

import { onMounted, ref } from 'vue'
let bodyref = ref(null)
let colors = ref([])
let files = ref([])
let currentColor = ref(randomColor())
//渐变色
let gradientColor = ref(randomGradientColor())

function colorToHslString(color) {
  let r, g, b

  // 检查输入的格式
  if (color.substring(0, 1) === '#') {
    // 处理十六进制颜色
    r = parseInt(color.substring(1, 3), 16)
    g = parseInt(color.substring(3, 5), 16)
    b = parseInt(color.substring(5, 7), 16)
  } else if (color.substring(0, 4) === 'rgb(') {
    // 处理RGB颜色
    const values = color.substring(4, color.length - 1).split(',')
    r = parseInt(values[0].trim(), 10)
    g = parseInt(values[1].trim(), 10)
    b = parseInt(values[2].trim(), 10)
  } else {
    // 无效的输入格式
    return ''
  }

  // 将RGB值转换为小数
  r = r / 255
  g = g / 255
  b = b / 255

  // 找到最大和最小的RGB值
  const max = Math.max(r, g, b)
  const min = Math.min(r, g, b)

  // 计算亮度
  const lightness = (max + min) / 2

  // 如果最大和最小值相等，则颜色为灰色，无法计算色相和饱和度
  if (max === min) {
    return `hsl(0, 0%, ${Math.round(lightness * 100)}%)`
  }

  // 计算饱和度
  const delta = max - min
  const saturation = lightness > 0.5 ? delta / (2 - max - min) : delta / (max + min)

  // 计算色相
  let hue = 0
  if (max === r) {
    hue = ((g - b) / delta + (g < b ? 6 : 0)) * 60
  } else if (max === g) {
    hue = ((b - r) / delta + 2) * 60
  } else if (max === b) {
    hue = ((r - g) / delta + 4) * 60
  }

  // 确保色相在0到360之间
  if (hue < 0) {
    hue += 360
  }

  // 返回HSL字符串
  return `hsl(${Math.round(hue)}, ${Math.round(saturation * 100)}%, ${Math.round(
    lightness * 100
  )}%)`
}
function generateColor(str) {
  let [h, s, l] = str.replace('hsl(', '').replace(')', '').split(',')
  console.log(h, s, l)
  if (
    !tinycolor(str).isValid() ||
    h.toString().includes('NaN') ||
    s.toString().includes('NaN') ||
    l.toString().includes('NaN')
  )
    return generateColor.history || []
  let numbers = [parseFloat(l)]
  for (
    let index = (parseFloat(l) * 1000 - 3.125 * 1000) / 1000;
    ;
    index = (index * 1000 - 3.125 * 1000) / 1000
  ) {
    if (index <= 0) {
      index = 0
      numbers.push(index)
      break
    }

    numbers.push(index)
  }
  for (
    let index = (parseFloat(l) * 1000 + 3.125 * 1000) / 1000;
    ;
    index = (index * 1000 + 3.125 * 1000) / 1000
  ) {
    if (index >= 100) {
      index = 100
      numbers.unshift(index)
      break
    }

    numbers.unshift(index)
  }
  if (numbers.length != 32) {
    numbers.splice(1, 1)
  }
  if (numbers.length != 32) {
    numbers.splice(numbers.length - 2, 1)
  }

  generateColor.history = numbers.map((i) => `hsl(${h},${s},${i}%)`)
  return numbers.map((i) => `hsl(${h},${s},${i}%)`)
}
function randomColor() {
  return '#' + (Math.random() * 256).toString(16).substring(4, 10)
}
function randomNum(minNum, maxNum) {
  switch (arguments.length) {
    case 1:
      return parseInt(Math.random() * minNum + 1, 10)
    case 2:
      return parseInt(Math.random() * (maxNum - minNum + 1) + minNum, 10)
    default:
      return 0
  }
}

function randomGradientColor() {
  //指定范围随机整数

  let colors = []
  for (let index = 0; index < randomNum(2, 5); index++) {
    colors.push(randomColor())
  }
  return `linear-gradient(${Math.random() * 360}deg,${colors.join(',')})`
}

function colorInput(e) {
  console.log(e)
  handleGenerateColor(e.target.value)
}

function getContrastColor(color) {
  if (color.startsWith('rgb')) {
    const rgb = color.slice(4, -1).split(',').map(Number)
    const r = rgb[0]
    const g = rgb[1]
    const b = rgb[2]
    const brightness = (r * 299 + g * 587 + b * 114) / 1000
    return brightness > 125 ? 'black' : 'white'
  } else if (color.startsWith('#')) {
    const hex = color.slice(1)
    const r = parseInt(hex.substr(0, 2), 16)
    const g = parseInt(hex.substr(2, 2), 16)
    const b = parseInt(hex.substr(4, 2), 16)
    const brightness = (r * 299 + g * 587 + b * 114) / 1000
    return brightness > 125 ? 'black' : 'white'
  } else if (color.startsWith('hsl')) {
    const values = color.substring(4, color.length - 1).split(',')
    const hue = parseInt(values[0].trim())
    const saturation = parseInt(values[1].trim().slice(0, -1)) / 100
    const lightness = parseInt(values[2].trim().slice(0, -1)) / 100
    const c = (1 - Math.abs(2 * lightness - 1)) * saturation
    const x = c * (1 - Math.abs(((hue / 60) % 2) - 1))
    const m = lightness - c / 2
    let r, g, b
    if (hue < 60) {
      r = c
      g = x
      b = 0
    } else if (hue < 120) {
      r = x
      g = c
      b = 0
    } else if (hue < 180) {
      r = 0
      g = c
      b = x
    } else if (hue < 240) {
      r = 0
      g = x
      b = c
    } else if (hue < 300) {
      r = x
      g = 0
      b = c
    } else {
      r = c
      g = 0
      b = x
    }
    r = Math.round((r + m) * 255)
    g = Math.round((g + m) * 255)
    b = Math.round((b + m) * 255)
    const brightness = (r * 299 + g * 587 + b * 114) / 1000
    return brightness > 125 ? 'black' : 'white'
  } else {
    return 'black'
  }
}
function copyColor(e) {
  console.log(e.target.dataset.bg)
  navigator.clipboard.writeText(
    `background-color: ${e.target.dataset.bg}; color: ${e.target.dataset.color};`
  )
  ElMessage.success('复制成功')
}
function copyColor2(e) {
  console.log(e.target.dataset.bg)
  navigator.clipboard.writeText(e.target.dataset.bg)
  ElMessage.success('复制成功')
}

function copyGradientColor() {
  navigator.clipboard.writeText('background-image:' + gradientColor.value + ';')
  ElMessage.success('复制成功')
}

function handleUploadClick(e) {
  console.log(e.target.parentNode.querySelector('input').click())
}

async function onfileInput(e) {
  console.log(e.target.files)
  let fileList = [...e.target.files].filter(
    (el) => el.type.includes('image') || el.type.includes('video')
  )
  console.log(files)
  let readFileAsDataUrl = (file) => {
    return new Promise((resolve, reject) => {
      let reader = new FileReader()
      reader.readAsDataURL(file)
      reader.onload = () => {
        resolve(reader.result)
      }
      reader.onerror = () => {
        reject(reader.error)
      }
    })
  }
  for (let file of fileList) {
    let dataUrl = await readFileAsDataUrl(file)
    let type = file.type.includes('image') ? 'image' : 'video'
    let tempFile = {
      type,
      url: dataUrl,
      file,
      size: file.size,
      time: +new Date(),
      status: 'uploading'
    }
    files.value.push(tempFile)
    setTimeout(() => {
      console.log('上传结果')
      tempFile.status = Math.random() > 0.5 ? 'success' : 'error'
      files.value.indexOf(tempFile) > -1 &&
        files.value.splice(files.value.indexOf(tempFile), 1, { ...tempFile })
    }, 2000)
  }
}

onMounted(() => {
  // let $$ = document.querySelectorAll.bind(document)
  // $$('*').forEach((el) => {
  //   el.addEventListener('click', (e) => {
  //     console.log(e.target.classList)
  //     e.stopPropagation()
  //   })
  // })
})
</script>
<style lang="less" scoped>
.w200px {
  width: 200px;
  height: fit-content;
  .text {
    transition: background-size 0.8s linear;
    background-image: linear-gradient(135deg, #ca1818, #ca1818, #ca1818);
    background-size: 0% 3px;
    padding-bottom: 3px;

    background-position: right bottom;
    background-repeat: no-repeat;
  }
  &:hover .text {
    background-position-x: left;
    background-size: 100% 3px;
  }
  section {
    .info {
      .name {
      }
    }
  }
}
.upload {
  width: 1000px;
  .item {
    width: 100%;
    height: 250px;

    border-radius: 10px;
    overflow: hidden;
    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      cursor: zoom-in;
    }
    video {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
    .uploading,
    .error {
      position: absolute;
      inset: 0;
      background-color: rgba(255, 255, 255, 0.582);
    }
  }
  .uploadBtn {
    border: 1px solid #cdd0d6;
    cursor: pointer;
  }
  .uploadBtn:hover {
    border: 1px solid #409eff;
  }
}
.colors {
  .item {
    cursor: pointer;
    box-sizing: border-box;
    .tips {
      transition: opacity 0.5s 0.5s;
      opacity: 0;
      left: 50%;
      top: -10px;
      transform: translate(-50%, -100%);
      padding: 10px;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 4em;
      height: fit-content;
      z-index: 4;

      background-color: rgb(0, 0, 0);
      border-radius: 8px;
      color: #fff;
      &::after {
        position: absolute;
        content: '';
        width: 0px;
        height: 0px;
        border-left: 10px solid transparent;
        border-right: 10px solid transparent;
        border-bottom: 10px solid transparent;
        border-top: 10px solid rgb(0, 0, 0);
        left: 50%;
        bottom: 0;
        transform: translate(-50%, 100%);
      }
    }
  }
}
.item:hover {
  border: #409eff 3px solid !important;
  z-index: 2;
  .tips {
    opacity: 1;
  }
}
div.item.current {
  border: 1px solid red;
  transform: scale(1.05);
  position: relative;
  z-index: 1;
}

.item + .item {
  border-top: 1px solid rgb(180, 180, 180);
}
& + & {
  border-left: 1px solid rgb(180, 180, 180);
}

.jbs {
}
</style>
