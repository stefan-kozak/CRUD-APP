<template>
  <li class="position-item" v-for="position in positionInfo" :key="position.id">
    <input
      @keydown.enter="submitPositionSalary($event, position)"
      :value="position.salary"
      readonly
      @dblclick="handleEdit($event)"
    />
  </li>
</template>

<script>
export default {
  props: ['positionInfo'],
  methods: {
    handleEdit(e) {
      e.target.removeAttribute('readonly')
      e.target.classList.add('editing')
      e.target.setSelectionRange(0, e.target.value.length)
    },
    submitPositionSalary(text, job) {
      job.salary = text.target.value
      text.target.setAttribute('readonly', 'true')
      text.target.classList.remove('editing')
    },
  },
}
</script>

<style lang="scss" scoped>
.position-item {
  display: flex;
  flex-direction: row;
  width: 80%;
  justify-content: center;
  margin: 0.5rem 0;

  input {
    text-align: center;
    cursor: pointer;
    width: 100%;
    font-size: 1rem;
    border: 1px solid transparent;
    padding: 0.5rem 0;
    &:focus-within {
      outline: none;
    }
  }
}

.editing {
  cursor: text !important;
  border: 1px solid black !important;
  border-radius: 2em;
}
</style>
