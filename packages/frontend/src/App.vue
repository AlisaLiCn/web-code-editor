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
    <monaco-editor></monaco-editor>
    <div class="preview-wrapper">preview</div>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}
.main-wrapper {
  display: flex;
  justify-content: flex-start;
  height: 100vh;
}
.preview-wrapper {
  width: 50%;
}

@media (min-width: 1024px) {
  /* header { */
    /* display: flex; */
    /* place-items: center; */
    /* padding-right: calc(var(--section-gap) / 2); */
  /* } */
}
</style>
