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
const onClick = async () => {
  const res = await fetch("https://emoldinosupport.zendesk.com/access/jwt", {
    method: "POST",
    body: JSON.stringify(body.value),
    headers: {
      Accept: "*/*",
      "Allow-Control-Allow-Origin": "*",
    },
  });
  const data = await res.json();
  console.log(data);
};
</script>

<template>
  <input v-model="mmsJsonRaw" />
  <p style="max-width: 80vw; overflow: auto">jwt: {{ body?.jwt }}</p>
  <p>return_to: {{ body?.return_to }}</p>
  <button @click="onClick">Click to move to emoldino zendesk</button>
</template>

<style scoped></style>
