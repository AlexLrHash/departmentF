<template>
  <div class="">
    <AdminHeader></AdminHeader>
    <br>
    <div class="container mt-5">
    <div class="p-3 p-md-5 text-white rounded bg-dark row">
      <div class="col-6">
        <h1 class="display-4 font-italic">{{ user.name }}</h1>
        <p class="lead my-3">{{ user.email }}</p>
        <div>Дисциплины:
          <table class="table table-striped table-sm">
            <thead>
            <tr>
              <th>Название</th>
              <th>Количество лекций</th>
              <th>Количество парктик</th>
              <th></th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="userDiscipline in user.disciplines">
              <td>{{ userDiscipline.name }}</td>
              <td>{{ userDiscipline.number_of_labs  }}</td>
              <td>{{ userDiscipline.number_of_practices }}</td>
              <td>{{ userDiscipline.description  }}</td>
              <td>
                <button @click="removeDiscipline(userDiscipline.id)" class="btn btn-danger"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-journal-minus" viewBox="0 0 16 16">
                <path fill-rule="evenodd" d="M5.5 8a.5.5 0 0 1 .5-.5h4a.5.5 0 0 1 0 1H6a.5.5 0 0 1-.5-.5z"/>
                <path d="M3 0h10a2 2 0 0 1 2 2v12a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2v-1h1v1a1 1 0 0 0 1 1h10a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H3a1 1 0 0 0-1 1v1H1V2a2 2 0 0 1 2-2z"/>
                <path d="M1 5v-.5a.5.5 0 0 1 1 0V5h.5a.5.5 0 0 1 0 1h-2a.5.5 0 0 1 0-1H1zm0 3v-.5a.5.5 0 0 1 1 0V8h.5a.5.5 0 0 1 0 1h-2a.5.5 0 0 1 0-1H1zm0 3v-.5a.5.5 0 0 1 1 0v.5h.5a.5.5 0 0 1 0 1h-2a.5.5 0 0 1 0-1H1z"/>
              </svg></button></td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
      <div class="col-6">
        <div>
          <div class="table-responsive">
            <table class="table table-striped table-sm">
              <thead>
              <tr>
                <th>Название</th>
                <th>Описание</th>
                <th></th>
              </tr>
              </thead>
              <tbody>
              <tr v-for="discipline in disciplines">
                <td>{{ discipline.name }}</td>
                <td>{{ discipline.description  }}</td>
                <td>
                  <b-button id="show-btn" class="btn btn-success" @click="openModal(discipline.id)">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-person-plus-fill" viewBox="0 0 16 16">
                      <path d="M1 14s-1 0-1-1 1-4 6-4 6 3 6 4-1 1-1 1H1zm5-6a3 3 0 1 0 0-6 3 3 0 0 0 0 6z"/>
                      <path fill-rule="evenodd" d="M13.5 5a.5.5 0 0 1 .5.5V7h1.5a.5.5 0 0 1 0 1H14v1.5a.5.5 0 0 1-1 0V8h-1.5a.5.5 0 0 1 0-1H13V5.5a.5.5 0 0 1 .5-.5z"/>
                    </svg>
                  </b-button>
                </td>
              </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <div>
        <div>Количество лекций: {{ user.common_number_of_labs }}</div>
        <div>Количество практик: {{ user.common_number_of_practices }}</div>
        <div>Всего: {{ user.common_number_of_practices + user.common_number_of_labs }}</div>
      </div>
    </div>
    <div>
      <div>
        <b-modal id="bv-modal-example"  v-model="show" hide-footer>
          <template #modal-title>
            Добавление дисциплины
          </template>
          <div class="d-block text-center">
            <td>
              <div v-if="errors.number_of_labs" class="">
                <span class="text-danger">
                  {{ errors.number_of_labs }}
                </span>
              </div>
              <input type="number" class="form-control" placeholder="Введите количество лаб" v-model="disciplineNumberOfLabs">
            </td>
            <td>
              <div v-if="errors.number_of_practices" class="">
                <span class="text-danger">
                  {{ errors.number_of_practices }}
                </span>
              </div>
              <input type="number" class="form-control" placeholder="Введите количество практик" v-model="disciplineNumberOfPractices">
            </td>
          </div>
          <div class="row text-center">
            <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-success" block @click="addDiscipline(disciplineId)">Добавить дисциплину</b-button>
            <b-button class="col-5 ml-2 mt-3" block @click="$bvModal.hide('bv-modal-example')">Отмена</b-button>
          </div>
        </b-modal>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import AdminHeader from "../../../../components/AdminHeader";
export default {
  name: "_id",
  components: {AdminHeader},
  data() {
    return {
      user: "",
      disciplines: "",
      disciplineNumberOfPractices: '',
      disciplineNumberOfLabs: '',
      errors: '',
      disciplineId: '',
      show: false
    }
  },
  async mounted() {
    await this.getTeachers();
    await this.getDisciplines();
  },
  methods: {
    async getTeachers() {
      const response = await fetch('http://localhost:8000/api/admin/users/' + this.$route.params.id, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
      });
      if (response.status == 500) {

      } else {
        this.user = await response.json();
        this.user = this.user.data;
      }
    },
    async getDisciplines() {
      const response = await fetch('http://localhost:8000/api/admin/disciplines', {
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
    async addDiscipline(id) {
      const response = await fetch('http://localhost:8000/api/admin/teachers/' + this.user.id + '/disciplines/' + id + '/add', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: "POST",
        body: JSON.stringify({
          'number_of_practices': this.disciplineNumberOfPractices,
          'number_of_labs': this.disciplineNumberOfLabs
        })
      });
      let responseData = await response.json();
      if (response.status == 422) {
        this.errors = new Object();
        const errorNumberOfLabs = responseData.errors.number_of_labs;
        const errorNumberOfPractices = responseData.errors.number_of_practices;
        this.errors.number_of_labs = errorNumberOfLabs ? errorNumberOfLabs[0] : null;
        this.errors.number_of_practices = errorNumberOfPractices ? errorNumberOfPractices[0] : null
      } else {
        this.user = responseData
        this.user = this.user.data;
        this.show = false;
        this.disciplineNumberOfLabs = '';
        this.disciplineNumberOfPractices = '';
        this.errors = '';
      }
    },
    async removeDiscipline(id) {
      const response = await fetch('http://localhost:8000/api/admin/teachers/' + this.user.id + '/disciplines/' + id + '/remove', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: "POST"
      });
      if (response.status == 500) {

      } else {
        this.user = await response.json();
        this.user = this.user.data;
      }
    },
    async openModal(id)
    {
      this.show = true;
      this.disciplineId = id;
    }
  }
}
</script>

<style scoped>

</style>
