<script setup lang="ts">
import { ref } from 'vue'

const clipBoardValue = ref<HTMLInputElement | null>(null);

const noPasteCommand = ref(false);
const noCopyCommand = ref(false);
const noInsertCommand = ref(false);

const successCopyCommand = ref(false);
const successPasteCommand = ref(false);
const successInsertCommand = ref(false);
const copiedValue = ref('');

const tryToReadFromClipBoard = () => {
  resetAllMessages();
  successPasteCommand.value = false;
  noPasteCommand.value = !document.queryCommandSupported('paste');
  if(!noPasteCommand.value) {
    if(clipBoardValue.value) {
      clipBoardValue.value.focus();
      successPasteCommand.value = document.execCommand("paste");
    }
    return;
  }
}

const tryToCopyToClipBoard = () => {
  resetAllMessages();
  successCopyCommand.value = false;
  noCopyCommand.value = !document.queryCommandSupported('copy');
  if(!noCopyCommand.value) {
    if(clipBoardValue.value) {
      clipBoardValue.value.select();
      successCopyCommand.value = document.execCommand("copy");
    }
    return;
  }
}

const tryToInsertText = () => {
  resetAllMessages();
  noInsertCommand.value = !document.queryCommandSupported('insertText');
  if(!noInsertCommand.value) {
    if(clipBoardValue.value) {
      clipBoardValue.value.select();
      successInsertCommand.value = document.execCommand("insertText", false, 'I love holy js');
    }
    return;
  }
}


const resetAllMessages = () => {
  noPasteCommand.value = false;
  noCopyCommand.value = false;
  noInsertCommand.value = false;
  successCopyCommand.value = false;
  successPasteCommand.value = false;
  successInsertCommand.value = false;
}
</script>

<template>
  <h3>Document: execCommand</h3>
  <textarea
    ref="clipBoardValue"
    v-model="copiedValue"
    class="clipboard-value"
  />
  <button
    class="button"
    @click="tryToReadFromClipBoard"
  >
    Paste (Read)
  </button>
  <button
    class="button"
    @click="tryToCopyToClipBoard"
  >
    Copy (Write)
  </button>
  <button
    class="button"
    @click="tryToInsertText"
  >
    Insert
  </button>
  <article class="messages-container">
    <section class="messages">
      <p
        v-if="noInsertCommand"
        class="error"
      >
        Not support Paste Command
      </p>
      <p
        v-if="noPasteCommand"
        class="error"
      >
        Not support Paste Command
      </p>
      <p
        v-if="noCopyCommand"
        class="error"
      >
        Not support Copy Command
      </p>
      <p
        v-if="successCopyCommand"
        class="success"
      >
        Copied Text from textarea! try to past as usual
      </p>

      <p
        v-if="successPasteCommand"
        class="success"
      >
        Paste success!
      </p>

      <p
        v-if="successInsertCommand"
        class="success"
      >
        Insert success!
      </p>
    </section>
  </article>
</template>

<style scoped>


.messages-container {
  position: relative;
}

.messages {
  position: absolute;

}
.button {
  cursor: pointer;
  min-height: 30px;
}
.error {
  color: red;
}
.success {
  color: green;
}
.clipboard-value {
  min-height: 100px;
}
</style>
