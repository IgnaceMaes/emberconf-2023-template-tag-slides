<script setup lang="ts">
import { ref } from 'vue'

const isCopied = ref(false);
function copyToClipboard() {
  isCopied.value = true;
}
</script>

<template>
  <button @click="copyToClipboard" class="text-gray-900 bg-white border border-gray-300 focus:outline-none hover:bg-gray-100 focus:ring-4 focus:ring-gray-200 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2">
    <span v-if="isCopied">Copied!</span>
    <span v-else>Click to copy</span>
    <mdi-clipboard class="ml-2"/>
  </button>
</template>
