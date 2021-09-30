<template>
  <div class="small-nav">
    <a
      :class="{ active: manageEmployee }"
      @click=";(manageEmployee = true), (managePositions = false)"
      >Zamestnanci</a
    >
    <a
      :class="{ active: managePositions }"
      @click=";(manageEmployee = false), (managePositions = true)"
      >Pracovné pozície</a
    >
  </div>

  <div class="employees-wrap" v-if="manageEmployee">
    <div class="employees-main">
      <div class="position-name">
        <div class="title">
          <h1>Pracovná pozícia</h1>
        </div>
        <div class="position-name-list">
          <ul>
            <employeePosition
              :employees="this.employees"
              :positionInfo="this.positions"
            />
          </ul>
        </div>
      </div>

      <div class="employee-name">
        <div class="title">
          <h1>Meno</h1>
        </div>
        <div class="position-name-list">
          <ul>
            <employeeName :employees="this.employees" />
          </ul>
        </div>
      </div>

      <div class="employee-contact-email">
        <div class="title">
          <h1>Email</h1>
        </div>
        <div class="position-name-list">
          <ul>
            <employeeEmail :employees="this.employees" />
          </ul>
        </div>
      </div>

      <div class="employee-contact-phone">
        <div class="title">
          <h1>Telefón</h1>
        </div>
        <div class="position-name-list">
          <ul>
            <employeePhone :employees="this.employees" />
          </ul>
        </div>
      </div>

      <div class="employee-salary">
        <div class="title">
          <h1>Plat</h1>
        </div>
        <div class="position-name-list">
          <ul>
            <employeeSalary
              :employees="this.employees"
              :positions="this.positions"
            />
          </ul>
        </div>
      </div>
      <div class="position-remove">
        <div class="position-remove-list">
          <ul>
            <employeeRemove
              :employees="this.employees"
              @removeEmployee="removeEmployee($event)"
            />
          </ul>
        </div>
      </div>
      <div class="position-add">
        <div class="title">
          <h1><a @click="addEmployeeForm = true">Pridať +</a></h1>
        </div>
      </div>
    </div>
    <div v-if="addEmployeeForm" class="employee-add-form">
      <h1>Pridať nového zamestnanca</h1>
      <select
        ref="employeeAddPosition"
        name="positionList"
        class="employee-add-name"
      >
        <option v-for="position in this.positions" :key="position.id">
          {{ position.name }}
        </option>
      </select>

      <input
        class="employee-add-name"
        type="text"
        placeholder="Titul"
        v-model="employeeTitle"
        @keydown.enter="handleAddEmployeeSubmit"
      />

      <input
        class="employee-add-name"
        type="text"
        placeholder="Krstné meno"
        v-model="employeeFirstName"
        @keydown.enter="handleAddEmployeeSubmit"
      />

      <input
        class="employee-add-name"
        type="text"
        placeholder="Priezvisko"
        v-model="employeeLastName"
        @keydown.enter="handleAddEmployeeSubmit"
      />

      <input
        class="employee-add-name"
        type="text"
        placeholder="E-mail"
        v-model="employeeEmail"
        @keydown.enter="handleAddEmployeeSubmit"
      />

      <input
        class="employee-add-name"
        type="text"
        placeholder="Telefón"
        v-model="employeePhone"
        @keydown.enter="handleAddEmployeeSubmit"
      />

      <input
        class="employee-add-salary"
        type="text"
        placeholder="Plat"
        v-model="employeeSalary"
        @keydown.enter="handleAddEmployeeSubmit"
      />
      <div class="add-position-btns">
        <a class="cancel-add-position" @click="addEmployeeForm = false">Späť</a>
        <a @click="handleAddEmployeeSubmit" class="submit-add-position"
          >Pridať</a
        >
      </div>
    </div>
    <errorMessages :errors="this.errors" />
  </div>

  <div class="positions-wrap" v-if="managePositions">
    <div class="positions-main">
      <div class="position-name">
        <div class="title">
          <h1>Pracovná pozícia</h1>
        </div>
        <div class="position-name-list">
          <ul>
            <positionName :positionInfo="this.positions" />
          </ul>
        </div>
      </div>

      <div class="position-salary">
        <div class="title">
          <h1>Plat</h1>
        </div>
        <div class="position-salary-list">
          <ul>
            <positionSalary :positionInfo="this.positions" />
          </ul>
        </div>
      </div>

      <div class="position-remove">
        <div class="position-remove-list">
          <ul>
            <positionRemove
              :positionInfo="this.positions"
              @removePosition="removePosition($event)"
            />
          </ul>
        </div>
      </div>
      <div class="position-add">
        <div class="title">
          <h1 @click="addPositionForm = true"><a>Pridať +</a></h1>
        </div>
      </div>
    </div>
    <div class="add-position-formular" v-if="addPositionForm">
      <div class="position-add-form">
        <h1>Pridať novú pracovnú pozíciu</h1>
        <input
          class="position-add-name"
          type="text"
          placeholder="Meno pozície"
          v-model="positionName"
          @keydown.enter="handleAddPositionSubmit"
          ref="addPositionInput"
        />

        <input
          class="position-add-salary"
          type="text"
          placeholder="Plat"
          v-model="positionSalary"
          @keydown.enter="handleAddPositionSubmit"
        />
        <div class="add-position-btns">
          <a class="cancel-add-position" @click="addPositionForm = false"
            >Späť</a
          >
          <a @click="handleAddPositionSubmit" class="submit-add-position"
            >Pridať</a
          >
        </div>
      </div>
    </div>

    <errorMessages :errors="this.errors" />
  </div>
  <div class="add-employee-formular" v-if="addEmployeeForm"></div>
</template>

<script>
import positionName from '@/components/positionName.vue'
import positionSalary from '@/components/positionSalary.vue'
import positionRemove from '@/components/positionRemove.vue'
import errorMessages from '@/components/errorMessages.vue'
import employeeName from '@/components/employeeName.vue'
import employeePosition from '@/components/employeePosition.vue'
import employeeEmail from '@/components/employeeEmail.vue'
import employeePhone from '@/components/employeePhone.vue'
import employeeSalary from '@/components/employeeSalary.vue'
import employeeRemove from '@/components/employeeRemove.vue'

export default {
  components: {
    positionName,
    positionSalary,
    positionRemove,
    errorMessages,
    employeeName,
    employeePosition,
    employeeEmail,
    employeePhone,
    employeeSalary,
    employeeRemove,
  },
  methods: {
    removePosition(currentPosition) {
      for (let i = 0; i < this.employees.length; i++) {
        const element = this.employees[i].position
        this.checkEmployeePosition.push(element)
      }
      if (this.checkEmployeePosition.includes(currentPosition.name)) {
        this.errors.push(
          'Nedá sa vymazať táto pozícia pretože sa práve používa'
        )
      } else {
        this.positions = this.positions.filter(
          (FromAllPositions) => FromAllPositions !== currentPosition
        )
      }

      this.checkEmployeePosition = []
      setTimeout(() => {
        this.errors = []
      }, 5000)

      //this.positions = this.positions.filter((FromAllPositions) => FromAllPositions !== currentPosition)
      //this.errors.push('Nedá sa vymazať táto pozícia pretože sa práve používa')
    },
    removeEmployee(currentEmployee) {
      this.employees = this.employees.filter(
        (FromAllEmployees) => FromAllEmployees !== currentEmployee
      )
    },
    handleAddPositionSubmit() {
      this.errors = []
      if (this.positionName.trim() && this.positionSalary.trim()) {
        this.positions.push({
          id: this.positions.length,
          name: this.positionName.trim(),
          salary: this.positionSalary.trim(),
        })
        this.positionSalary = ''

        this.positionName = ''
        this.$refs.addPositionInput.focus()
        this.addPositionForm = false

        return true
      } else {
        this.errors.push('Meno a plat nemôžu obsahovať len medzeru')
      }

      if (!this.positionName) {
        this.errors.push('Meno pozície musí byť vyplnené.')
      }

      if (!this.positionSalary) {
        this.errors.push('Plat musí byť vyplnený.')
      }

      setTimeout(() => {
        this.errors = []
      }, 5000)
    },

    handleAddEmployeeSubmit() {
      this.errors = []

      if (
        this.employeeFirstName &&
        this.employeeLastName &&
        this.employeeEmail &&
        this.employeePhone &&
        this.employeeSalary
      ) {
        this.employees.push({
          id: this.employees.length,
          title: this.employeeTitle,
          firstName: this.employeeFirstName,
          lastName: this.employeeLastName,
          email: this.employeeEmail,
          phoneNumber: this.employeePhone,
          salary: this.employeeSalary,
          showPosition: false,
          position: this.$refs.employeeAddPosition.value,
        })
        this.employeeTitle = ''
        this.employeeFirstName = ''
        this.employeeLastName = ''
        this.employeeEmail = ''
        this.employeePhone = ''
        this.employeeSalary = ''
        this.addEmployeeForm = false

        return true
      }

      if (!this.employeeFirstName) {
        this.errors.push('Krstné meno zamestnanca musí byť vyplnené.')
      }

      if (!this.employeeLastName) {
        this.errors.push('Priezvisko zamestnanca musí byť vyplnené.')
      }

      if (!this.employeeEmail) {
        this.errors.push('Email zamestnanca musí byť vyplnený.')
      }

      if (!this.employeePhone) {
        this.errors.push('Telefón zamestnanca musí byť vyplnený.')
      }
      if (!this.employeeSalary) {
        this.errors.push('Plat zamestnanca musí byť vyplnený.')
      }

      setTimeout(() => {
        this.errors = []
      }, 5000)
    },
  },

  data() {
    return {
      errors: [],
      positionName: '',
      positionSalary: '',
      checkEmployeePosition: [],
      manageEmployee: true,
      managePositions: false,
      addPositionForm: false,
      addEmployeeForm: false,
      employeeTitle: '',
      employeeFirstName: '',
      employeeLastName: '',
      employeeEmail: '',
      employeePhone: '',
      employeeSalary: '',
      employees: [
        {
          id: 0,
          title: '',
          firstName: 'Štefan',
          lastName: 'Kozák',
          email: 'stefan.kozak99@gmail.com',
          phoneNumber: '0917648474',
          position: 'Junior Front-end developer',
          salary: '1450€',
          showPosition: false,
        },

        {
          id: 1,
          title: 'Dr.',
          firstName: 'John',
          lastName: 'Doe',
          email: 'johndoe@gmail.com',
          phoneNumber: '0917648474',
          position: 'Back-end developer',
          salary: '3000€',
          showPosition: false,
        },

        {
          id: 2,
          title: 'Mgr.',
          firstName: 'Susan',
          lastName: 'Doe',
          email: 'susandoe@gmail.com',
          phoneNumber: '0917648474',
          position: 'Accountant',
          salary: '2000€',
          showPosition: false,
        },
      ],
      positions: [
        {
          id: 0,
          name: 'Junior Front-end developer',
          salary: '1400€',
        },

        {
          id: 1,
          name: 'Back-end developer',
          salary: '3000€',
        },

        {
          id: 2,
          name: 'Accountant',
          salary: '2000€',
        },
      ],
    }
  },
}
</script>

<style lang="scss" scoped>
.positions-wrap,
.employees-wrap {
  margin-top: 5%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  width: 100%;
}

.employees-main {
  display: flex;
  justify-content: center;
  flex-direction: row;
}

.position-name,
.employee-name,
.employee-contact-email,
.employee-contact-phone,
.employee-salary,
.position-add {
  display: flex;
  flex-direction: column;
  align-self: flex-start;
  width: 25%;
  .title {
    width: 100%;
    h1 {
      font-size: 1.5rem;
      text-align: center;
      padding: 0.5rem 0;
      border-bottom: 1px solid;
    }
  }
}

.employee-name {
  width: 15%;
}

.employee-contact-email {
  width: 15%;
}

.employee-contact-phone {
  width: 10%;
}

.employee-salary {
  width: 6%;
}

.position-name-list,
.position-salary-list {
  ul {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    padding: 0;
    margin: 0;
    width: 100%;
  }
}

.positions-wrap {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  width: 100%;
}

.positions-main {
  display: flex;
  flex-direction: row;
  justify-content: center;
}

.position-name {
  width: 25%;
}

.position-add {
  width: 10%;

  h1 {
    cursor: pointer;
    padding: 0.5rem 0;
    background-color: #05457e;
    color: #fff;
    border-radius: 1rem;
  }
  .title {
    margin-left: 3rem;
  }
}

.position-salary {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  width: 10%;
  .title {
    width: 100%;
    h1 {
      font-size: 1.5rem;
      text-align: center;
      padding: 0.5rem 0;
      border-bottom: 1px solid;
    }
  }
}

.position-remove {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  width: 2%;
  ul {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .title {
    width: 100%;
    h1 {
      font-size: 1.5rem;
      color: transparent;
      text-align: center;
    }
  }

  .position-remove-list {
    display: flex;
    justify-content: center;
    ul {
      padding: 0;
      margin: 0;
    }
  }
}

.position-name-list,
.position-salary-list {
  ul {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    padding: 0;
    margin: 0;
    width: 100%;
  }
}

.table-search {
  display: flex;
  flex-direction: row;
  height: max-content;
  margin: 1rem;
  width: 20%;
  svg {
    width: 10%;
    margin: 0 1rem;
  }
  input {
    height: max-content;
    font-size: 1.5rem;
    width: 80%;
  }
}
.add-position-formular,
.add-employee-formular {
  display: flex;
  position: fixed;
  height: 100vh;
  width: 100%;
  justify-content: center;
  background-color: rgba(#000, 0.8);
  align-items: center;
  top: 0;
  left: 0;
  z-index: 2;
  backdrop-filter: blur(10px);
}

.position-add-form,
.employee-add-form {
  display: flex;
  position: absolute;
  width: 40%;
  left: 29%;
  min-height: 500px;
  flex-direction: column;
  background-color: #121212;
  justify-content: center;
  text-align: center;
  border-radius: 2rem;
  padding: 1rem;
  margin-top: -8rem;
  z-index: 3;
  h1 {
    color: #fff;
    padding: 1rem;
  }
}

.position-add-name,
.position-add-salary,
.employee-add-name,
.employee-add-salary {
  font-size: 1rem;
  padding: 0.5rem 0;
  text-align: center;
  width: 50%;
  align-self: center;
  margin-top: 2rem;
  border-radius: 2em;
  border: 1px solid black;
  font-family: inherit;
  &:focus-visible {
    outline: none;
  }
  &::after {
    content: '*';
    position: relative;
  }
}

.position-add-salary {
  width: 50%;
}

.add-position-btns {
  margin-top: 3rem;
  padding: 1rem;
}

.submit-add-position {
  padding: 0.5rem 2rem;
  border: 1px solid transparent;
  font-size: 1rem;
  align-self: center;
  margin-top: 2rem;
  background: rgb(33, 136, 56);
  background: linear-gradient(
    45deg,
    rgba(33, 136, 56, 1) 0%,
    rgba(19, 157, 50, 1) 100%
  );
  color: #fff;
  border-radius: 2rem;
  cursor: pointer;
}

.cancel-add-position {
  padding: 0.5rem 2rem;
  border: 1px solid transparent;
  font-size: 1rem;
  align-self: center;
  margin-top: 2rem;
  color: #fff;
  border-radius: 2rem;
  cursor: pointer;
}

.small-nav {
  background-color: #05457e;
  padding: 1rem;
  a {
    padding: 1rem;
    cursor: pointer;
    color: #fff;
  }
}

.active {
  font-weight: 700;
  border-bottom: 10px solid white;
  border-width: 5px;
}
</style>
