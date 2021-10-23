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
        <div class="ml-2">
          <button class="btn btn-success" @click="openDisciplineCreateModal"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-file-earmark-plus-fill" viewBox="0 0 16 16">
            <path d="M9.293 0H4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V4.707A1 1 0 0 0 13.707 4L10 .293A1 1 0 0 0 9.293 0zM9.5 3.5v-2l3 3h-2a1 1 0 0 1-1-1zM8.5 7v1.5H10a.5.5 0 0 1 0 1H8.5V11a.5.5 0 0 1-1 0V9.5H6a.5.5 0 0 1 0-1h1.5V7a.5.5 0 0 1 1 0z"/>
          </svg></button>
        </div>
      </div>
      <br>
      <table class="table table-striped table-sm">
        <thead>
        <tr>
          <th>#</th>
          <th>Название</th>
          <th>Описание</th>
          <th>Преподаватели</th>
          <th></th>
          <th></th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="discipline in disciplines">
          <td></td>
          <td>{{ discipline.name }}</td>
          <td>{{ discipline.description }}</td>
          <td>
            <select name="" class="form-control">
              <option value="" v-for="teacher in discipline.teachers">{{ teacher.name }}</option>
            </select>
          </td>
          <td><button class="btn btn-primary" @click="openUpdateModal(discipline.id)"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pen-fill" viewBox="0 0 16 16">
            <path d="m13.498.795.149-.149a1.207 1.207 0 1 1 1.707 1.708l-.149.148a1.5 1.5 0 0 1-.059 2.059L4.854 14.854a.5.5 0 0 1-.233.131l-4 1a.5.5 0 0 1-.606-.606l1-4a.5.5 0 0 1 .131-.232l9.642-9.642a.5.5 0 0 0-.642.056L6.854 4.854a.5.5 0 1 1-.708-.708L9.44.854A1.5 1.5 0 0 1 11.5.796a1.5 1.5 0 0 1 1.998-.001z"/>
          </svg></button></td>
          <td><button class="btn btn-danger" @click="openDeleteModal(discipline.id)"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-file-earmark-x-fill" viewBox="0 0 16 16">
            <path d="M9.293 0H4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V4.707A1 1 0 0 0 13.707 4L10 .293A1 1 0 0 0 9.293 0zM9.5 3.5v-2l3 3h-2a1 1 0 0 1-1-1zM6.854 7.146 8 8.293l1.146-1.147a.5.5 0 1 1 .708.708L8.707 9l1.147 1.146a.5.5 0 0 1-.708.708L8 9.707l-1.146 1.147a.5.5 0 0 1-.708-.708L7.293 9 6.146 7.854a.5.5 0 1 1 .708-.708z"/>
          </svg></button></td>
        </tr>
        </tbody>
      </table>
    </div>
    <b-modal id="bv-modal-example"  v-model="showDeleteModal" hide-footer>
      <template #modal-title>
        Удаление дисциплины
      </template>
      <div class="d-block text-center">
        Вы действительно хотите удалить дисциплину?
      </div>
      <div class="row text-center">
        <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-danger" block @click="deleteDiscipline">Удалить дисциплину</b-button>
        <b-button class="col-5 ml-2 mt-3" block @click="$bvModal.hide('bv-modal-example')">Отмена</b-button>
      </div>
    </b-modal>
    <b-modal id="bv-modal-example" v-model="show" hide-footer>
      <template #modal-title>
        Создание дисциплины
      </template>
      <div class="d-block text-center">
        <div class="" v-if="errors.name">
          <div class="text-danger">{{ errors.name }}</div>
        </div>
        <input type="text" placeholder="Введите название дисциплины" v-model="disciplineNameForCreating" class="form-control">
        <div class="" v-if="errors.description">
          <div class="text-danger">{{ errors.description }}</div>
        </div>
        <textarea placeholder="Введите описание" v-model="disciplineDescriptionForCreating" class="form-control mt-2" id="" cols="30" rows="10"></textarea>
        <div class="" v-if="errors.department">
          <div class="text-danger">{{ errors.department }}</div>
        </div>
        <select v-model="departmentIdForCreating" class="form-control mt-2">
          <option value="" disabled>Выберите отделение</option>
          <option :value="department.id" v-for="department in departments">{{ department.name }}</option>
        </select>
      </div>
      <div class="row text-center">
        <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-success" block @click="createDiscipline">Создать дисциплину</b-button>
        <b-button class="col-5 ml-2 mt-3" block @click="closeCreateDisciplineModal">Отмена</b-button>
      </div>
    </b-modal>
    <b-modal id="bv-modal-example" v-model="showUpdateModal" hide-footer>
      <template #modal-title>
        Изменение дисциплины
      </template>
      <div class="d-block text-center">
        <div class="" v-if="errors.name">
          <div class="text-danger">{{ errors.name }}</div>
        </div>
        <input type="text" placeholder="Введите название дисциплины" v-model="disciplineNameForCreating" class="form-control">
        <div class="" v-if="errors.description">
          <div class="text-danger">{{ errors.description }}</div>
        </div>
        <textarea placeholder="Введите описание" v-model="disciplineDescriptionForCreating" class="form-control mt-2" cols="30" rows="10"></textarea>
        <div class="" v-if="errors.department">
          <div class="text-danger">{{ errors.department }}</div>
        </div>
        <select v-model="departmentIdForCreating" class="form-control mt-2">
          <option value="" disabled>Выберите отделение</option>
          <option :value="department.id" v-for="department in departments">{{ department.name }}</option>
        </select>
      </div>
      <div class="row text-center">
        <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-success" block @click="updateDiscipline">Измениты дисциплину</b-button>
        <b-button class="col-5 ml-2 mt-3" block @click="closeDisciplineUpdateModal">Отмена</b-button>
      </div>
    </b-modal>
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
      disciplineNameForCreating: '',
      departmentIdForCreating: '',
      disciplineDescriptionForCreating: '',
      departments: '',
      show: false,
      showDeleteModal: false,
      showUpdateModal: false,
      disciplineId: '',
      errors: '',
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
    await this.getDepartments();
  },
  methods: {
    async getDisciplines() {
      const response = await fetch(`http://laravel.test/api/admin/disciplines?name=${this.disciplineName}&teacher=${this.disciplineTeacher}`, {
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
      const response = await fetch(`http://laravel.test/api/admin/users?name=${val}`, {
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
    },

    async getDepartments() {
      const response = await fetch(`http://laravel.test/api/admin/departments`, {
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
    async createDiscipline() {
      const response = await fetch(`http://laravel.test/api/admin/disciplines`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
        body: JSON.stringify({
          'name': this.disciplineNameForCreating,
          'department_id': this.departmentIdForCreating,
          'description': this.disciplineDescriptionForCreating
        })
      })
      const responseData = await response.json();
      if (response.status == 422) {
        this.errors = new Object();
        const errorsName = responseData.errors.name
        const errorsDescription = responseData.errors.description
        const errorsDepartment = responseData.errors.department_id
        this.errors.name = errorsName ? errorsName[0] : null
        this.errors.description = errorsDescription ? errorsDescription[0] : null
        this.errors.department = errorsDepartment ? errorsDepartment[0]: null
      } else {
        this.disciplines.push(responseData.data);
        this.show = false
        this.resetData();
      }
    },
    openDeleteModal(id) {
      this.disciplineId = id;
      this.showDeleteModal = true;
    },
    async deleteDiscipline() {
      const response = await fetch(`http://laravel.test/api/admin/disciplines/${this.disciplineId}`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
      })
      this.showDeleteModal = false;
      for(let i = 0; i < this.disciplines.length; i++) {
        if(this.disciplines[i].id == this.disciplineId) {
          this.disciplines.splice(i, 1);
          break;
        }
      }
    },
    async openUpdateModal(disciplineId) {
      this.showUpdateModal = true;
      this.disciplineId = disciplineId
      for(let i = 0; i < this.disciplines.length; i++) {
        if(this.disciplines[i].id == this.disciplineId) {
          this.disciplineNameForCreating = this.disciplines[i].name;
          this.disciplineDescriptionForCreating = this.disciplines[i].description
          break;
        }
      }
    },
    async updateDiscipline() {
      const response = await fetch(`http://laravel.test/api/admin/disciplines/update/${this.disciplineId}`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
        body: JSON.stringify({
          'name': this.disciplineNameForCreating,
          'department_id': this.departmentIdForCreating,
          'description': this.disciplineDescriptionForCreating
        })
      })
      const responseData = await response.json();
      if (response.status == 422) {
        this.errors = new Object();
        const errorsName = responseData.errors.name
        const errorsDescription = responseData.errors.description
        const errorsDepartment = responseData.errors.department_id
        this.errors.name = errorsName ? errorsName[0] : null
        this.errors.description = errorsDescription ? errorsDescription[0] : null
        this.errors.department = errorsDepartment ? errorsDepartment[0]: null
      } else {
        this.showUpdateModal = false
        for(let i = 0; i < this.disciplines.length; i++) {
          if(this.disciplines[i].id == this.disciplineId) {
            this.disciplines[i].name = responseData.data.name;
            this.disciplines[i].description = responseData.data.description;
            this.disciplines[i].department_id = responseData.data.department_id;
            break;
          }
        }
        this.resetData();
      }
    },
    resetData()
    {
      this.departmentIdForCreating = '';
      this.disciplineDescriptionForCreating = '';
      this.disciplineNameForCreating = '';
      this.errors = '';
    },
    closeDisciplineUpdateModal()
    {
      this.showUpdateModal = false;
      this.resetData();
    },
    closeCreateDisciplineModal()
    {
      this.show = false;
      this.resetData();
    },
    openDisciplineCreateModal()
    {
      this.show = true;
      this.resetData()
    }
  }
}
</script>

<style scoped>

</style>
