<script setup lang="ts">
 import { ref } from "vue"

defineProps<{
  msg: string
}>()

const tryToReadFromClipBoard = () => {
  navigator.clipboard.readText().then((copiedText) => {
    console.log(copiedText); // copied text will be shown here.
    copiedValue.value=copiedText;
  });
}

const copiedValue = ref('');

</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <h3>
      CLIPBOARD API
    </h3>
    <button @click="tryToReadFromClipBoard">READ FROM CLIPBOARD!</button>
    <div>
      <textarea id='clipBoardValue' v-model="copiedValue"></textarea>
    </div>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
