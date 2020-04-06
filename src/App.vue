<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>

    <employee-form @add:employee="addEmployee" />
    <employee-table :employees="employees" @delete:employee="deleteEmployee" @edit:employee="editEmployee"/>
  </div>
</template>

<script>
import EmployeeTable from "@/components/EmployeeTable";
import EmployeeForm from "@/components/EmployeeForm";

export default {
  name: 'App',
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data() {
    return {
      employees: [
        {
          id: 1,
          name: 'Michael Scott',
          email: 'mscott@dundermifflin.com'
        },
        {
          id: 2,
          name: 'Jim Halpert',
          email: 'jhalpert@dundermifflin.com'
        },
        {
          id: 3,
          name: 'Pam Beesly',
          email: 'pbeesly@dundermifflin.com'
        }
      ]
    }
  },
  mounted() {
    this.getEmployees()
  },
  methods: {
    async getEmployees() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        const allEmployees = await response.json();
        this.employees = allEmployees;
      } catch (e) {
        console.error(e);
      }
    },
    async addEmployee(employee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users`, {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' }
        });
        const newEmployee = await response.json();
        this.employees = [...this.employees, newEmployee];
      } catch (e) {
        console.error(e);
      }
    },
    async deleteEmployee(employeeId) {
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${employeeId}`, {
          method: 'DELETE'
        });
        this.employees = this.employees.filter(
            employee => employee.id != employeeId
        );
      } catch (e) {
        console.error(e);
      }
    },
    async editEmployee(employeeId, updatedEmployee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${employeeId}`, {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: { 'Content-type': 'application/json; charset=UTF-8'}
        });
        const updatedEmployeeData = await response.json();
        this.employees = this.employees.map(
            employee => employee.id == employeeId ? updatedEmployeeData : employee
        );
      } catch (e) {
        console.error(e);
      }
    }
  }
}
</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 680px;
  }
</style>
