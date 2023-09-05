<template>
  <div class="small-container">
    <button @click="selectedComponent = 'PageA'">A</button>
    <button @click="selectedComponent = 'PageB'">B</button>
    <component :is="selectedComponent"></component>
    <h1>Employees</h1>
    <employee-form @add:employee="addEmployee" />
    <employee-table
      v-bind:employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
import EmployeeTable from "./components/EmployeeTable.vue";
import EmployeeForm from "./components/EmployeeForm.vue";
import PageA from "./components/Add.vue";
import PageB from "./components/Remove.vue";

export default {
  name: "App",
  components: {
    EmployeeTable,
    EmployeeForm,
    PageA,
    PageB,
  },
  data() {
    return {
      employees: [],
      selectedComponent: "PageA",
    };
  },
  methods: {
    async addEmployee(employee) {
      try {
      const response = await fetch("https://jsonplaceholder.typicode.com/users", {
          method: "POST",
          body: JSON.stringify(employee),
          headers: {
            "Content-type": "application/json; charset=UTF-8",
          },
        });
        const data = await response.json();
        
        this.employees = [...this.employees, data];
      } catch (error) {
        console.log(error);
      }
    },

   async deleteEmployee(id) {
    try {
    await fetch("https://jsonplaceholder.typicode.com/users"+id,
     { method: "DELETE",
     })
      this.employees = this.employees.filter((employee) => {
        return employee.id != id;
      });
    } catch (error) {
      console.log(error);
    }
      
    },
  async  editEmployee(id, updatedEmployee) {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users"+id,
          { method: "PUT" ,
            body:JSON.stringify(updatedEmployee),
            headers: {"Content-type": "application/json; charset=UTF-8"},
        });
        const data = await response.JSON();
        this.employees.map((employee) => {
        return employee.id == id ? data : employee;
      });
      } catch (error) {
        console.log(error);
      }
      
    },
    async getEmployees() {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users",
          { method: "GET" }
        );
        const data = await response.json();
        this.employees = data;
      } catch (error) {
        console.log(error);
      }
    },
  },
  mounted() {
    this.getEmployees();
  },
};
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
