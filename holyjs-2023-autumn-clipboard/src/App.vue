<script setup lang="ts">
import { ref } from 'vue'
import ClipBoardApi from './components/ClipBoardApi.vue'
import ClipBoardExecCommand from './components/ClipBoardExecCommand.vue';

const execCommand = ref(true)

</script>
<template>
  <main class="main">
    <section class="toggle">
      <div class="option">
        <input
          id="execCommandRadio"
          v-model="execCommand"
          type="radio"
          name="clipboardRadio"
          value="true"
          class="radio-option"
        >
        <label for="execCommandRadio">Document: execCommand</label>
      </div>
      <div class="option">
        <input
          id="clipboardApiRadio"
          v-model="execCommand"
          type="radio"
          name="clipboardRadio"
          class="radio-option"
          value=""
        >
        <label for="clipboardApiRadio">Clipboard API</label>
      </div>
    </section>
    <div class="clipboard">
      <ClipBoardExecCommand v-if="execCommand" />
      <ClipBoardApi v-else />
    </div>
  </main>
</template>

<style scoped>
.clipboard {
  display: flex;
  flex-direction: column;
  width: 360px;
  min-height: 400px;
  margin-top: 10px;
}

header {
  line-height: 1.5;
}
.main {
  height: 100vh;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
.radio-option {
  margin-right: 20px;
}
.option {
  display: block;
}
.toggle {
  min-width: 360px
}
label {
  font-weight: 500;
  font-size: 18px;
}
</style>
