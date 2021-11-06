<template>
  <div class="">
    <NuxtLink to="/groups" class="btn btn-primary">Вернуться назад</NuxtLink>
    <div class="pricing-header px-3 py-3 pt-md-5 pb-md-4 mx-auto text-center">
      <h1 class="display-4">{{ group.name }}</h1>
      <p class="lead">{{ group.description }}</p>
    </div>
    <div class="row">
      <nav class="col-md-3 d-none d-md-block bg-light sidebar">
        <div class="sidebar-sticky">
          <h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-muted">
            <span>Студенты</span>
            <a class="d-flex align-items-center text-muted" href="#" style="margin-left: 10px">
            </a>
          </h6>
          <button v-if="user.role=='TEACHER'" @click="showAddStudentModal = true" class="btn btn-success ml-3">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                 class="bi bi-person-plus-fill" viewBox="0 0 16 16">
              <path d="M1 14s-1 0-1-1 1-4 6-4 6 3 6 4-1 1-1 1H1zm5-6a3 3 0 1 0 0-6 3 3 0 0 0 0 6z"/>
              <path fill-rule="evenodd"
                    d="M13.5 5a.5.5 0 0 1 .5.5V7h1.5a.5.5 0 0 1 0 1H14v1.5a.5.5 0 0 1-1 0V8h-1.5a.5.5 0 0 1 0-1H13V5.5a.5.5 0 0 1 .5-.5z"/>
            </svg>
            Добавить студента
          </button>
          <br>
          <h5 class="nav-link active">Все студенты группы</h5>
          <div class="" v-if="group.students && group.students.length != 0">
            <ul class="nav flex-column" v-for="student in group.students">
              <li class="nav-item ml-3">
                <span>{{ student.name }} <button v-if="user.role=='TEACHER'" @click="showDeleteStudentModal = true; studentId = student.id" class="btn btn-danger"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-person-x-fill" viewBox="0 0 16 16">
                  <path fill-rule="evenodd" d="M1 14s-1 0-1-1 1-4 6-4 6 3 6 4-1 1-1 1H1zm5-6a3 3 0 1 0 0-6 3 3 0 0 0 0 6zm6.146-2.854a.5.5 0 0 1 .708 0L14 6.293l1.146-1.147a.5.5 0 0 1 .708.708L14.707 7l1.147 1.146a.5.5 0 0 1-.708.708L14 7.707l-1.146 1.147a.5.5 0 0 1-.708-.708L13.293 7l-1.147-1.146a.5.5 0 0 1 0-.708z"/>
                </svg></button>
                </span>
              </li>
            </ul>
            <br>
          </div>
          <div v-else class="ml-4 mb-2">
            Тут пока пусто
          </div>
        </div>
      </nav>
      <div class="col-md-8">
          <h3>Все задания</h3>
          <button class="btn btn-success" @click="showAddTaskModal = true" v-if="user.role=='TEACHER'"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-file-earmark-plus-fill" viewBox="0 0 16 16">
            <path d="M9.293 0H4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V4.707A1 1 0 0 0 13.707 4L10 .293A1 1 0 0 0 9.293 0zM9.5 3.5v-2l3 3h-2a1 1 0 0 1-1-1zM8.5 7v1.5H10a.5.5 0 0 1 0 1H8.5V11a.5.5 0 0 1-1 0V9.5H6a.5.5 0 0 1 0-1h1.5V7a.5.5 0 0 1 1 0z"/>
          </svg> Добавить задачу</button>
          <div class="mt-2" v-if="group.tasks && group.tasks.length != 0">
              <div v-for="task in group.tasks">
                <div class="card flex-md-row mb-4 box-shadow h-md-250">
                  <div class="card-body d-flex flex-column align-items-start">
                    <h3 class="mb-0">
                      <a class="text-dark" href="#">{{ task.name }}</a>
                    </h3>
                    <div class="mb-1 text-muted">{{ task.expired_at }}</div>
                    <p class="card-text mb-auto">{{ task.description }}</p>
                  </div>
                  <button class="btn btn-danger" v-if="user.role=='TEACHER'" @click="showRemoveTaskModal = true; taskId = task.id"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" class="bi bi-file-earmark-minus-fill" viewBox="0 0 16 16">
                    <path d="M9.293 0H4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V4.707A1 1 0 0 0 13.707 4L10 .293A1 1 0 0 0 9.293 0zM9.5 3.5v-2l3 3h-2a1 1 0 0 1-1-1zM6 8.5h4a.5.5 0 0 1 0 1H6a.5.5 0 0 1 0-1z"/>
                  </svg></button>
                </div>
              </div>
            </div>
          <div class="" v-else>
            Тут пока пусто
          </div>
      </div>

    </div>
    <b-modal id="bv-modal-example" v-model="showAddStudentModal" hide-footer>
      <template #modal-title>
        Добавление студента
      </template>
      <div class="d-block text-center">
        <div class="" v-if="errors.number">
          <div class="text-danger">{{ errors.number }}</div>
        </div>
        <input type="text" placeholder="Введите номер студента" v-model="numberOfStudentForAdditing"
               class="form-control">
      </div>
      <div class="row text-center">
        <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-success" block @click="addStudent">Добавить</b-button>
        <b-button class="col-5 ml-2 mt-3" block @click="closeAddStudentModal">Отмена</b-button>
      </div>
    </b-modal>

    <b-modal id="bv-modal-example" v-model="showDeleteStudentModal" hide-footer>
      <template #modal-title>
        Удаление студента
      </template>
      <div class="d-block text-center">
        Вы действительно хотите удалить?
      </div>
      <div class="row text-center">
        <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-danger" block @click="removeStudent">Удалить</b-button>
        <b-button class="col-5 ml-2 mt-3" block @click="closeRemoveStudentModal">Отмена</b-button>
      </div>
    </b-modal>

    <b-modal id="bv-modal-example" v-model="showAddTaskModal" hide-footer>
      <template #modal-title>
        Добавление задачи
      </template>
      <div class="d-block text-center">
        <div class="" v-if="errors.name">
          <div class="text-danger">{{ errors.name }}</div>
        </div>
        <input type="text" placeholder="Введите название" v-model="taskNameForCreating"
               class="form-control mt-2">
        <div class="" v-if="errors.description">
          <div class="text-danger">{{ errors.description }}</div>
        </div>
        <textarea type="text" placeholder="Введите описание" rows="10" cols="30" v-model="taskDescriptionForCreating"
               class="form-control mt-2"></textarea>
        <div class="" v-if="errors.expired_at">
          <div class="text-danger">{{ errors.expired_at }}</div>
        </div>
        <b-form-datepicker v-model="taskExpiredAtForCreating" placeholder="Введите дату окончания" class="mt-2 mb-2"></b-form-datepicker>
      </div>
      <div class="row text-center">
        <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-success" block @click="addTask">Добавить</b-button>
        <b-button class="col-5 ml-2 mt-3" block @click="closeAddTaskModal">Отмена</b-button>
      </div>
    </b-modal>

    <b-modal id="bv-modal-example" v-model="showRemoveTaskModal" hide-footer>
      <template #modal-title>
        Удаление задачи
      </template>
      <div class="d-block text-center">
        Вы действительно хотите удалить?
      </div>
      <div class="row text-center">
        <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-danger" block @click="removeTask">Удалить</b-button>
        <b-button class="col-5 ml-2 mt-3" block @click="closeRemoveTaskModal">Отмена</b-button>
      </div>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "GroupId",
  data() {
    return {
      group: '',
      showAddStudentModal: false,
      errors: '',
      numberOfStudentForAdditing: '',
      showDeleteStudentModal: false,
      showAddTaskModal: false,
      studentId: '',
      taskNameForCreating: '',
      taskDescriptionForCreating: '',
      taskExpiredAtForCreating: '',
      showRemoveTaskModal: false,
      taskId: '',
      user: ''
    }
  },
  async mounted() {
    await this.getUser()

    if (this.user.role == "TEACHER") {
      await this.getGroup()
    } else {
      await this.getUserGroup()
    }
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
    async getGroup() {
      const response = await fetch(`http://department.biz/api/groups/${this.$route.params.id}`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt'),
        },
      });
      if (response.ok) {
        this.group = await response.json();
        this.group = this.group.data;
      } else if (response.status == 401) {
        this.$router.push('/errors/unauthenticated');
      } else if (response.status == 403) {
        this.$router.push('/errors/permissions');
      } else if (response.status == 404) {
        this.$router.push('/errors/notfound');
      }
    },
    async getUserGroup() {
      const response = await fetch(`http://department.biz/api/student/groups/${this.$route.params.id}`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt'),
        },
      });
      if (response.ok) {
        this.group = await response.json();
        this.group = this.group.data;
      } else if (response.status == 401) {
        this.$router.push('/errors/unauthenticated');
      } else if (response.status == 403) {
        this.$router.push('/errors/permissions');
      } else if (response.status == 404) {
        this.$router.push('/errors/notfound');
      }
    },
    async addStudent() {
      const response = await fetch(`http://department.biz/api/groups/${this.$route.params.id}/students/add`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
        body: JSON.stringify({
          'number': this.numberOfStudentForAdditing,
        })
      })
      const responseData = await response.json();
      if (response.status == 422) {
        this.errors = new Object();
        if (responseData.errors) {
          const errorsNumber = responseData.errors.number
          this.errors.number = errorsNumber ? errorsNumber[0] : null
        } else {
          this.errors.number = responseData.message;
        }
      } else {

        this.group.students.push(responseData.data);
        this.showAddStudentModal = false
        this.resetData();
      }
    },
    async removeStudent() {
      const response = await fetch(`http://department.biz/api/groups/${this.$route.params.id}/students/remove`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
        body: JSON.stringify({
          'student_id': this.studentId,
        })
      })
      const responseData = await response.json();
      if (response.status == 422) {
        this.errors = new Object();
        if (responseData.errors) {
        } else {
        }
      } else {
        for (let i = 0; i < this.group.students.length; i++) {
          if (this.group.students[i].id == this.studentId) {
            this.group.students.splice(i, 1);
          }
        }
        this.showDeleteStudentModal = false
        this.resetData();
      }
    },
    async removeTask() {
      const response = await fetch(`http://department.biz/api/groups/${this.$route.params.id}/tasks/remove`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
        body: JSON.stringify({
          'task_id': this.taskId,
        })
      })
      const responseData = await response.json();
      if (response.status == 422) {
        this.errors = new Object();
        if (responseData.errors) {
        } else {
        }
      } else {
        for (let i = 0; i < this.group.tasks.length; i++) {
          if (this.group.tasks[i].id == this.taskId) {
            this.group.tasks.splice(i, 1);
          }
        }
        this.showRemoveTaskModal = false
        this.resetData();
      }
    },
    resetData() {
      this.errors = '';
      this.numberOfStudentForAdditing = '';
    },
    closeAddStudentModal() {
      this.resetData();
      this.showAddStudentModal = false;
    },
    closeRemoveStudentModal() {
      this.resetData();
      this.showDeleteStudentModal = false;
    },
    async addTask() {
      const response = await fetch(`http://department.biz/api/groups/${this.$route.params.id}/tasks/add`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
        body: JSON.stringify({
          'name': this.taskNameForCreating,
          'description': this.taskDescriptionForCreating,
          'expired_at': this.taskExpiredAtForCreating
        })
      })
      const responseData = await response.json();
      if (response.status == 422) {
        this.errors = new Object();
        const errorsName = responseData.errors.name
        const errorsDescription = responseData.errors.description
        const errorsExpiredAt = responseData.errors.expired_at
        this.errors.name = errorsName ? errorsName[0] : null
        this.errors.description = errorsDescription ? errorsDescription[0] : null;
        this.errors.expired_at = errorsExpiredAt ? errorsExpiredAt[0] : null;
      } else {
        this.group.tasks.push(responseData.data);
        this.showAddTaskModal = false
        this.resetData();
      }
    },
    closeAddTaskModal() {
      this.resetData();
      this.showAddTaskModal = false;
    },
    closeRemoveTaskModal() {
      this.resetData();
      this.showRemoveTaskModal = false
    }
  }
}
</script>

<style scoped>

</style>
