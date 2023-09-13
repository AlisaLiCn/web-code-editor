<template>
  <header class="header-wrapper">
    <div class="tool-item" @click="execute">运行</div>
    <div class="tool-item">设置</div>
  </header>
  
  <main class="main-wrapper">
    <editor-container lang="html" @change="onHtmlChange"></editor-container>
    <editor-container lang="css" @change="onCssChange"></editor-container>
    <editor-container lang="javascript" @change="onChange"></editor-container>
  </main>

  <div class="preview-wrapper">preview</div>
</template>

<script setup lang="ts">
import EditorContainer from '@/components/editor-container/index.vue'
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
