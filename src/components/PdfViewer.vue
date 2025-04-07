<template>
  <div style="max-width: 800px; margin: auto;">
    <h3>📄 PDF Viewer (No Download/Print)</h3>

    <!-- Input for PDF link -->
    <input
      v-model="inputUrl"
      type="text"
      placeholder="Enter PDF URL or upload a file"
      style="width: 100%; padding: 8px; margin-bottom: 10px;"
    />

    <!-- Upload input -->
    <input type="file" @change="onFileChange" accept="application/pdf" />

    <!-- Show iframe -->
    <div v-if="pdfSrc" style="margin-top: 20px;">
      <iframe
        :src="pdfSrc"
        width="100%"
        height="800"
        frameborder="0"
        title="PDF Viewer"
      ></iframe>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, onMounted, onBeforeUnmount } from 'vue';

const inputUrl = ref('');
const pdfSrc = ref('');

// Handle file input
function onFileChange(event) {
  const file = event.target.files[0];
  if (file && file.type === 'application/pdf') {
    const blobUrl = URL.createObjectURL(file);
    pdfSrc.value = `${blobUrl}#toolbar=0&navpanes=0&scrollbar=0`;
  } else {
    alert('Please upload a valid PDF file');
  }
}

// Watch URL input
watch(inputUrl, (newUrl) => {
  if (newUrl.endsWith('.pdf')) {
    pdfSrc.value = `${newUrl}#toolbar=0&navpanes=0&scrollbar=0`;
  }
});

// Disable right-click in this component
onMounted(() => {
  const preventContextMenu = (e) => e.preventDefault();
  window.addEventListener('contextmenu', preventContextMenu);

  // Clean up when component is unmounted
  onBeforeUnmount(() => {
    window.removeEventListener('contextmenu', preventContextMenu);
  });
});
</script>
