<template>
  <div class="editor" ref="containerRef"></div>
</template>

<script setup lang="ts">
import * as monaco from 'monaco-editor'
import { initMonacoEnv } from './utils'

const props = withDefaults(
    defineProps<{
      value?: string
      readonly?: boolean
      lang?: string
    }>(),
    {
      readonly: false,
      lang: 'javascript'
    }
)

const emit = defineEmits<{
  (e: 'change', value: string): void
}>()

const containerRef = ref<HTMLDivElement>()
const editor = shallowRef<monaco.editor.IStandaloneCodeEditor>()

initMonacoEnv()

onMounted(async () => {
  await nextTick()

  if(!containerRef.value) {
    throw new Error('Cannot find containerRef')
  }

  const editorInstance = monaco.editor.create(containerRef.value, {
    // ...(props.readonly
    //   ? { value: props.value, language: lang.value }
    //   : { model: null }),
    value: props.value,
    language: props.lang,
    fontSize: 13,
    theme: 'vs-dark',
    readOnly: props.readonly,
    automaticLayout: true,
    scrollBeyondLastLine: false,
    minimap: {
      enabled: false,
    },
    inlineSuggest: {
      enabled: false,
    },
    'semanticHighlighting.enabled': true,
    fixedOverflowWidgets: true,
  })
  editor.value = editorInstance

  editorInstance.onDidChangeModelContent(() => {
    emit('change', editorInstance.getValue())
  })
})


onBeforeUnmount(() => {
  editor.value?.dispose()
})


</script>

<style scoped>
.editor {
  position: relative;
  height: 100%;
  width: 100%;
  overflow: hidden;
}
</style>
