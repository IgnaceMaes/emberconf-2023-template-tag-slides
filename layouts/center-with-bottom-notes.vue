<template>
  <div class="slidev-layout center h-full grid place-content-center statement">
    <div class="my-auto">
      <slot />
    </div>
    <div class="absolute bottom-0 grid grid-cols-2 gap-4 p-6 w-full">
      <div class="text-left">
        <slot name="right" />
      </div>
      <div class="text-right">
        <slot name="left" />
      </div>
    </div>
  </div>
</template>