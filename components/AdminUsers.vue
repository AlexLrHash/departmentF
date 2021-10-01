<template>
  <div style="margin-top: 100px;">
    <div class="table-responsive">
      <div class="container row">
        <div class="ml-2">
          <input type="text" v-model="userName" placeholder="Укажите имя" class="form-control">
        </div>
        <div class="ml-2">
          <input type="text" v-model="userEmail" placeholder="Укажите почту" class="form-control">
        </div>
        <div class="ml-2">
          <select v-model="userRole" class="form-control" id="">
            <option value="TEACHER">Преподаватель</option>
            <option value=""></option>
          </select>
        </div>
        <div class="ml-2">
          <button class="btn btn-success" @click="getTeachers">Поиск</button>
        </div>
      </div>
      <br>
      <table class="table table-striped table-sm">
        <thead>
        <tr>
          <th>#</th>
          <th>Имя</th>
          <th>Почта</th>
          <th>Роль</th>
          <th>Отделение/Дисциплина</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="user in users">
          <td></td>
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.role }}</td>
          <td v-if="user.role == 'Преподаватель'">
            <NuxtLink :to="`/admin/users/teachers/${user.id}`">Диспциплины</NuxtLink></td>
          <td v-else-if="user.role == 'Админ'">Отец</td>
          <td v-else></td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "AdminUsers",
  data() {
    return {
      users: "",
      userName: "Kiera",
      userEmail: "",
      userRole: "TEACHER"
    }
  },
  async mounted() {
    await this.getTeachers();
  },
  methods: {
    async getTeachers() {
      const response = await fetch(`http://localhost:8000/api/admin/users?name=${this.userName}&email=${this.userEmail}&role=${this.userRole}`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
      });
      if (response.status == 500) {

      } else {
        this.users = await response.json();
        this.users = this.users.data;
      }
    }
  }
}
</script>

<style scoped>

</style>
