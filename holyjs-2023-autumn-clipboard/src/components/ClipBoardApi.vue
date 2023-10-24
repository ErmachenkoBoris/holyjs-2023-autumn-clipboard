<script setup lang="ts">
import { ref } from 'vue'

const tryToReadFromClipBoard = () => {
  navigator.clipboard.readText().then((copiedText) => {
    copiedValue.value = copiedText
  })
}

const copiedValue = ref('')
</script>

<template>
  <div class="clipboard">
    <h3>CLIPBOARD API TEST</h3>
    <button @click="tryToReadFromClipBoard">
      READ FROM CLIPBOARD!
    </button>
    <textarea
      id="clipBoardValue"
      v-model="copiedValue"
    />
  </div>
</template>

<style scoped>
.clipboard {
  display: flex;
  flex-direction: column;
  width: 300px;
}
</style>
