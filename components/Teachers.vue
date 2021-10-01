<template>
  <div class="">
    <div class="" v-if="teacher">
      <section class="jumbotron text-center">
        <div class="container">
          <img class="rounded-circle" :src="teacher.avatar" alt="Generic placeholder image" width="200" height="200">
          <h1 class="jumbotron-heading">{{ teacher.name }}</h1>
          <p class="lead text-muted">{{ teacher.email }}</p>
          <table class="table table-striped table-sm">
            <thead>
            <tr>
              <th>Название</th>
              <th>Описание</th>
              <th>Количество практик</th>
              <th>Количество лаб</th>
              <th></th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="discipline in teacher.disciplines">
              <td>{{ discipline.name }}</td>
              <td>{{ discipline.description }}</td>
              <td>{{ discipline.number_of_practices }}</td>
              <td>{{ discipline.number_of_labs }}</td>
              <td><NuxtLink :to="`/disciplines/${discipline.id}`" class="btn btn-primary">Подробнее >></NuxtLink></td>
            </tr>
            </tbody>
          </table>
        </div>
      </section>
    </div>
    <div v-else="" class="">
      <div class="row" v-for="teacher in teachers.data">
        <div class="col-lg-4">
          <img class="rounded-circle" :src="teacher.avatar" alt="Generic placeholder image" width="140" height="140">
          <h2>{{ teacher.name }}</h2>
          <p>{{ teacher.email }}</p>
          <p><NuxtLink :to="`/teachers/${teacher.id}`" class="btn btn-secondary" href="#" role="button">Посмотреть подробную информацию »</NuxtLink></p>
        </div><!-- /.col-lg-4 -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Teachers",
  data() {
    return {
      teachers: "",
      teacher: "",
      teacherId: "",
    }
  },
  async mounted() {
    this.teacherId = this.$route.params.id;
    if (this.teacherId) {
      const response = await fetch('http://localhost:8000/api/teachers/' + this.teacherId, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
        },
      });
      if (response.status == 500) {

      } else {
        this.teacher = await response.json();
        this.teacher = this.teacher.data;
      }
    } else {
      const response = await fetch('http://localhost:8000/api/teachers', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
        },
      });
      if (response.status == 500) {

      } else {
        this.teachers = await response.json();
      }
    }
  }
}
</script>

<style scoped>

</style>
