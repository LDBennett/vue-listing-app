<template>
  <div>
    <table class="u-full-width">
      <thead>
        <tr>
          <th v-for="header in headers" :key="header.id">{{ header.name }}</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="employee in employees" :key="employee.id">
          <td>{{ employee.id }}</td>
          <td>{{ employee.name }}</td>
          <td>{{ employee.email }}</td>
        </tr>
      </tbody>

    </table>
  </div>
</template>

<script>
  export default {
    name: 'employee-table',
    props: {
      headers: Array,
      employees: Array,
    },
  }
</script>

<style scoped></style>
