<script setup lang="ts">
import { onMounted, ref } from 'vue'

const clipBoardValue = ref<HTMLInputElement | null>(null);

const permissionsToReadClipboardApi = ref(false);
const permissionsToWriteClipboardApi = ref(false);

const noPasteClipboardApi = ref('');
const noCopyClipboardApi = ref('');
const noInsertClipboardApi = ref('');

const successCopyClipboardApi = ref(false);
const successPasteClipboardApi = ref(false);
const successInsertClipboardApi = ref(false);
const copiedValue = ref('');

const tryToReadSetTimeOut = () => {
  setTimeout(tryToReadFromClipBoard, 1000);
}

const tryToReadFromClipBoard = () => {
  getPermissions();
  resetAllMessages();
  successPasteClipboardApi.value = false;
  noPasteClipboardApi.value = !navigator?.clipboard?.readText ? 'No support readText' : '';
  if(!noPasteClipboardApi.value) {
    navigator.clipboard.readText().then((copiedText) => {
    copiedValue.value = copiedText;
    successPasteClipboardApi.value = true;
   }).catch(error => {
    noCopyClipboardApi.value = error
   })
  }
}

const tryToCopyToClipBoard = () => {
  getPermissions();
  resetAllMessages();
  successCopyClipboardApi.value = false;
  noCopyClipboardApi.value = !navigator?.clipboard?.writeText ? 'No support writeText' : '';
  if(!noCopyClipboardApi.value) {
    navigator.clipboard.writeText(`${copiedValue.value} i love frontend`).then(() => {
    successPasteClipboardApi.value = true;
   }).catch(error => {
    noCopyClipboardApi.value = error
   })
  }
}

const resetAllMessages = () => {
  noPasteClipboardApi.value = '';
  noCopyClipboardApi.value = '';
  noInsertClipboardApi.value = '';
  successCopyClipboardApi.value = false;
  successPasteClipboardApi.value = false;
  successInsertClipboardApi.value = false;
}

const getPermissions = () => {
  // @ts-ignore
  navigator.permissions.query({ name: 'clipboard-read' }).then(res => {
    permissionsToReadClipboardApi.value = res?.state === 'granted' ?? false;
  });
  // @ts-ignore
  navigator.permissions.query({ name: 'clipboard-write' }).then(res => {
    permissionsToWriteClipboardApi.value = res?.state === 'granted' ?? false;
  });
}

const handleVisibility = () => {
  tryToReadFromClipBoard();
}

onMounted(() => {
  // setTimeout(tryToReadFromClipBoard, 5000);
  getPermissions();
  document.addEventListener('visibilitychange', handleVisibility)

})
</script>

<template>
  <div class="clipboard">
    <h3>CLIPBOARD API TEST</h3>
    <textarea
      id="clipBoardValue"
      v-model="copiedValue"
    />
    <button
      class="button"
      @click="tryToReadFromClipBoard"
    >
      Read (and paste)
    </button>
    <button
      class="button"
      @click="tryToCopyToClipBoard"
    >
      Copy
    </button>
    <button
      class="button"
      @click="tryToReadSetTimeOut"
    >
      Read setTimeOut
    </button>
    <article>
      <h3>Permissions</h3>
      <table>
        <tr>
          <td>clipboard-read</td>
          <td
            :class="{
              success:
                permissionsToReadClipboardApi,
              error:
                !permissionsToReadClipboardApi}"
          >
            {{ permissionsToReadClipboardApi }}
          </td>
        </tr>
        <tr>
          <td>clipboard-write</td>
          <td
            :class="{
              success:
                permissionsToWriteClipboardApi,
              error:
                !permissionsToWriteClipboardApi}"
          >
            {{ permissionsToWriteClipboardApi }}
          </td>
        </tr>
      </table>
    </article>
    <article class="messages-container">
      <section class="messages">
        <p
          v-if="noInsertClipboardApi"
          class="error"
        >
          {{ noInsertClipboardApi }}
        </p>
        <p
          v-if="noPasteClipboardApi"
          class="error"
        >
          {{ noPasteClipboardApi }}
        </p>
        <p
          v-if="noCopyClipboardApi"
          class="error"
        >
          {{ noCopyClipboardApi }}
        </p>
        <p
          v-if="successCopyClipboardApi"
          class="success"
        >
          Copied Text from textarea
        </p>

        <p
          v-if="successPasteClipboardApi"
          class="success"
        >
          Read success
        </p>

        <p
          v-if="successInsertClipboardApi"
          class="success"
        >
          Insert success
        </p>
      </section>
    </article>
  </div>
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
}
.clipboard {
  display: flex;
  flex-direction: column;
  width: 300px;
}
.error {
  color: red;
}
.success {
  color: green;
}
</style>
