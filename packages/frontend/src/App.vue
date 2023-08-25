<script setup lang="ts">
import monacoEditor from '@/components/monaco-editor/index.vue'
import { debounce } from '@/utils/index'

const htmlCode = ref('')
const cssCode = ref('')
const jsCode = ref('')

const onHtmlChange = debounce((code: string) => {
   htmlCode.value = code
}, 500)

const onCssChange = debounce((code: string) => {
   cssCode.value = code
}, 500)

const onChange = debounce((code: string) => {
   jsCode.value = code
}, 500)

const execute = () => {
  
}

</script>

<template>
  <header class="header-wrapper">
    <div class="tool-item" @click="execute">运行</div>
    <div class="tool-item">设置</div>
  </header>

  <main class="main-wrapper">
    <monaco-editor lang="html" @change="onHtmlChange"></monaco-editor>
    <monaco-editor lang="css" @change="onCssChange"></monaco-editor>
    <monaco-editor lang="javascript" @change="onChange"></monaco-editor>
  </main>

  <div class="preview-wrapper">preview</div>
</template>

<style scoped>
.header-wrapper {
  display: flex;
  justify-content: flex-end;
  padding-right: 20px;
  line-height: 1.5;
}
.tool-item {
  padding: 5px 10px;
  font-size: 14px;
  color: #666;
}

.main-wrapper {
  display: flex;
  justify-content: flex-start;
  height: 600px;
}
.preview-wrapper {

}

</style>
