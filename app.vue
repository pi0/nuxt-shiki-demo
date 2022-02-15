<template>
  <main>
    <div contenteditable="true" id="code" v-html="hlCode"></div>
    Code is highlighted using <a href="https://github.com/unjs/shiki-es">unjs/shiki-es</a>
  </main>
</template>

<script lang="ts" setup>
const code = ref(`
import { getHighlighter } from 'shiki-es'

const highlighter = await getHighlighter({ theme: 'nord' })

console.log(highlighter.codeToHtml('console.log("Hello shiki!");', { lang: 'ts' }))
`.trim())

onMounted(() => {
  let lastValue
  document.getElementById("code").addEventListener("input", (ev) => {
    lastValue = (ev.target as any).innerText
    setTimeout(() => code.value = lastValue, 1000)
}, false);
})

const hlCode = await useShiki(code, { lang: 'ts' })
</script>

<style>
body {
    margin: 2em;
    background-color: #2e3440ff;
}
body, a {
  color: white;
}
#code {
  padding: 1em;
  margin: 2em;
  border: 1px solid white;
}
</style>
