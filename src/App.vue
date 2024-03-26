<script setup lang="ts">
import { computed, ref } from "vue";

const mmsJsonRaw = ref("");
const body = computed(() => {
  if (!mmsJsonRaw.value) return;
  const mmsJson = JSON.parse(mmsJsonRaw.value);
  return {
    jwt: mmsJson.token,
    return_to: mmsJson.url,
  };
});
</script>

<template>
  <input v-model="mmsJsonRaw" />
  <p style="max-width: 80vw; overflow: auto;">jwt: {{ body?.jwt }}</p>
  <p>return_to: {{ body?.return_to }}</p>
  <button @click="">Click to move to emoldino zendesk</button>
</template>

<style scoped></style>
