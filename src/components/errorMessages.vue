<template>
  <div class="error-wrap" v-if="errors.length">
    <b>Akciu sa nepodarilo vykonať:</b>
    <ul>
      <li :key="error" v-for="error in errors">{{ error }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ['errors'],
}
</script>

<style lang="scss" scoped>
.error-wrap {
  position: absolute;
  right: 0;
  bottom: 0;
  background: rgba(#c82333, 0.8);
  border-radius: 2rem;
  padding: 2rem 3.5rem;
  color: #fff;
  font-size: 1rem;
  margin: 1rem;
  z-index: 5;
  ul {
    list-style: square;
    margin-top: 1rem;
  }
}
</style>
