<template>
  <div class="editor-container">
    <div class="editor-lang">{{  props.lang }}</div>
    <monaco-editor lang="html" @change="onChange"></monaco-editor>
  </div>
</template>
  
<script setup lang="ts">
import monacoEditor from '@/components/monaco-editor/index.vue'
import { debounce } from '@/utils/index'

const props = withDefaults(
  defineProps<{
    lang: string
  }>(),
  {
    lang: 'javascript'
  }
)

const emit = defineEmits<{
  (e: 'change', value: string): void
}>()

const onChange = debounce((code: string) => {
  emit('change', code)
}, 500)

</script>

<style scoped>
.editor-container {
  width: 100%;
  height: 100%;
}
.editor-lang {
  padding: 5px 10px;
  color: #f2f2f2;
  background-color: #3c3c3c;
}
</style>
  