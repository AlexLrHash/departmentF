<template>
  <div class="">
    <h1></h1>
    <div class="">
      <div class="">
        <div class="row" v-if="teachers.length != 0">
          <div class="col-lg-4" v-for="teacher in teachers">
            <img class="rounded-circle" :src="teacher.avatar" alt="Generic placeholder image" width="140" height="140">
            <h2>{{ teacher.name }}</h2>
            <p>{{ teacher.email }}</p>
            <p><NuxtLink :to="`/teachers/${teacher.id}`" class="btn btn-secondary" href="#" role="button">Посмотреть подробную информацию »</NuxtLink></p>
          </div><!-- /.col-lg-4 -->
        </div>
        <div class="" v-else>
          У данной дисциплины пока нет преподавателей
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Disciplines",
  data() {
    return {
      teachers: "",
    }
  },
  async mounted() {
    await this.getTeachers();

  },
  methods: {
    async getTeachers() {
      const response = await fetch('http://department.biz/api/disciplines/teachers/' + this.$route.params.id, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
        },
      });
      if (response.status == 500) {

      } else {
        this.teachers = await response.json();
        this.teachers = this.teachers.data;
      }
    },
  }
}
</script>

<style scoped>

</style>
