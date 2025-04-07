<template>
    <div v-if="videoId" :style="{ maxWidth: width + 'px', margin: 'auto' }">
      <iframe
        :width="width"
        :height="height"
        :src="`https://www.youtube.com/embed/${videoId}`"
        title="YouTube Video Player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
      ></iframe>
    </div>
    <p v-else>Invalid YouTube URL</p>
  </template>
  
  <script setup>
  import { computed } from 'vue';
  
  const props = defineProps({
    url: {
      type: String,
      required: true
    },
    width: {
      type: Number,
      default: 640
    },
    height: {
      type: Number,
      default: 360
    }
  });
  
  const videoId = computed(() => {
    const match = props.url.match(
      /(?:youtube\.com\/(?:watch\?v=|embed\/)|youtu\.be\/)([a-zA-Z0-9_-]{11})/
    );
    return match ? match[1] : null;
  });
  </script>
  