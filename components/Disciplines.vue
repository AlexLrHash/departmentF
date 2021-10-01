<template>
  <div class="">
    <div class="" v-if="discipline">
      <section class="jumbotron text-center">
        <div class="container">
          <img class="rounded-circle" :src="discipline.background" alt="Generic placeholder image" width="200" height="200">
          <h1 class="jumbotron-heading">{{ discipline.name }}</h1>
          <p class="lead text-muted">{{ discipline.description }}</p>
          <table class="table table-striped table-sm">
            <thead>
            <tr>
              <th>Имя</th>
              <th>Почта</th>
              <th></th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="teacher in discipline.teachers">
              <td>{{ teacher.name }}</td>
              <td>{{ teacher.email }}</td>
              <td><NuxtLink :to="`/teachers/${teacher.id}`" class="btn btn-primary">Подробнее >></NuxtLink></td>
            </tr>
            </tbody>
          </table>
        </div>
      </section>
    </div>
    <div v-else="" class="">
      <div class="row" v-for="discipline in disciplines.data">
        <div class="col-lg-4">
          <img class="rounded-circle" :src="discipline.background" alt="Generic placeholder image" width="140" height="140">
          <h2>{{ discipline.name }}</h2>
          <p>{{ discipline.description }}</p>
          <p><NuxtLink :to="`/disciplines/${discipline.id}`" class="btn btn-secondary" href="#" role="button">Посмотреть подробную информацию »</NuxtLink></p>
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
      disciplines: "",
      discipline: "",
      disciplineId: "",
    }
  },
  async mounted() {
    this.disciplineId = this.$route.params.id;
    if (this.disciplineId) {
      const response = await fetch('http://localhost:8000/api/disciplines/' + this.disciplineId, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
        },
      });
      if (response.status == 500) {

      } else {
        this.discipline = await response.json();
        this.discipline = this.discipline.data;
      }
    } else {
      const response = await fetch('http://localhost:8000/api/disciplines', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
        },
      });
      if (response.status == 500) {

      } else {
        this.disciplines = await response.json();
      }
    }
  }
}
</script>

<style scoped>

</style>
