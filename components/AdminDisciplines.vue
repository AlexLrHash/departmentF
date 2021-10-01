<template>
  <div style="margin-top: 100px;">
    <div class="table-responsive">
      <div class="container row">
        <div class="ml-2">
          <input type="text" v-model="disciplineName" placeholder="Укажите название" class="form-control">
        </div>
        <div class="ml-2">
          <input type="text" v-model="disciplineTeacher" placeholder="Укажите преподавателя" class="form-control">
          <div class="" style="position: absolute; background: white; opacity: .9; color: white">
            <div v-for="teacher in disciplineTeachers" style="cursor: pointer" @click="selectTeacher(teacher.name)">{{ teacher.name }}</div>
          </div>
        </div>
        <div class="ml-2">
          <button class="btn btn-success" @click="getDisciplines">Поиск</button>
        </div>
      </div>
      <br>
      <table class="table table-striped table-sm">
        <thead>
        <tr>
          <th>#</th>
          <th>Название</th>
          <th>Описание</th>
          <th>Количество лаб</th>
          <th>Количество практик</th>
          <th>Преподаватели</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="discipline in disciplines">
          <td></td>
          <td>{{ discipline.name }}</td>
          <td>{{ discipline.description }}</td>
          <td>{{ discipline.number_of_labs }}</td>
          <td>{{ discipline.number_of_practices }}</td>
          <td>
            <select name="" class="form-control">
              <option value="" v-for="teacher in discipline.teachers">{{ teacher.name }}</option>
            </select>
          </td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "AdminDisciplines",
  data() {
    return {
      disciplines: "",
      disciplineName: "",
      disciplineTeacher: "",
      disciplineTeachers: "",
    }
  },
  watch: {
    disciplineTeacher: function (val) {
      if (val) {
        this.getDisciplineTeachers(val);
      } else {
        this.disciplineTeachers = [];
      }
    }
  },
  async mounted() {
    await this.getDisciplines();
  },
  methods: {
    async getDisciplines() {
      const response = await fetch(`http://localhost:8000/api/admin/disciplines?name=${this.disciplineName}&teacher=${this.disciplineTeacher}`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
      });
      if (response.status == 500) {

      } else {
        this.disciplines = await response.json();
        this.disciplines = this.disciplines.data;
      }
    },
    async getDisciplineTeachers(val) {
      console.log('ues');
      const response = await fetch(`http://localhost:8000/api/admin/users?name=${val}`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
      });
      if (response.status == 500) {

      } else {
        this.disciplineTeachers = await response.json();
        this.disciplineTeachers = this.disciplineTeachers.data;
      }
    },
    selectTeacher(teacherName) {
      this.disciplineTeacher = teacherName;
      this.disciplineTeachers = '';
    }
  }
}
</script>

<style scoped>

</style>
