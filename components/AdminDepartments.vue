<template>
  <div style="margin-top: 100px;">
    <div class="table-responsive">
      <table class="table table-striped table-sm">
        <thead>
        <tr>
          <th>Название</th>
          <th>Описание</th>
          <th>Зав отделения</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="department in departments">
          <td>{{ department.name }}</td>
          <td>{{ department.description }}</td>
          <td>{{ department.manager.name }}</td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "AdminDepartments",
  data() {
    return {
      departments: ""
    }
  },
  async mounted() {
    await this.getDepartments();
  },
  methods: {
    async getDepartments() {
      const response = await fetch('http://localhost:8000/api/admin/departments', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
      });
      if (response.status == 500) {
      } else {
        this.departments = await response.json();
        this.departments = this.departments.data;
      }
    }
  }
}
</script>

<style scoped>

</style>
