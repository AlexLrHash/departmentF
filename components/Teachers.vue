<template>
  <div class="">
    <div class="" v-if="teacher">
      <section class="jumbotron text-center">
        <div class="container">
          <img class="rounded-circle" :src="teacher.avatar" alt="Generic placeholder image" width="200" height="200">
          <h1 class="jumbotron-heading">{{ teacher.name }}</h1>
          <p class="lead text-muted">{{ teacher.email }}</p>
          <div class="text-center">
            <button class="btn btn-primary ml-2" @click="likeTeacher">
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-heart-fill" viewBox="0 0 16 16">
                <path fill-rule="evenodd" d="M8 1.314C12.438-3.248 23.534 4.735 8 15-7.534 4.736 3.562-3.248 8 1.314z"/>
              </svg> {{ this.teacherCountLikes }}
            </button>
          </div>
          <table class="table table-striped table-sm mt-2">
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
      <div class="row">
        <div class="col-lg-4" v-for="teacher in teachers.data">
          <img class="rounded-circle" :src="teacher.avatar" alt="Generic placeholder image" width="140" height="140">
          <h2>{{ teacher.name }}</h2>
          <p>{{ teacher.email }}</p>
          <p><NuxtLink :to="`/teachers/${teacher.id}`" class="btn btn-secondary" href="#" role="button">Посмотреть подробную информацию »</NuxtLink></p>
        </div><!-- /.col-lg-4 -->
      </div>
    </div>
    <b-modal id="bv-modal-example" v-model="showErrorModal" hide-footer>
      <template #modal-title>
        Оценка преподавателя
      </template>
      <div class="d-block text-center">
        Вы уже оценили этого преподавателя.
      </div>
      <div class="row text-center">
        <b-button class="ml-2 mt-3" block @click="$bvModal.hide('bv-modal-example')">Назад</b-button>
      </div>
    </b-modal>
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
      teacherCountLikes: 0,
      showErrorModal: 0
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
        this.teacherCountLikes = this.teacher.count_likes;
        this.teacherCountDislikes = this.teacher.count_dislikes;
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

  },
  methods: {
    async likeTeacher() {
      this.teacherId = this.$route.params.id;
      if (this.teacherId) {
        const response = await fetch(`http://localhost:8000/api/teachers/${this.teacherId}/like`, {
          headers: {
            'Content-Type': 'application/json',
            "Accept": "application/json",
            'Authorization': "Bearer " + localStorage.getItem('jwt'),
          },
          method: 'post'
        });
        if (response.status == 422) {
          this.showErrorModal = true;
        } else if (response.status == 403) {
          this.$router.push({path: '/auth/verify/'});
        } else {
          this.teacherCountLikes ++;
          // this.teacher = this.teacher.data;
        }
      }
    },
  }
}
</script>

<style scoped>

</style>
