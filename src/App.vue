<script setup>
import HelloWorld from './components/HelloWorld.vue'
import ATextArea from './components/ATextArea.vue';

const list = ['$$l \perp \beta ,l \subset \alpha \Rightarrow \alpha \perp \beta$$', '$$l \perp \beta ,l \subset \alpha \Rightarrow \alpha \perp \beta$$', '$$l \perp \beta ,l \subset \alpha \Rightarrow \alpha \perp \beta$$']
</script>

<template>
   <h1>
    飞书输入
  </h1>
  <ATextArea />
  <h1>
    mathjax
  </h1>
  <HelloWorld :msg="item" v-for="item in list" :key="item" />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
