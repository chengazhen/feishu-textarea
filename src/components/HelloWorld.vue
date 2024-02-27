<script setup>
import { onMounted, ref } from 'vue';

const props = defineProps({
  msg: String
})

const div = ref()

onMounted(() => {
  setTimeout(() => {
    init()
  }, 1000);
})

function init() {
  window.MathJax.typesetPromise([div.value])
    .then((node) => {
      return node
    })
    .catch((err) => {
      console.log(err)
    })
}
// MathJax.typesetPromise(input).then(function (node) {
//   // console.log(node)
//   output.appendChild(node);
//   MathJax.startup.document.clear();
//   MathJax.startup.document.updateDocument();
// }).catch(function (err) {

//   output.appendChild(document.createElement('pre')).appendChild(document.createTextNode(err.message));
// }).then(function () {

//   // button.disabled = display.disabled = false;
// });
</script>

<template>
  <div ref="div">
    <div >
      {{ msg }}
    </div>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
