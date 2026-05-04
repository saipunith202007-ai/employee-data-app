<template>
  <div class="container mt-4">
    <h2 class="mb-3">Employee Management</h2>

    <!-- FORM -->
    <form @submit.prevent="saveEmployee">
      <input
        v-model="employee.name"
        class="form-control mb-2"
        placeholder="Name"
        required
      />
      <input
        v-model="employee.designation"
        class="form-control mb-2"
        placeholder="Designation"
        required
      />
      <input
        v-model="employee.department"
        class="form-control mb-2"
        placeholder="Department"
        required
      />
      <input
        v-model="employee.salary"
        type="number"
        class="form-control mb-2"
        placeholder="Salary"
        required
      />

      <button class="btn btn-primary">
        {{ editMode ? "Update Employee" : "Add Employee" }}
      </button>
    </form>

    <!-- TABLE -->
    <table class="table table-bordered table-striped mt-4">
      <thead class="table-dark">
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Designation</th>
          <th>Department</th>
          <th>Salary</th>
          <th>Actions</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="emp in employees" :key="emp.id">
          <td>{{ emp.id }}</td>
          <td>{{ emp.name }}</td>
          <td>{{ emp.designation }}</td>
          <td>{{ emp.department }}</td>
          <td>{{ emp.salary }}</td>
          <td>
            <button
              class="btn btn-warning btn-sm me-2"
              @click="editEmployee(emp)"
            >
              Edit
            </button>
            <button
              class="btn btn-danger btn-sm"
              @click="deleteEmployee(emp.id)"
            >
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";s

const API_URL = "https://69f55ddefb098eb7f0b539c1.mockapi.io/employee";

export default {
  data() {
    return {
      employees: [],
      employee: {
        name: "",
        designation: "",
        department: "",
        salary: "",
      },
      editMode: false,
      editId: null,
    };
  },

  methods: {
    // GET
    async fetchEmployees() {
      const res = await axios.get(API_URL);
      this.employees = res.data;
    },

    // POST + PUT
    async saveEmployee() {
      if (this.editMode) {
        await axios.put(`${API_URL}/${this.editId}`, this.employee);
        this.editMode = false;
        this.editId = null;
      } else {
        await axios.post(API_URL, this.employee);
      }

      this.resetForm();
      this.fetchEmployees();
    },

    // EDIT
    editEmployee(emp) {
      this.employee = { ...emp };
      this.editMode = true;
      this.editId = emp.id;
    },

    // DELETE
    async deleteEmployee(id) {
      await axios.delete(`${API_URL}/${id}`);
      this.fetchEmployees();
    },

    // RESET FORM
    resetForm() {
      this.employee = {
        name: "",
        designation: "",
        department: "",
        salary: "",
      };
    },
  },

  mounted() {
    this.fetchEmployees();
  },
};
</script>