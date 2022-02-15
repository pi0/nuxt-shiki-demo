<template>
  <main>
    <div contenteditable="true" id="code" v-html="hlCode"></div>
    Code is highlighted using <a href="https://github.com/unjs/shiki-es">unjs/shiki-es</a>
  </main>
</template>

<script lang="ts" setup>
const code = ref(`
import { readFile } from 'fs/promises';
import { WASI } from 'wasi';
import { argv, env } from 'process';

// Some WASI binaries require:
//   const importObject = { wasi_unstable: wasi.wasiImport };
const importObject = { wasi_snapshot_preview1: wasi.wasiImport };

const wasm = await WebAssembly.compile(
  await readFile(new URL('./demo.wasm', import.meta.url))
);
const instance = await WebAssembly.instantiate(wasm, importObject);

wasi.start(instance);
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
