<template>
  <div id="app">
    <!-- Overlay when user switches tab -->
    <div v-if="isHidden" class="overlay">
      <h2>⚠️ Tab inactive. Viewing disabled.</h2>
    </div>

    <!-- Actual app content -->
    <div v-else>
      <h2>🎬 Embedded YouTube Video</h2>
      <YouTubeEmbed url="https://www.youtube.com/watch?v=0c65c2rDAo0" />

      <h2>📄 PDF Viewer (Upload or Link)</h2>
      <PdfViewer />

      <h2>🧠 MCQ Quiz</h2>
      <McqQuestion />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import YouTubeEmbed from './components/YouTubeEmbed.vue'
import PdfViewer from './components/PdfViewer.vue'
import McqQuestion from './components/McqQuestion.vue'

const isHidden = ref(false)

function handleVisibilityChange() {
  isHidden.value = document.hidden
}

onMounted(() => {
  document.addEventListener('visibilitychange', handleVisibilityChange)
})

onBeforeUnmount(() => {
  document.removeEventListener('visibilitychange', handleVisibilityChange)
})
</script>

<style scoped>
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.95);
  color: white;
  z-index: 9999;
  text-align: center;
  padding-top: 200px;
  font-size: 24px;
}
</style>
