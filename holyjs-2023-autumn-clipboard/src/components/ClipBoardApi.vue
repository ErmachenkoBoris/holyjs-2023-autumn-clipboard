<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { TIME_DELAY, COPY_WRITE_SET_TIMEOUT, PASTE_READ_SET_TIMEOUT, COPY_WRITE, PASTE_READ, PASTE_SUCCESS, COPY_WRITE_SUCCESS, COPY_WRITE_INFO, PASTE_READ_INFO} from '@/consts/consts';
const permissionsToReadClipboardApi = ref(false);
const permissionsToWriteClipboardApi = ref(false);


const permissionState = ref('');
const noPasteClipboardApi = ref('');
const noCopyClipboardApi = ref('');
const noInsertClipboardApi = ref('');

const successCopyClipboardApi = ref(false);
const successPasteClipboardApi = ref(false);
const copiedValue = ref('');

const tryToReadSetTimeOut = () => {
  resetAllMessages();
  setTimeout(tryToReadFromClipBoard, TIME_DELAY);
}

const tryToWriteSetTimeOut = () => {
  resetAllMessages();
  setTimeout(tryToCopyToClipBoard, TIME_DELAY);
}

const tryToReadFromClipBoard = () => {
  getPermissions();
  resetAllMessages();
  successPasteClipboardApi.value = false;
  noPasteClipboardApi.value = !navigator?.clipboard?.readText ? 'No support readText' : '';
  if(!noPasteClipboardApi.value) {
    navigator.clipboard.readText().then((copiedText) => {
      getPermissions();
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
    navigator.clipboard.writeText(copiedValue.value).then(() => {
      successCopyClipboardApi.value = true;
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
}

const getPermissions = () => {
  // @ts-ignore
  navigator?.permissions?.query({ name: 'clipboard-read' }).then(res => {
    permissionsToReadClipboardApi.value = res?.state === 'granted' ?? false;
  });
  // @ts-ignore
  navigator?.permissions?.query({ name: 'clipboard-write' }).then(res => {
    permissionsToWriteClipboardApi.value = res?.state === 'granted' ?? false;
  });
}

onMounted(() => {
  getPermissions();
})

</script>

<template>
  <h3>Clipboard API</h3>
  <textarea
    id="clipBoardValue"
    v-model="copiedValue"
    class="clipboard-value"
  />
  <button
    class="button"
    @click="tryToReadFromClipBoard"
  >
    <p class="value">
      {{ PASTE_READ }}
    </p><p class="info">
      {{ PASTE_READ_INFO }}
    </p>
  </button>
  <button
    class="button"
    @click="tryToCopyToClipBoard"
  >
    <p class="value">
      {{ COPY_WRITE }}
    </p><p class="info">
      {{ COPY_WRITE_INFO }}
    </p>
  </button>
  <button
    class="button"
    @click="tryToReadSetTimeOut"
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
    @click="tryToWriteSetTimeOut"
  >
    <p class="value">
      {{ COPY_WRITE_SET_TIMEOUT }}
    </p>
    <p class="info">
      {{ COPY_WRITE_INFO }}
    </p>
  </button>
  <article class="permissions">
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
      {{ permissionState }}
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
        {{ COPY_WRITE_SUCCESS }}
      </p>

      <p
        v-if="successPasteClipboardApi"
        class="success"
      >
        {{ PASTE_SUCCESS }}
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
.permissions {
  margin-top: 10px;
}
.info {
  color: rgb(63, 62, 62)
}
</style>
