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

  <div class="preview-wrapper">
    <preview ref="previewRef" :files="{
      html: htmlCode,
      css: cssCode,
      javascript: jsCode
    }"></preview>
  </div>
</template>

<script setup lang="ts">
import EditorContainer from '@/components/editor-container/index.vue'
import Preview from '@/components/preview/index.vue'
import { debounce } from '@/utils/index'

const htmlCode = ref('')
const cssCode = ref('')
const jsCode = ref('')
const previewRef = ref(null)

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
  previewRef.value.execute()
}

</script>

<style scoped>
.header-wrapper {
  display: flex;
  justify-content: flex-end;
  padding: 10px 20px;
  line-height: 1.5;
  background-color: #18212d;
}
.tool-item {
  margin-right: 10px;
  padding: 5px 15px;
  font-size: 14px;
  color: #fff;
  background-color: rgb(255 255 255 / 8%);
  cursor: pointer;
  border-radius: 5px;
}

.main-wrapper {
  display: flex;
  justify-content: flex-start;
  height: 700px;
  overflow-y: hidden;
}
.preview-wrapper {

}

</style>
