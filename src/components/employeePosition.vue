<template>
  <li class="position-item" v-for="employee in employees" :key="employee.id">
    <input
      :value="employee.position"
      @keydown.enter="submitPositionName($event, employee)"
      readonly
      v-if="employee.showPosition == false"
    />
    <a
      class="link"
      v-if="employee.showPosition == false"
      @click="employee.showPosition = true"
      ><svg
        xmlns="http://www.w3.org/2000/svg"
        width="16"
        height="16"
        fill="currentColor"
        class="bi bi-chevron-down"
        viewBox="0 0 16 16"
      >
        <path
          fill-rule="evenodd"
          d="M1.646 4.646a.5.5 0 0 1 .708 0L8 10.293l5.646-5.647a.5.5 0 0 1 .708.708l-6 6a.5.5 0 0 1-.708 0l-6-6a.5.5 0 0 1 0-.708z"
        /></svg
    ></a>
    <div v-if="employee.showPosition">
      <form class="position-select-form" action="">
        <select ref="employeePositionEdit" name="positionList">
          <option v-for="position in positionInfo" :key="position.id">
            {{ position.name }}
          </option>
        </select>
        <input
          @click.prevent="confirmNewPosition(employee)"
          v-if="employee.showPosition"
          type="submit"
          value="OK"
        />
      </form>
    </div>
  </li>
</template>

<script>
export default {
  props: ['employees', 'positionInfo'],
  methods: {
    submitPositionName(text, employeePosition) {
      employeePosition.name = text.target.value
      text.target.setAttribute('readonly', 'true')
      text.target.classList.remove('editing')
    },
    confirmNewPosition(employee) {
      employee.position = this.$refs.employeePositionEdit.value
      var newSalary

      for (let index = 0; index < this.positionInfo.length; index++) {
        let positionItem = this.positionInfo[index]
        if (positionItem.name === this.$refs.employeePositionEdit.value) {
          newSalary = this.positionInfo[index].salary
        }
      }

      employee.salary = newSalary
      employee.showPosition = false
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
    width: 70%;
    font-size: 1rem;
    border: 1px solid transparent;
    padding: 0.5rem 0;
    &:focus-within {
      outline: none;
    }
  }
}

.position-select-form {
  display: flex;
  flex-direction: row;
  margin-left: 12.5%;
  select {
    padding: 0.5rem 1rem;
    font-size: 1rem;
    font-family: inherit;
    text-align: center;
  }
  input {
    padding: 0 1rem;
    margin-left: 1rem;
    background-color: #4caf50;
    font-weight: 700;
    color: #fff;
  }
}
.link {
  cursor: pointer;
  align-self: center;
  svg {
    margin-top: 45%;
  }
}
.editing {
  cursor: text !important;
  border: 1px solid black !important;
  border-radius: 2em;
}
</style>
