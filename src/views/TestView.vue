<template>
  <div>
    <button @click="click">Add</button>
    <ul style="height: 300px; overflow-y: auto" ref="ulRef">
      <li v-for="item in list" :key="item.id">{{ item.text }}</li>
      <!-- {{
        text
      }} -->
    </ul>
  </div>
</template>
<script lang="ts" setup>
import { ref, onMounted } from 'vue'

const ulRef = ref()
const text = ref('')
const list = ref<any>([])

onMounted(() => {
  startDOMObserver(ulRef.value)
  console.log('onMounted...' + new Date().getTime())
  for (let i = 0; i < 50; i++) {
    list.value.push({
      text: 'Item-' + i
    })
  }
})

const click = () => {
  // list.value.push({
  //   text: 'Item-'
  // })
  list.value.shift();
}

const startDOMObserver = (el: Node) => {
  const observer = new MutationObserver((mutationsList) => {
    for (const mutation of mutationsList) {
      console.log(mutation.type)
    }
  })

  // 监视根元素的子节点变化
  observer.observe(el, { childList: true, subtree: true })
}
</script>
