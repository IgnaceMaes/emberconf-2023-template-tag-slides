<script setup lang="ts">
const props = defineProps({
  class: {
    type: String,
  },
})
</script>

<template>
  <div class="slidev-layout two-columns w-full h-full grid grid-cols-2 gap-4">
    <div :class="props.class">
      <slot />
    </div>
    <div :class="props.class">
      <slot name="topright" />
    </div>
    <div :class="props.class">
      <slot name="bottomleft" />
    </div>
    <div :class="props.class">
      <slot name="bottomright" />
    </div>
  </div>
</template>