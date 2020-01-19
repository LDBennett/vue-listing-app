<template>
  <div id="app" class="container">
    <h1>Employees</h1>
    <div class="row">
      <EmployeeForm @add:employee="addEmployee" />
    </div>
    <div class="row">
      <EmployeeTable :employees="employees" :headers="headers"/>
    </div>
  </div>
</template>

<script>

import EmployeeTable from '@/components/EmployeeTable.vue'
import EmployeeForm from '@/components/EmployeeForm.vue'

import './styles/app.scss';

export default {
  name: 'app',
  components: {
    EmployeeTable,
    EmployeeForm,
  },
  data() {
    return {
      headers:[
        {
          id: 1,
          name: 'ID',
        },
        {
          id: 2,
          name: 'Name'
        },
        {
          id: 3,
          name: 'Email'
        }
      ],
      employees: [
        {
          id: 1,
          name: 'Richard Hendricks',
          email: 'richard@piedpiper.com',
        },
        {
          id: 2,
          name: 'Bertram Gilfoyle',
          email: 'gilfoyle@piedpiper.com',
        },
        {
          id: 3,
          name: 'Dinesh Chugtai',
          email: 'dinesh@piedpiper.com',
        },
      ],
    }
  },
  methods: {
    addEmployee(employee) {
      let lastId =
        this.employees.length > 0
          ? this.employees[this.employees.length - 1].id
          : 0;
      let id = lastId + 1;
      let newEmployee = { ...employee, id };

      this.employees = [...this.employees, newEmployee];
    },
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
