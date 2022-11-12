<script setup lang="ts">
import { useCopyToClipboard } from '../utils'
const copyToClipboard = useCopyToClipboard()
const svg = $ref('')
let base64 = $ref('')
const svg2DataUrl = (svgStr: string): string => {
  svgStr = svgStr.replace(
    /\<svg/,
    '<?xml version="1.0" encoding="utf-8"?><svg ',
  ).replace(/ height="\d+"/g, ' height="100%"').replace(/ width="\d+"/g, ' width="100%"')
  const encoded: string = encodeURIComponent(svgStr)
    .replace(/'/g, '%27')
    .replace(/"/g, '%22')
  return `data:image/svg+xml,${encoded}`
}

const go = () => {
  if (svg)
    base64 = svg2DataUrl(svg)
}
const copy = () => {
  copyToClipboard(base64)
}
</script>

<template>
  <div>
    <div i-carbon-campsite text-4xl inline-block />
    <p>
      <a rel="noreferrer" href="https://github.com/antfu/vitesse-lite" target="_blank">
        Vitesse Lite
      </a>
    </p>
    <p>
      <em text-sm op75>Opinionated Vite Starter Template</em>
    </p>

    <div py-4 />
    <textarea
      id="textarea" v-model="svg" placeholder="svg code" type="text" autocomplete="false" p="x-4 y-2" w="650px"
      h="300px" text="center" bg="transparent" border="~ rounded gray-200 dark:gray-700" outline="none active:none"
      @keydown.enter="go"
    />

    <div>
      <button class="m-3 text-sm btn" :disabled="!svg" @click="go">
        Transform
      </button>
      <button class="m-3 text-sm btn" :disabled="!svg" @click="copy">
        Copy
      </button>
    </div>
    <textarea
      id="input" v-model="base64" placeholder="base64 code" disabled type="text" autocomplete="false"
      p="x-4 y-2" w="650px" h="300px" text="center" bg="transparent" border="~ rounded gray-200 dark:gray-700"
      outline="none active:none" @keydown.enter="go"
    />
  </div>
</template>
