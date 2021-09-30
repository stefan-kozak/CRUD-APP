<template>
  <li class="position-item" v-for="employee in employees" :key="employee.id">
    <a @click="$emit('removeEmployee', employee)" class="employee-remove">✖</a>
  </li>
</template>

<script>
export default {
  props: ['employees'],
  emits: ['removeEmployee'],
}
</script>

<style lang="scss" scoped>
.employee-remove {
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
