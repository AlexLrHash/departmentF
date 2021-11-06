<template>
  <div style="margin-top: 100px;">
    <div class="table-responsive">
      <div class="container row">
        <div class="ml-2">
          <input type="text" v-model="userName" placeholder="Укажите имя" class="form-control">
        </div>
        <div class="ml-2">
          <input type="text" v-model="userEmail" placeholder="Укажите почту" class="form-control">
        </div>
        <div class="ml-2">
          <select v-model="userRole" class="form-control" id="">
            <option value="TEACHER">Преподаватель</option>
            <option value="STUDENT">Студенты</option>
            <option value="MANAGER">Зав. Отделения</option>
            <option value="ADMIN">Администраторы</option>
          </select>
        </div>
        <div class="ml-2">
          <button class="btn btn-success" @click="getTeachers">Поиск</button>
        </div>
        <button class="btn btn-success ml-2" @click="openUserCreateModal"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-person-plus-fill" viewBox="0 0 16 16">
          <path d="M1 14s-1 0-1-1 1-4 6-4 6 3 6 4-1 1-1 1H1zm5-6a3 3 0 1 0 0-6 3 3 0 0 0 0 6z"/>
          <path fill-rule="evenodd" d="M13.5 5a.5.5 0 0 1 .5.5V7h1.5a.5.5 0 0 1 0 1H14v1.5a.5.5 0 0 1-1 0V8h-1.5a.5.5 0 0 1 0-1H13V5.5a.5.5 0 0 1 .5-.5z"/>
        </svg></button>
      </div>
      <br>
      <table class="table table-striped table-sm">
        <thead>
        <tr>
          <th>#</th>
          <th>Имя</th>
          <th>Почта</th>
          <th>Телефон</th>
          <th>Роль</th>
          <th>Дисциплина</th>
          <td></td>
          <td></td>
        </tr>
        </thead>
        <tbody>
        <tr v-for="user in users">
          <td></td>
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.phone }}</td>
          <td>{{ user.role }}</td>
          <td v-if="user.role == 'Преподаватель'">
            <NuxtLink :to="`/admin/users/teachers/${user.id}`">Диспциплины</NuxtLink></td>
          <td v-else-if="user.role == 'Админ'">Отец</td>
          <td v-else></td>
          <td>
            <button class="btn btn-primary" @click="openUpdateModal(user.id)"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pen-fill" viewBox="0 0 16 16">
            <path d="m13.498.795.149-.149a1.207 1.207 0 1 1 1.707 1.708l-.149.148a1.5 1.5 0 0 1-.059 2.059L4.854 14.854a.5.5 0 0 1-.233.131l-4 1a.5.5 0 0 1-.606-.606l1-4a.5.5 0 0 1 .131-.232l9.642-9.642a.5.5 0 0 0-.642.056L6.854 4.854a.5.5 0 1 1-.708-.708L9.44.854A1.5 1.5 0 0 1 11.5.796a1.5 1.5 0 0 1 1.998-.001z"/>
            </svg>
            </button>
          </td>
          <td><button class="btn btn-danger" @click="openDeleteModal(user.id)"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-person-x-fill" viewBox="0 0 16 16">
            <path fill-rule="evenodd" d="M1 14s-1 0-1-1 1-4 6-4 6 3 6 4-1 1-1 1H1zm5-6a3 3 0 1 0 0-6 3 3 0 0 0 0 6zm6.146-2.854a.5.5 0 0 1 .708 0L14 6.293l1.146-1.147a.5.5 0 0 1 .708.708L14.707 7l1.147 1.146a.5.5 0 0 1-.708.708L14 7.707l-1.146 1.147a.5.5 0 0 1-.708-.708L13.293 7l-1.147-1.146a.5.5 0 0 1 0-.708z"/>
          </svg></button></td>
        </tr>
        </tbody>
      </table>

      <b-modal id="bv-modal-example" v-model="showCreateModal" hide-footer>
        <template #modal-title>
          Создание пользователя
        </template>
        <div class="d-block text-center">
          <div class="" v-if="errors.name">
            <div class="text-danger">{{ errors.name }}</div>
          </div>
          <input type="text" placeholder="Введите имя" v-model="userNameForCreating" class="form-control">
          <div class="" v-if="errors.email">
            <div class="text-danger">{{ errors.email }}</div>
          </div>
          <input type="email" class="form-control mt-2" placeholder="Введите почту" v-model="userEmailForCreating">
          <div class="" v-if="errors.phone">
            <div class="text-danger">{{ errors.phone }}</div>
          </div>
          <input placeholder="Введите телефон. Прим:80(25)8965390" v-model="userPhoneForCreating" class="form-control mt-2">

          <div class="" v-if="errors.role">
            <div class="text-danger">{{ errors.role }}</div>
          </div>
          <select v-model="userRoleForCreating" class="form-control mt-2">
            <option value="" disabled>Выберите роль</option>
            <option value="TEACHER">Преподаватель</option>
            <option value="STUDENT">Студенты</option>
            <option value="MANAGER">Зав. Отделения</option>
            <option value="ADMIN">Администраторы</option>
          </select>
        </div>
        <div class="row text-center">
          <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-success" block @click="createUser">Создать пользователя</b-button>
          <b-button class="col-5 ml-2 mt-3" block @click="closeUserCreateModal">Отмена</b-button>
        </div>
      </b-modal>

      <b-modal id="bv-modal-example" v-model="showUpdateModal" hide-footer>
        <template #modal-title>
          Изменение пользователя
        </template>
        <div class="d-block text-center">
          <div class="" v-if="errors.name">
            <div class="text-danger">{{ errors.name }}</div>
          </div>
          <input type="text" placeholder="Введите имя" v-model="userNameForCreating" class="form-control">
          <div class="" v-if="errors.email">
            <div class="text-danger">{{ errors.email }}</div>
          </div>
          <input type="email" class="form-control mt-2" placeholder="Введите почту" v-model="userEmailForCreating">
          <div class="" v-if="errors.phone">
            <div class="text-danger">{{ errors.phone }}</div>
          </div>
          <input placeholder="Введите телефон. Прим:80(25)8965390" v-model="userPhoneForCreating" class="form-control mt-2">

          <div class="" v-if="errors.role">
            <div class="text-danger">{{ errors.role }}</div>
          </div>
          <select v-model="userRoleForCreating" class="form-control mt-2">
            <option value="" disabled>Выберите роль</option>
            <option value="TEACHER">Преподаватель</option>
            <option value="ADMIN">Администратор</option>
          </select>
        </div>
        <div class="row text-center">
          <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-success" block @click="updateUser">Изменить пользователя</b-button>
          <b-button class="col-5 ml-2 mt-3" block @click="closeUserUpdateModal">Отмена</b-button>
        </div>
      </b-modal>

      <b-modal id="bv-modal-example" v-model="showDeleteModal" hide-footer>
        <template #modal-title>
          Удаление пользователя
        </template>
        <div class="d-block text-center">
          Вы действительно хотите удалить?
        </div>
        <div class="row text-center">
          <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-danger" block @click="deleteUser">Удалить пользователя</b-button>
          <b-button class="col-5 ml-2 mt-3" block @click="$bvModal.hide('bv-modal-example')">Отмена</b-button>
        </div>
      </b-modal>
    </div>
  </div>
</template>

<script>
export default {
  name: "AdminUsers",
  data() {
    return {
      users: "",
      userName: "",
      userEmail: "",
      userRole: "TEACHER",
      userId: '',
      errors: '',
      userPhoneForCreating: '',
      userEmailForCreating: '',
      userNameForCreating: '',
      userRoleForCreating: '',
      showCreateModal: false,
      showDeleteModal: false,
      showUpdateModal: false
    }
  },
  async mounted() {
    await this.getTeachers();
  },
  methods: {
    async getTeachers() {
      const response = await fetch(`http://department.biz/api/admin/users?name=${this.userName}&email=${this.userEmail}&role=${this.userRole}`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
      });
      if (response.status == 500) {

      } else {
        this.users = await response.json();
        this.users = this.users.data;
      }
    },
    async createUser() {
      const response = await fetch('http://department.biz/api/admin/users/create', {
        headers: {
          'Content-Type': "application/json",
          'Accept': "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
        body: JSON.stringify({
          'name': this.userNameForCreating,
          'email': this.userEmailForCreating,
          'phone': this.userPhoneForCreating,
          'role': this.userRoleForCreating
        })
      })

      const responseData = await response.json();
      if (response.status == 422) {
        this.errors = new Object();
        this.errors.name = responseData.errors.name ? responseData.errors.name[0] : null;
        this.errors.phone = responseData.errors.phone ? responseData.errors.phone[0] : null
        this.errors.email = responseData.errors.email ? responseData.errors.email[0] : null
        this.errors.role = responseData.errors.role ? responseData.errors.role[0] : null
      } else {
        this.users.push(responseData.data);
        this.showCreateModal = false;
        this.resetData()
      }
    },
    openDeleteModal(id) {
      this.userId = id;
      this.showDeleteModal = true
    },
    async deleteUser() {
      const response = await fetch(`http://department.biz/api/admin/users/delete/${this.userId}`, {
        headers: {
          'Content-Type': "application/json",
          'Accept': "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
      })
      const responseData = await response.json();
      if (response.status == 422) {

      } else {
        this.showDeleteModal = false;
        for(let i = 0; i < this.users.length; i++) {
          if(this.users[i].id == this.userId) {
            this.users.splice(i, 1);
            break;
          }
        }
        this.resetData()
      }
    },
    openUpdateModal(userId) {
      this.userId = userId
      this.showUpdateModal = true
      for(let i = 0; i < this.users.length; i++) {
        if(this.users[i].id == this.userId) {
          this.userNameForCreating = this.users[i].name;
          this.userEmailForCreating = this.users[i].email;
          this.userPhoneForCreating = this.users[i].phone;
          break;
        }
      }
    },
    async updateUser()
    {
      const response = await fetch(`http://department.biz/api/admin/users/update/${this.userId}`, {
        headers: {
          'Content-Type': "application/json",
          'Accept': "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
        body: JSON.stringify({
          'name': this.userNameForCreating,
          'email': this.userEmailForCreating,
          'phone': this.userPhoneForCreating,
          'role': this.userRoleForCreating
        })
      })

      const responseData = await response.json();
      if (response.status == 422) {
        this.errors = new Object();
        this.errors.name = responseData.errors.name ? responseData.errors.name[0] : null;
        this.errors.phone = responseData.errors.phone ? responseData.errors.phone[0] : null
        this.errors.email = responseData.errors.email ? responseData.errors.email[0] : null
        this.errors.role = responseData.errors.role ? responseData.errors.role[0] : null
      } else {
        for(let i = 0; i < this.users.length; i++) {
          if(this.users[i].id == this.userId) {
            this.users[i].name = responseData.data.name;
            this.users[i].email = responseData.data.email;
            this.users[i].phone = responseData.data.phone;
            break;
          }
        }
        this.showUpdateModal = false;
        this.resetData();
      }
    },
    resetData() {
      this.userNameForCreating = '';
      this.userEmailForCreating = '';
      this.userPhoneForCreating = '';
      this.userRoleForCreating = '';
      this.errors = '';
    },
    closeUserUpdateModal() {
      this.showUpdateModal = false;
      this.resetData();
    },
    closeUserCreateModal() {
      this.showCreateModal = false;
      this.resetData();
    },
    openUserCreateModal() {
      this.showCreateModal = true;
      this.resetData()
    }
  }
}
</script>

<style scoped>

</style>
