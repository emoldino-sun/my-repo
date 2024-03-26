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
  });
  const data = await res.json();
  console.log(data);
};

const dynamicRequest = () => {
  if (!body.value?.jwt || !body.value?.return_to) return;

  const form = document.createElement("form");
  form.target = "ZendeskSSOPopup";
  form.method = "POST";
  form.action = "https://emoldinosupport.zendesk.com/access/jwt";
  form.style.display = "none";

  const jwtField = document.createElement("input");
  jwtField.type = "hidden";
  jwtField.name = "jwt";
  jwtField.value = body.value.jwt;
  form.appendChild(jwtField);

  const returnUrlField = document.createElement("input");
  returnUrlField.type = "hidden";
  returnUrlField.name = "return_to";
  returnUrlField.value = body.value.return_to;
  form.appendChild(returnUrlField);

  const windowFeatures =
    "width=600,height=700,resizable=yes,scrollbars=yes,status=yes";
  const popup = window.open("", "ZendeskSSOPopup", windowFeatures);
  if (!popup) {
    alert("팝업 창을 열 수 없습니다. 팝업 창 차단을 해제해주세요.");
    return;
  }

  document.body.appendChild(form);
  form.submit();

  document.body.removeChild(form);

  const checkPopupClosed = setInterval(() => {
    if (popup.closed) {
      clearInterval(checkPopupClosed);
      console.log("Popup has been closed");
    }
  }, 1000);
};
</script>

<template>
  <input v-model="mmsJsonRaw" />
  <p style="max-width: 80vw; overflow: auto">jwt: {{ body?.jwt }}</p>
  <p>return_to: {{ body?.return_to }}</p>
  <button @click="onClick">Click to move to emoldino zendesk</button>
  <br>
  <button @click="dynamicRequest">Dynamic request for zendesk</button>
</template>

<style scoped></style>
