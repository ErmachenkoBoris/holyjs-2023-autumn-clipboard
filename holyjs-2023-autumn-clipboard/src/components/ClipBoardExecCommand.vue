<script setup lang="ts">
import { TIME_DELAY, COPY_WRITE_SET_TIMEOUT, PASTE_READ_SET_TIMEOUT, COPY_WRITE, PASTE_READ,PASTE_SUCCESS, COPY_WRITE_SUCCESS, COPY_WRITE_INFO, PASTE_READ_INFO  } from '@/consts/consts';
import { ref } from 'vue'

const clipBoardValue = ref<HTMLInputElement | null>(null);


const errorMessage = ref(false);
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
      if(!successPasteCommand.value) {
        errorMessage.value = true;
      }
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
      if(!successCopyCommand.value) {
        errorMessage.value = true;
      }
    }
    return;
  }
}

const tryToReadFromClipBoardSetTimeOut = () => {
  resetAllMessages();
  setTimeout(tryToReadFromClipBoard, TIME_DELAY);
}

const tryToCopyToClipBoardSetTimeOut = () => {
  resetAllMessages();
  setTimeout(tryToCopyToClipBoard, TIME_DELAY);
}

const tryToInsertText = () => {
  resetAllMessages();
  noInsertCommand.value = !document.queryCommandSupported('insertText');
  if(!noInsertCommand.value) {
    if(clipBoardValue.value) {
      clipBoardValue.value.select();
      successInsertCommand.value = document.execCommand("insertText", false, 'I love holy js');
      if(!successInsertCommand.value) {
        errorMessage.value = true;
      }
    }
    return;
  }
}


const resetAllMessages = () => {
  errorMessage.value = false;
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
    <p class="value">
      {{ PASTE_READ }}
    </p>
    <p class="info">
      {{ PASTE_READ_INFO }}
    </p>
  </button>
  <button
    class="button"
    @click="tryToCopyToClipBoard"
  >
    <p class="value">
      {{ COPY_WRITE }}
    </p>
    <p class="info">
      {{ COPY_WRITE_INFO }}
    </p>
  </button>
  <button
    class="button"
    @click="tryToReadFromClipBoardSetTimeOut"
  >
    <p class="value">
      {{ PASTE_READ_SET_TIMEOUT }}
    </p>
    <p class="info">
      {{ PASTE_READ_INFO }}
    </p>
  </button>
  <button
    class="button"
    @click="tryToCopyToClipBoardSetTimeOut"
  >
    <p class="value">
      {{ COPY_WRITE_SET_TIMEOUT }}
    </p>
    <p class="info">
      {{ COPY_WRITE_INFO }}
    </p>
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
        v-if="errorMessage"
        class="error"
      >
        Error
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
        {{ COPY_WRITE_SUCCESS }}
      </p>

      <p
        v-if="successPasteCommand"
        class="success"
      >
        {{ PASTE_SUCCESS }}
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
    font-size: 14px;
    cursor: pointer;
    min-height: 40px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-radius: 4px;
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

.info {
  color: rgb(63, 62, 62)
}
</style>
