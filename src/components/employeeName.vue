<template>
  <li class="position-item" v-for="employee in employees" :key="employee.id">
    <input
      :value="
        employee.title + ' ' + employee.firstName + ' ' + employee.lastName
      "
      readonly
      @keydown.enter="submitEmployeeName($event, employee)"
      @dblclick="handleEdit($event)"
    />
  </li>
</template>

<script>
export default {
  props: ['employees'],
  methods: {
    handleEdit(e) {
      e.target.removeAttribute('readonly')
      e.target.classList.add('editing')
      e.target.setSelectionRange(0, e.target.value.length)
    },
    submitEmployeeName(text, employee) {
      let splitedName = text.target.value.split(' ')
      if (splitedName.length == 3) {
        employee.title = splitedName[0].trim()
        employee.firstName = splitedName[1].trim()
        employee.lastName = splitedName[2].trim()
      } else if (splitedName.length == 2) {
        employee.firstName = splitedName[0].trim()
        employee.lastName = splitedName[1].trim()
        employee.title = ''
      } else if (splitedName.length == 1) {
        employee.firstName = splitedName[0].trim()
        employee.lastName = ''
        employee.title = ''
      } else {
        text.target.value =
          employee.title + ' ' + employee.firstName + ' ' + employee.lastName
      }
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
