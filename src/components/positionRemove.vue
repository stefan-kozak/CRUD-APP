<template>
  <li class="position-item" v-for="position in positionInfo" :key="position.id">
    <a @click="$emit('removePosition', position)" class="position-remove">✖</a>
  </li>
</template>

<script>
export default {
  props: ['positionInfo'],
  emits: ['removePosition'],
}
</script>

<style lang="scss" scoped>
.position-remove {
  align-self: center;
  justify-content: center;
  padding: 0.58rem 0;
  color: #c82333 !important;
  cursor: pointer;
}

.position-item {
  display: flex;
  flex-direction: row;
  width: 100%;
  justify-content: center;
  padding: 0.5rem 0;
}
</style>
