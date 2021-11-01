<template>
  <div class="text-white flex font-semibold" :class="[height ?? 'h-[18.18182%]', m === 'r' ? '-mr-1' : (m === 'l' ? '-ml-1' : '-mx-1') ]">
    <Key
        v-for="key in keys"
        :key="key"
        :width="100/keys.length"
        @click="$emit('key', key)"
    >
      {{ key }}
    </Key>
  </div>
</template>

<script>

import Key from "./Key.vue";

export default {
  name: 'RowKeys',
  components: {Key},
  props: ['keys', 'height', 'm', 'key'],
  emits: [ 'key' ],
  methods: {
    styleHeight() {
      switch (this?.height){
        case '1/2' :
          return '50%';
        case 'full' :
      }
      return this?.height === 'big' ? '32%' : (this?.height === 'small' ? '9.09091%' : '18.18182%')
    }
  },
}
</script>
<style scoped>
.key {
  @apply p-1;
}

.key > div {
  @apply rounded flex justify-center items-center bg-gray-800;
}
</style>