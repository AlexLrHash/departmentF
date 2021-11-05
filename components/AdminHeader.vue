<template>
  <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark mb-5">
    <a class="navbar-brand" href="#">{{ user.name }}(Админ)</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarCollapse" aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarCollapse">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item active">
          <NuxtLink to="/admin" class="nav-link" href="#">Главная <span class="sr-only">(current)</span></NuxtLink>
        </li>
        <li class="nav-item ml-5 active">
          <NuxtLink to="/admin/users" class="nav-link" href="/">Пользователи</NuxtLink>
        </li>
        <li class="nav-item active">
          <NuxtLink to="/admin/disciplines" class="nav-link" href="#">Дисциплины</NuxtLink>
        </li>
        <li class="nav-item active">
          <NuxtLink to="/admin/departments" class="nav-link" href="#">Отделения</NuxtLink>
        </li>
        <li class="nav-item active">
          <NuxtLink to="/admin/likes" class="nav-link" href="#">Лайки</NuxtLink>
        </li>
      </ul>
      <form class="form-inline mt-2 mt-md-0">
        <NuxtLink to="/" class="btn btn-outline-primary my-2 my-sm-0" type="submit">Выйти из админ панели</NuxtLink>
      </form>
    </div>
  </nav>
</template>

<script>
export default {
  name: "AdminHeader",
  data() {
    return {
      user: ''
    }
  },
  async mounted() {
    await this.getUser();
  },
  methods: {
    async getUser() {
      const response = await fetch('http://department.biz/api/admin', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt'),
        },
      });
      if (response.ok) {
        this.user = await response.json();
        this.user = this.user.data;
      } else if (response.status == 403) {
        this.$router.push('/errors/permissions');
      } else if (response.status == 401) {
        this.$router.push('/errors/unauthenticated');
      }
    }
  }
}
</script>

<style scoped>

</style>
