<script setup lang="ts">
import { ref } from 'vue'
import ClipBoardApi from './components/ClipBoardApi.vue'
//  @ts-ignore
import { VueToggles } from 'vue-toggles'
import ClipBoardExecComand from './components/ClipBoardExecComand.vue';

const example = ref(true)

</script>
<template>
  <main class="main">
    <VueToggles
      :value="example"
      :height="30"
      :width="300"
      checked-text="ExecCommand"
      unchecked-text="ClipBoardApi"
      @click="example = !example"
    />
    <ClipBoardExecComand v-if="example" />
    <ClipBoardApi v-else />
  </main>
</template>

<style scoped>
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
</style>
