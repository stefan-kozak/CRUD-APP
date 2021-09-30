<template>
  <li class="position-item" v-for="employee in employees" :key="employee.id">
    <input
      :value="employee.salary"
      readonly
      @keydown.enter="submitEmployeeSalary($event, employee)"
      @dblclick="handleEdit($event)"
    />
  </li>
</template>

<script>
export default {
  props: ['employees', 'positions'],
  data() {
    return {
      employeesFinalSalary: [],
    }
  },
  methods: {
    handleEdit(e) {
      e.target.removeAttribute('readonly')
      e.target.classList.add('editing')
      e.target.setSelectionRange(0, e.target.value.length)
    },
    submitEmployeeSalary(text, employee) {
      employee.salary = text.target.value
      text.target.classList.remove('editing')
      text.target.setAttribute('readonly', 'true')
    },
  },
}
</script>

<style lang="scss" scoped>
.position-item {
  display: flex;
  flex-direction: row;
  width: 100%;
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
