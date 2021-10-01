<template>
  <div style="margin-top: 100px;">
    <div class="table-responsive">
      <div class="container row">
        <div class="ml-2">
          <input type="text" v-model="departmentName" placeholder="Укажите название" class="form-control">
        </div>
        <div class="ml-2">
          <input type="text" v-model="departmentManager" placeholder="Укажите заведующего" class="form-control">
          <div class="" style="position: absolute; background: white; opacity: .9; color: white">
            <div v-for="manager in departmentManagers" style="cursor: pointer" @click="selectManager(manager.name)">{{ manager.name }}</div>
          </div>
        </div>
        <div class="ml-2">
          <button class="btn btn-success" @click="getDepartments">Поиск</button>
        </div>
      </div>
      <br>
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
      departments: "",
      departmentName: "",
      departmentManagers: "",
      departmentManager: ""
    }
  },
  watch: {
    departmentManager: async function (val) {
      await this.getManagers(val);
    }
  },
  async mounted() {
    await this.getDepartments();
  },
  methods: {
    async getDepartments() {
      const response = await fetch(`http://localhost:8000/api/admin/departments?name=${this.departmentName}&manager=${this.departmentManager}`, {
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
    },
    selectManager(managerName) {
      this.departmentManager = managerName;
    },
    async getManagers(val) {
      const response = await fetch(`http://localhost:8000/api/admin/users?name=${val}&email=&role=MANAGER`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
      });
      if (response.status == 500) {
      } else {
        this.departmentManagers = await response.json();
        this.departmentManagers = this.departmentManagers.data;
      }
    }
  }
}
</script>

<style scoped>

</style>
