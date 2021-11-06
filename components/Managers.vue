<template>
  <div class="">
    <div class="" v-if="manager">
      <section class="jumbotron text-center">
        <div class="container">
          <img class="" :src="manager.avatar" alt="Generic placeholder image" height="200">
          <h1 class="jumbotron-heading">{{ manager.name }}</h1>
          <p class="lead text-muted">{{ manager.email }}</p>
          <p>Заведующий отделением: {{ manager.department.name }}</p>
          <NuxtLink :to="`/departments/${manager.department.id}`" class="btn btn-primary">Подробнее</NuxtLink>
        </div>
      </section>
    </div>
    <div v-else="" class="">
      <div class="row" v-for="manager in managers.data">
        <div class="col-lg-4">
          <img class="rounded-circle" :src="manager.avatar" alt="Generic placeholder image" width="140" height="140">
          <h2>{{ manager.name }}</h2>
          <p>{{ manager.email }}</p>
          <p><NuxtLink :to="`/managers/${manager.id}`" class="btn btn-secondary" href="#" role="button">Посмотреть подробную информацию »</NuxtLink></p>
        </div><!-- /.col-lg-4 -->
        <NuxtLink :to=`/departments/${manager.department.id}`>Информация о отделении</NuxtLink>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Managers",
  data() {
    return {
      manager: '',
      managers: '',
      managerId: ''
    }
  },
  async mounted() {
    this.managerId = this.$route.params.id;
    if (this.managerId) {
      await this.getManager()
    } else {
     await this.getManagers()
    }
  },
  methods: {
    async getManager() {
      const response = await fetch('http://department.biz/api/managers/' + this.managerId, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
        },
      });
      if (response.status == 500) {

      } else {
        this.manager = await response.json();
        this.manager = this.manager.data;
      }
    },
    async getManagers()
    {
      const response = await fetch('http://department.biz/api/managers', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
        },
      });
      if (response.status == 500) {

      } else {
        this.managers = await response.json();
      }
    }
  }
}
</script>

<style scoped>

</style>
