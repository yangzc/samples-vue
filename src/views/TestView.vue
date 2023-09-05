<template>
  <div>
    <ul style="height: 300px; overflow-y: auto" ref="ulRef">
      <li v-for="item in list" :key="item">{{ item }}</li>
    </ul>
  </div>
</template>
<script lang="ts" setup>
import { ref, onMounted } from 'vue'

const ulRef = ref()
const list = ref<string[]>([])

onMounted(() => {
  startDOMObserver(ulRef.value)

  console.log('onMounted...' + new Date().getTime())

  // list.value.push('First Item')
  // console.log("onMounted...1.." + new Date().getTime());

  // setTimeout(() => {
  //   console.log("setTimeout..." + new Date().getTime());
  // }, 0);
  // for (let i = 0; i < 50; i++) {
  //   list.value.push('Item-' + i.toString())
  // }
  // console.log("onMounted...2.." + new Date().getTime());

  setTimeout(() => {
    list.value.push('First Item')
    console.log('setTimeout2...' + new Date().getTime())

    setTimeout(() => {
      list.value.push('First Item11')
      console.log('setTimeout3...' + new Date().getTime())
    }, 0)

    new Promise((resolve) => {
      console.log('Promise...')
      resolve(1)
    }).then(() => {
      console.log('Promise then...')
    })
  }, 0)
})

const startDOMObserver = (el: Node) => {
  const observer = new MutationObserver((mutationsList) => {
    for (const mutation of mutationsList) {
      if (mutation.type === 'childList') {
        console.log('DOM 已更新')
      }
    }
  })

  // 监视根元素的子节点变化
  observer.observe(el, { childList: true, subtree: true })
}
</script>
