<template>
  <div class="">
    <div class="jumbotron p-3 p-md-5 text-white rounded bg-dark">
      <div class="col-md-6 px-0">
        <h1 class="display-4 font-italic">Группы</h1>
        <p class="lead my-3">Создавайте свои собственные группы, добавляейте в них учащихся и давайте им задания.</p>
        <p class="lead mb-0"><a href="https://ru.wikipedia.org/wiki/Google_%D0%9A%D0%BB%D0%B0%D1%81%D1%81"
                                class="text-white font-weight-bold">Подробнее...</a></p>
      </div>
      <br>
      <button class="btn btn-success" @click="showCreateGroupModal = true"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-file-earmark-plus-fill" viewBox="0 0 16 16">
        <path d="M9.293 0H4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V4.707A1 1 0 0 0 13.707 4L10 .293A1 1 0 0 0 9.293 0zM9.5 3.5v-2l3 3h-2a1 1 0 0 1-1-1zM8.5 7v1.5H10a.5.5 0 0 1 0 1H8.5V11a.5.5 0 0 1-1 0V9.5H6a.5.5 0 0 1 0-1h1.5V7a.5.5 0 0 1 1 0z"/>
      </svg> Добавить групу</button>
    </div>

    <div class="row mb-2">
      <div class="col-md-6" v-for="group in groups">
        <div class="card flex-md-row mb-4 box-shadow h-md-250">
          <div class="card-body d-flex flex-column align-items-start">
            <strong class="d-inline-block mb-2 text-primary">{{ group.discipline ? group.discipline.name : ''  }}</strong>
            <h3 class="mb-0">
              <a class="text-dark" href="#">{{ group.name }}</a>
            </h3>
            <div class="mb-1 text-muted">{{ group.created_at }}</div>
            <p class="card-text mb-auto">{{ group.description }}</p>
            <div class="row">
              <NuxtLink class="ml-2 btn btn-primary" :to="`/groups/${group.id}`" href="#">Подробнее</NuxtLink>
              <div v-if="user.role=='TEACHER'">
                <button class="ml-4 btn btn-danger" @click="showDeleteGroupModal = true; groupId = group.id">Удалить группу</button>
              </div>
            </div>
          </div>
          <img class="card-img-right flex-auto d-none d-md-block" data-src="holder.js/200x250?theme=thumb"
               alt="Thumbnail [200x250]" style="width: 200px; height: 250px;"
               src="data:image/svg+xml;charset=UTF-8,%3Csvg%20width%3D%22200%22%20height%3D%22250%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20viewBox%3D%220%200%20200%20250%22%20preserveAspectRatio%3D%22none%22%3E%3Cdefs%3E%3Cstyle%20type%3D%22text%2Fcss%22%3E%23holder_17cf4d06761%20text%20%7B%20fill%3A%23eceeef%3Bfont-weight%3Abold%3Bfont-family%3AArial%2C%20Helvetica%2C%20Open%20Sans%2C%20sans-serif%2C%20monospace%3Bfont-size%3A13pt%20%7D%20%3C%2Fstyle%3E%3C%2Fdefs%3E%3Cg%20id%3D%22holder_17cf4d06761%22%3E%3Crect%20width%3D%22200%22%20height%3D%22250%22%20fill%3D%22%2355595c%22%3E%3C%2Frect%3E%3Cg%3E%3Ctext%20x%3D%2256.20833206176758%22%20y%3D%22131%22%3EThumbnail%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fsvg%3E"
               data-holder-rendered="true">
        </div>
      </div>
    </div>

    <b-modal id="bv-modal-example" v-model="showDeleteGroupModal" hide-footer>
      <template #modal-title>
        Удаление группы
      </template>
      <div class="d-block text-center">
        Вы действительно хотите удалить?
      </div>
      <div class="row text-center">
        <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-danger" block @click="deleteGroup">Удалить</b-button>
        <b-button class="col-5 ml-2 mt-3" block @click="closeRemoveTaskModal">Отмена</b-button>
      </div>
    </b-modal>

    <b-modal id="bv-modal-example" v-model="showCreateGroupModal" hide-footer>
      <template #modal-title>
        Создание группы
      </template>
      <div class="d-block text-center">
        <div class="" v-if="errors.name">
          <div class="text-danger">{{ errors.name }}</div>
        </div>
        <input type="text" placeholder="Введите название группы" v-model="nameForCreating" class="form-control">
        <div class="" v-if="errors.description">
          <div class="text-danger">{{ errors.description }}</div>
        </div>
        <textarea placeholder="Введите описание" v-model="descriptionForCreating" class="form-control mt-2" id="" cols="30" rows="10"></textarea>
        <div class="" v-if="errors.discipline">
          <div class="text-danger">{{ errors.discipline }}</div>
        </div>
        <select v-model="disciplineForCreating" class="form-control mt-2">
          <option value="" disabled>Выберите дисциплину</option>
          <option :value="discipline.id" v-for="discipline in disciplines">{{ discipline.name }}</option>
        </select>
      </div>
      <div class="row text-center">
        <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-success" block @click="createGroup">Создать группу</b-button>
        <b-button class="col-5 ml-2 mt-3" block @click="closeCreateGroupModal">Отмена</b-button>
      </div>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "Group",
  data() {
    return {
      groups: '',
      showCreateGroupModal: false,
      errors: '',
      nameForCreating: '',
      descriptionForCreating: '',
      disciplineForCreating: '',
      disciplines: '',
      showDeleteGroupModal: false,
      groupId: '',
      user: ''
    }
  },
  async mounted()
  {
    await this.getUser();

    if (this.user.role == "STUDENT") {
      await this.getStudentGroups();
    } else {
      await this.getTeacherGroups();
    }

    await this.getDisciplines()
  },
  methods: {
    async getUser() {
      const response = await fetch('http://department.biz/api/user', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt'),
        },
      });
      if (response.ok) {
        this.user = await response.json();
        this.user = this.user.data;
      }
    },
    async getTeacherGroups()
    {
      const response = await fetch('http://department.biz/api/groups', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt'),
        },
      });
      if (response.ok) {
        this.groups = await response.json();
        this.groups = this.groups.data;
      } else if (response.status == 401) {
        this.$router.push('/errors/unauthenticated');
      } else if (response.status == 403) {
        this.$router.push('/errors/permissions');
      }
    },
    async getStudentGroups()
    {
      const response = await fetch('http://department.biz/api/student/groups', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt'),
        },
      });
      if (response.ok) {
        this.groups = await response.json();
        this.groups = this.groups.data;
      } else if (response.status == 401) {
        this.$router.push('/errors/unauthenticated');
      } else if (response.status == 403) {
        this.$router.push('/errors/permissions');
      }
    },
    async getDisciplines() {
      const response = await fetch(`http://department.biz/api/disciplines`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
        },
      });
      if (response.status == 500) {

      } else {
        this.disciplines = await response.json();
        this.disciplines = this.disciplines.data;
      }
    },
    async createGroup()
    {
      const response = await fetch(`http://department.biz/api/groups/create`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
        body: JSON.stringify({
          'name': this.nameForCreating,
          'description': this.descriptionForCreating,
          'discipline_id': this.disciplineForCreating,
        })
      })
      const responseData = await response.json();
      if (response.status == 422) {
        this.errors = new Object();
        const errorsName = responseData.errors.name
        const errorsDescription = responseData.errors.description
        const errorsDiscipline = responseData.errors.discipline_id
        this.errors.name = errorsName ? errorsName[0] : null
        this.errors.description = errorsDescription ? errorsDescription[0] : null
        this.errors.discipline = errorsDiscipline ? errorsDiscipline[0]: null
      } else {
        this.groups.push(responseData.data);
        this.showCreateGroupModal = false
        this.resetData();
      }
    },
    resetData() {
      this.nameForCreating = '';
      this.disciplineForCreating = '';
      this.descriptionForCreating = '';
      this.errors = '';
    },
    closeCreateGroupModal() {
      this.resetData();
      this.showCreateGroupModal = false;
    },
    async deleteGroup() {
      const response = await fetch(`http://department.biz/api/groups/${this.groupId}/delete`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
        body: JSON.stringify({
        })
      })
      const responseData = await response.json();
      if (response.status == 422) {
      } else {
        for (let i = 0; i < this.groups.length; i++) {
          if (this.groups[i].id == this.groupId) {
            this.groups.splice(i, 1);
          }
        }
        this.showDeleteGroupModal = false
        this.resetData();
      }
    },
    closeRemoveTaskModal() {
      this.showRemoveTaskModal = false;
      this.resetData();
    }
  }
}
</script>

<style scoped>

</style>
