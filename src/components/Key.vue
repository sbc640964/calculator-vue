<template>
  <div class="key" :style="{width: `${width}%`}">
    <div class="shadow-md hover:bg-gradient-to-bl from-gray-700 via-gray-800 to-gray-800 hover:shadow-inner hover:shadow transition-all cursor-pointer h-full flex justify-center items-center">
      <slot></slot>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Key',
  props: ['width'],
  mounted() {
    console.log(this.width)
  }
}
</script>

<style scoped>
.key {
  @apply p-1;
  @apply h-full;
}

.key > div {
  @apply rounded flex justify-center items-center bg-gray-800;
}
</style>