<script setup lang="ts">
import monacoEditor from '@/components/monaco-editor/index.vue'
</script>

<template>
  <header>
    <div class="wrapper">
      toolbar
    </div>
  </header>

  <main class="main-wrapper">
    <monaco-editor lang="html"></monaco-editor>
    <monaco-editor lang="css"></monaco-editor>
    <monaco-editor lang="javascript"></monaco-editor>
  </main>
  <footer>
    <div class="preview-wrapper">preview</div>
  </footer>
</template>

<style scoped>
header {
  line-height: 1.5;
}
.main-wrapper {
  display: flex;
  justify-content: flex-start;
  height: 600px;
}
.preview-wrapper {
}

</style>
