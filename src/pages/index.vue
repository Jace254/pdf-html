<script setup lang="ts" generic="T extends any, O extends any">
import { useFileDialog } from '@vueuse/core'

defineOptions({
  name: 'IndexPage',
})

const pickedPdf = ref<boolean>()

const { files, open, reset } = useFileDialog({
  accept: 'application/pdf',
  directory: false,
  multiple: false,
})
function clearPdf() {
  reset()
}

watch(files, (f) => {
  if (f)
    pickedPdf.value = true
  else
    pickedPdf.value = false
})
</script>

<template>
  <div class="h-full w-full flex flex-col items-center justify-center gap-4">
    <div v-if="!pickedPdf" class="h-200px w-300px flex flex-col cursor-pointer items-center justify-center border-2 border-border rounded-lg border-dashed p2 text-gray" @click="open">
      <span>Drag and Drop a PDF</span>
      <i>---- OR ----</i>
      <span>Click to Pick a PDF</span>
    </div>
    <div v-else class="relative h-200px w-300px flex flex-col items-center justify-center border border-border rounded-lg bg-active p2 py-7 text-gray">
      <button class="absolute right--2.5 top--2.5 h-25px w-25px flex items-center justify-center border border-border rounded-full bg-gray:50 transition-all hover:bg-white" @click="clearPdf">
        <div class="font-sm i-ic-round-close text-gray-800:40" />
      </button>
      <div class="flex items-center gap-2">
        <div i-carbon-document-pdf text-xl text-red:80 />
        <span class="text-sm text-foreground">{{ files[0].name }}</span>
      </div>
    </div>
    <button class="w-300px rounded-md bg-primary p2 text-white disabled:bg-primary:20 disabled:text-faded" :disabled="!pickedPdf">
      Next
    </button>
  </div>
</template>
