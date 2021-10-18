<template>
  <body>
  <div class="d-flex flex-column flex-md-row align-items-center p-3 px-md-4 mb-3 bg-white border-bottom box-shadow">
    <h5 class="my-0 mr-md-auto font-weight-normal"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-badge-ad-fill" viewBox="0 0 16 16">
      <path d="M11.35 8.337c0-.699-.42-1.138-1.001-1.138-.584 0-.954.444-.954 1.239v.453c0 .8.374 1.248.972 1.248.588 0 .984-.44.984-1.2v-.602zm-5.413.237-.734-2.426H5.15l-.734 2.426h1.52z"/>
      <path d="M2 2a2 2 0 0 0-2 2v8a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2H2zm6.209 6.32c0-1.28.694-2.044 1.753-2.044.655 0 1.156.294 1.336.769h.053v-2.36h1.16V11h-1.138v-.747h-.057c-.145.474-.69.804-1.367.804-1.055 0-1.74-.764-1.74-2.043v-.695zm-4.04 1.138L3.7 11H2.5l2.013-5.999H5.9L7.905 11H6.644l-.47-1.542H4.17z"/>
    </svg><NuxtLink to="/" class="p-2 text-dark" href="#">Department</NuxtLink></h5>
    <div class="">
      <NuxtLink class="btn btn-outline-primary ml-2" to="/">Выйти из профиля</NuxtLink>
    </div>
  </div>

  <div class="position-relative overflow-hidden p-2 bg-light">
    <div class="text-center">
      <div class="col-md-5 p-lg-5 mx-auto my-5">
        <img class="rounded-circle" :src="user.avatar" width="250" alt="">
        <div class="" v-if="errors.avatar">
          <span class="text-center text-danger">{{ errors.avatar }}</span>
        </div>
        <div class="row">
          <input type="file" @change="onAttachmedChange" class="form-control mt-2 col-8 mr-2">
          <button type="submit" class="btn btn-primary mt-2" @click="updateUserFace">Загрузить</button>
        </div>
        <h1 class="display-4 font-weight-normal">{{ user.name }}</h1>
        <p class="lead font-weight-normal">{{ user.email }}</p>
        <h6>Хотите изменить имя?</h6>
        <div class="row">
          <div class="" v-if="errors.name">
            <label for="userNameField">
              <span class="text-center text-danger">{{ errors.name }}</span>
            </label>
          </div>
          <input type="text" class="form-control col-8 mr-2" id="userNameField" placeholder="Введите новое имя" v-model="userName">
          <button @click="updateUserProfile" class="btn btn-outline-secondary">Изменить</button>
        </div>
      </div>
      <div class="product-device box-shadow d-none d-md-block"></div>
    </div>
    <b-modal id="bv-modal-example" v-model="showStudentModal" hide-footer>
      <template #modal-title>
        Студенческая информация
      </template>
      <div class="d-block text-center">
        <div class="" v-if="errors.studentsCourse">
          <div class="text-danger">{{ errors.studentsCourse }}</div>
        </div>
        <input type="number" placeholder="Введите курс обучения" v-model="studentCourse" class="form-control mt-2">
        <div class="" v-if="errors.studentId">
          <div class="text-danger">{{ errors.studentId }}</div>
        </div>
        <input type="text" placeholder="Введите номер студенческого" v-model="studentId" class="form-control mt-2">
        <div class="" v-if="errors.studentGroup">
          <div class="text-danger">{{ errors.studentGroup }}</div>
        </div>
        <input placeholder="Введите группу" v-model="studentGroup" class="form-control mt-2">
        <div class="" v-if="errors.studentDepartmentId">
          <div class="text-danger">{{ errors.studentDepartmentId }}</div>
        </div>
        <select v-model="studentDepartmentId" class="form-control mt-2">
          <option value="" disabled>Выберите отделение</option>
          <option :value="department.id" v-for="department in departments">{{ department.name }}</option>
        </select>
      </div>
      <div class="row text-center">
        <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-success" block @click="updateStudentParams">Изменить</b-button>
        <b-button class="col-5 ml-2 mt-3" block @click="closeStudentParamModal">Отмена</b-button>
      </div>
    </b-modal>
    <div class="" v-if="user.role == 'STUDENT'">
      <h3>Студент</h3>
      <button @click="openStudentInfo" class="btn btn-primary">Посмотреть вашу студенческую информацию</button>
    </div>
    <div class="" v-if="user.role == 'TEACHER'">
      <h3>Преподаватель</h3>
      <NuxtLink class="btn btn-primary" :to=`/teachers/${user.id}`>Перейди на страницу</NuxtLink>
    </div>
    <div class="" v-if="user.role == 'ADMIN'">
      <h3>Админ</h3>
      <NuxtLink class="btn btn-primary" to="/admin/">Административная панель</NuxtLink>
    </div>
  </div>
  </body>
</template>

<script>
export default {
  name: "Profile",
  data() {
    return {
      user: '',
      userName: '',
      userAvatar: "",
      errors: '',
      showStudentModal: false,
      studentCourse: '',
      studentDepartmentId: '',
      departments: '',
      studentGroup: '',
      studentId: ''
    }
  },
  async mounted() {
    await this.getUser();
    await this.getDepartments();
  },
  methods: {
    async getUser() {
      const response = await fetch('http://localhost:8000/api/user', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt'),
        },
      });
      if (response.ok) {
        this.user = await response.json();
        this.user = this.user.data;
        if (this.user.role == "STUDENT") {
          await this.getStudentParams();
        }
      } else if (response.status == 401) {
        this.$router.push('/errors/unauthenticated');
      }
    },
    async updateUserProfile()
    {
      const response = await fetch('http://localhost:8000/api/user/profile/update', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt'),
        },
        method: "post",
        body: JSON.stringify({
          name: this.userName,
        })
      })
      const responseData = await response.json();
      this.errors = new Object();
      if (response.ok) {
        this.user = responseData.data;
        this.resetData();
      } else if (response.status == 422) {
        console.log(responseData)
        const errors = responseData.errors;
        this.errors.name = errors.name ? errors.name[0] : null;
      }
    },
    async updateUserFace()
    {
      if (this.userAvatar) {
        const formData = new FormData();
        formData.append('avatar', this.userAvatar);
        const response = await fetch('http://localhost:8000/api/user/avatar/upload', {
          headers: {
            'Authorization': "Bearer " + localStorage.getItem('jwt'),
          },
          method: "post",
          body: formData
        })
        let responseData = await response.json()
        if (response.ok) {
          this.user = responseData.data;
        }
      } else {
        this.errors = new Object();
        this.errors.avatar = 'Передайте аватар';
      }
    },
    onAttachmedChange(e)
    {
      this.userAvatar = e.target.files[0]
    },
    resetData() {
      this.errors = '';
      this.userAvatar = '';
      this.userName = '';
    },
    async getDepartments() {
      const response = await fetch(`http://localhost:8000/api/departments`, {
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
    async openStudentInfo()
    {
      this.showStudentModal = true;
      const response = await fetch(`http://localhost:8000/api/students/params`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
      });
      const responseData = await response.json();
      if (response.status == 422) {

      } else {
        if (responseData) {

        }
      }
    },
    async updateStudentParams()
    {
      const response = await fetch('http://localhost:8000/api/students/params/update', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt'),
        },
        method: "post",
        body: JSON.stringify({
          params: {
            1: {
              name: 'COURSE',
              value: `${this.studentCourse}`
            },
            2: {
              name: "DEPARTMENT_ID",
              value: `${this.studentDepartmentId}`
            },
            3: {
              name: "GROUP",
              value: `${this.studentGroup}`
            },
            4: {
              name: "STUDENT_ID",
              value: `${this.studentId}`
            }
          }
        })
      })
      let responseData = await response.json();
      this.errors = new Object();
      if (response.ok) {
        responseData = responseData.data;
        this.updateParams(responseData);
        this.closeStudentParamModal();
      } else if (response.status == 422) {
        console.log(responseData)
        const errors = responseData.errors;
        this.errors.name = errors.name ? errors.name[0] : null;
      }
    },
    closeStudentParamModal() {
      this.showStudentModal = false;
    },
    async getStudentParams() {
      const response = await fetch('http://localhost:8000/api/students/params', {
        headers: {
          'Content-Type': "application/json",
          'Accept': "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        }
      })

      let responseData = await response.json();
      responseData = responseData.data;
      if (responseData) {
        this.updateParams(responseData)
      }
    },
    updateParams(responseData) {
      for(let i = 0; i < responseData.length; i++) {
        if (responseData[i].name == "STUDENT_ID") {
          this.studentId = responseData[i].value;
        }
        if (responseData[i].name == 'DEPARTMENT_ID') {
          this.studentDepartmentId = responseData[i].value;
        }
        if (responseData[i].name == "COURSE") {
          this.studentCourse = responseData[i].value;
        }
        if (responseData[i].name == 'GROUP') {
          this.studentGroup = responseData[i].value
        }
      }
    }
  }
}
</script>

<style scoped>

</style>
