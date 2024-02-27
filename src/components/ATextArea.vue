<template>
    <ul class="w-full overflow-hidden">
      <li  v-for="(item, index) in list">{{ index }} {{ item   }}</li>
    </ul>
    <textarea class="copy-textarea fixed top-0 init-textarea overflow-x-auto whitespace-nowrap w-auto hide-scrollbar invisible" :value="textContent" :style="{ width: textareaInitWidth }" />
    <textarea ref="copyTextarea" :value="textContent" :style="{ width: textareaWrapperWidth }" class="invisible init-textarea copy-height-textarea hide-scrollbar" />
  <div class="">
    <div class="bg-white rounded-8px px-5 py-12px textarea-wrapper" :class="isFlex ? 'flex' : ''">
      <!-- 左边输入框 -->
      <div class="flex-1 max-h-200px overflow-y-auto flex items-center">
        <textarea ref="textareaRef" :value="textContent" placeholder="请输入新消息"
          class="init-textarea real-textarea placeholder-#C1C2C6" cols="1000" :style="{ height: textareaHeight }"
          @input="textareaInput" @dragenter.prevent @dragover.prevent
          @keyup.enter="sendTextMsg" />
      </div>
      <div class="flex items-center justify-end" :class="{ 'mt-1': !isFlex }">
        <!-- <CollectAudio class="cursor-pointer" @send-audio-messages="sendAudioMessages" />
        <img class="w-18px h-18px mx-18px cursor-pointer" src="../../assets/images/upload-img-icon.png" alt=""
          @click="uploadImgsClick"> -->
        <!-- <img class="w-18px h-18px mr-18px cursor-pointer" src="../../assets/images/upload-video-icon.png" alt="" @click="uploadVideo"> -->
        <div class="bg-#A3B4C9 rounded-16px h-32px leading-8 text-center w-64px text-14px text-white"
          :style="{ 'background-color': textContent ? '#18457A' : '#A3B4C9', 'cursor': textContent ? 'pointer' : 'not-allowed' }"
          @click="sendTextMsg">
          发送
        </div>
        <div ref="loadingBox" class="absolute right-0 top-0 w-6 h-6" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue'

//  文本输入框
const textareaRef = ref<HTMLTextAreaElement>()
const initTextareaHeight = ref(0)
const textareaHeight = ref('')
const copyTextarea = ref<HTMLDivElement>()
const copyTextareaInitHeight = ref(0)
const copyInitWidth = ref(0)
const isFlex = ref(true)
const textareaInitWidth = ref('')
const textareaWrapperWidth = ref('')
onMounted(() => {
  initTextarea()
  // 记住最初的文本输入框宽度
})

function initTextarea() {
  if (!textareaRef.value) {
    return
  }
  initTextareaHeight.value = textareaRef.value.clientHeight
  copyTextareaInitHeight.value = textareaRef.value.clientHeight
  copyInitWidth.value = textareaRef.value.clientWidth

  textareaInitWidth.value = `${textareaRef.value.clientWidth}px`
  const textareaWrapper = document.querySelector('.textarea-wrapper');
  if (textareaWrapper) {
    const style = window.getComputedStyle(textareaWrapper);
    textareaWrapperWidth.value = `${textareaWrapper.clientWidth - parseFloat(style.paddingLeft) - parseFloat(style.paddingRight)}px`;
  }
}

const textContent = ref<string>('')
function textareaInput(event: Event) {
  // @ts-expect-error xxx
  textContent.value = event.target.value || ''
  if (!textareaRef.value) {
    return
  }

  // @ts-expect-error xxx
  if (!event.target?.value) {
    textareaHeight.value = `${initTextareaHeight.value}px`
    isFlex.value = true
    return
  }

  // 计算内容宽度
  nextTick(() => {
    compareWidth()
  })

  if (isFlex.value) {
    return
  }

  nextTick(() => {
    calcHeight()
  })

  function calcHeight() {
    const copyHeight = copyTextarea.value?.scrollHeight || 0
    if (copyHeight === 0) {
      textareaHeight.value = `${initTextareaHeight.value}px`
    } else {
      textareaHeight.value = `${copyHeight}px`
    }
  }

  // 对比内容宽度是否大于初始宽度
  function compareWidth() {
    console.log(document.querySelector('.copy-textarea')?.scrollWidth);
    
    const totalContentWidth = document.querySelector('.copy-textarea')?.scrollWidth || 0
    if (totalContentWidth > copyInitWidth.value) {
      isFlex.value = false
    } else {
      isFlex.value = true
    }
  }
}

const list = ref<string[]>([])
function sendTextMsg() {
  list.value = [...list.value, textContent.value]
  textContent.value = ''
  isFlex.value = true
  textareaHeight.value = `${initTextareaHeight.value}px`
}
</script>


<style lang="scss" scoped>
* {
  box-sizing: border-box;
}
.editDiv {
 &::-webkit-scrollbar {
  display: none;
 }
}

.init-textarea {
  line-height: 22px;
  font-size: 14px;
  outline: none;
  resize: none;
  font-weight: 400;
  word-break: break-all;
  height: 22px;
}

.real-textarea {
  width: 100%;
  resize: none;
  border: 0;
  outline: none;
  overflow: hidden;
  height: 22px;
}

.copy-height-textarea {
  position: fixed;
  top: 30px;
  left: 0;
}

.hide-scrollbar {
  border: none;
  scrollbar-width: none;
  -ms-overflow-style: none;
  &::-webkit-scrollbar {
    display: none;
  }
}
</style>
