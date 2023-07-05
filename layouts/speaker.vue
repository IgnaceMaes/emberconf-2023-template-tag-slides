<script setup lang="ts">
const props = defineProps({
  image: {
    type: String,
  },
})
</script>

<template>
  <div class="slidev-layout two-columns w-full h-full grid grid-cols-2 gap-4 place-items-center">
    <div>
      <img :src="props.image" class="rounded-full w-80 h-80" />
    </div>
    <div>
      <slot />
    </div>
  </div>
</template>