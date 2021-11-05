<template>
  <div style="margin-top: 100px;">
    <div class="table-responsive">
      <div class="container row">
        <div class="ml-2">
          <input type="text" v-model="departmentName" placeholder="Укажите название" class="form-control">
        </div>
        <div class="ml-2">
          <input type="text" v-model="departmentManager" placeholder="Укажите заведующего" class="form-control">
          <div class="" style="position: absolute; background: white; opacity: .9; color: white">
            <div v-for="manager in departmentManagers" style="cursor: pointer" @click="selectManager(manager.name)">{{ manager.name }}</div>
          </div>
        </div>
        <div class="ml-2">
          <button class="btn btn-success" @click="getDepartments">Поиск</button>
        </div>
        <div>
          <button class="btn btn-success ml-2" @click="openCreateModal"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-file-earmark-plus-fill" viewBox="0 0 16 16">
            <path d="M9.293 0H4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V4.707A1 1 0 0 0 13.707 4L10 .293A1 1 0 0 0 9.293 0zM9.5 3.5v-2l3 3h-2a1 1 0 0 1-1-1zM8.5 7v1.5H10a.5.5 0 0 1 0 1H8.5V11a.5.5 0 0 1-1 0V9.5H6a.5.5 0 0 1 0-1h1.5V7a.5.5 0 0 1 1 0z"/>
          </svg></button>
        </div>
      </div>
      <br>
      <table class="table table-striped table-sm">
        <thead>
        <tr>
          <th>Название</th>
          <th>Описание</th>
          <th>Зав отделения</th>
          <th></th>
          <th></th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="department in departments">
          <td>{{ department.name }}</td>
          <td>{{ department.description }}</td>
          <td>{{ department.manager.name }}</td>
          <td><button class="btn btn-primary" @click="openUpdateModal(department.id)"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pen-fill" viewBox="0 0 16 16">
            <path d="m13.498.795.149-.149a1.207 1.207 0 1 1 1.707 1.708l-.149.148a1.5 1.5 0 0 1-.059 2.059L4.854 14.854a.5.5 0 0 1-.233.131l-4 1a.5.5 0 0 1-.606-.606l1-4a.5.5 0 0 1 .131-.232l9.642-9.642a.5.5 0 0 0-.642.056L6.854 4.854a.5.5 0 1 1-.708-.708L9.44.854A1.5 1.5 0 0 1 11.5.796a1.5 1.5 0 0 1 1.998-.001z"/>
          </svg></button></td>
          <td><button class="btn btn-danger" @click="openDeleteModal(department.id)"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-file-earmark-x-fill" viewBox="0 0 16 16">
            <path d="M9.293 0H4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V4.707A1 1 0 0 0 13.707 4L10 .293A1 1 0 0 0 9.293 0zM9.5 3.5v-2l3 3h-2a1 1 0 0 1-1-1zM6.854 7.146 8 8.293l1.146-1.147a.5.5 0 1 1 .708.708L8.707 9l1.147 1.146a.5.5 0 0 1-.708.708L8 9.707l-1.146 1.147a.5.5 0 0 1-.708-.708L7.293 9 6.146 7.854a.5.5 0 1 1 .708-.708z"/>
          </svg></button></td>
        </tr>
        </tbody>
      </table>
      <b-modal id="bv-modal-example" v-model="showCreateModal" hide-footer>
        <template #modal-title>
          Создание отделений
        </template>
        <div class="d-block text-center">
          <div class="" v-if="errors.name">
            <div class="text-danger">{{ errors.name }}</div>
          </div>
          <input type="text" placeholder="Введите название отделения" v-model="departmentNameForCreating" class="form-control">
          <div class="" v-if="errors.description">
            <div class="text-danger">{{ errors.description }}</div>
          </div>
          <textarea placeholder="Введите описание" v-model="departmentDescriptionForCreating" class="form-control mt-2" id="" cols="30" rows="10"></textarea>
          <div class="" v-if="errors.manager">
            <div class="text-danger">{{ errors.manager }}</div>
          </div>
          <select v-model="managerIdForCreating" class="form-control mt-2">
            <option value="" disabled>Выберите мэнэджера</option>
            <option :value="manager.id" v-for="manager in managers">{{ manager.name }}</option>
          </select>
        </div>
        <div class="row text-center">
          <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-success" block @click="createDepartment">Создать отделение</b-button>
          <b-button class="col-5 ml-2 mt-3" block @click="closeCreateDepartmentModal">Отмена</b-button>
        </div>
      </b-modal>
      <b-modal id="bv-modal-example" v-model="showUpdateModal" hide-footer>
        <template #modal-title>
          Изменение отделения
        </template>
        <div class="d-block text-center">
          <div class="" v-if="errors.name">
            <div class="text-danger">{{ errors.name }}</div>
          </div>
          <input type="text" placeholder="Введите название отделения" v-model="departmentNameForCreating" class="form-control">
          <div class="" v-if="errors.description">
            <div class="text-danger">{{ errors.description }}</div>
          </div>
          <textarea placeholder="Введите описание" v-model="departmentDescriptionForCreating" class="form-control mt-2" id="" cols="30" rows="10"></textarea>
          <div class="" v-if="errors.manager">
            <div class="text-danger">{{ errors.manager }}</div>
          </div>
          <select v-model="managerIdForCreating" class="form-control mt-2">
            <option value="" disabled>Выберите мэнэджера</option>
            <option :value="manager.id" v-for="manager in managers">{{ manager.name }}</option>
          </select>
        </div>
        <div class="row text-center">
          <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-success" block @click="updateDepartment">Изменение отделения</b-button>
          <b-button class="col-5 ml-2 mt-3" block @click="closeUpdateDepartmentModal">Отмена</b-button>
        </div>
      </b-modal>
      <b-modal id="bv-modal-example" v-model="showDeleteModal" hide-footer>
        <template #modal-title>
          Удаление отделения
        </template>
        <div class="d-block text-center">
          Вы действительно хотите удалить?
        </div>
        <div class="row text-center">
          <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-danger" block @click="deleteDepartment">Удалить отделение</b-button>
          <b-button class="col-5 ml-2 mt-3" block @click="showDeleteModal = false">Отмена</b-button>
        </div>
      </b-modal>
    </div>
  </div>
</template>

<script>
export default {
  name: "AdminDepartments",
  data() {
    return {
      departments: "",
      departmentName: "",
      departmentManagers: "",
      departmentId: '',
      departmentManager: "",
      showCreateModal: false,
      departmentNameForCreating: '',
      departmentDescriptionForCreating: '',
      managerIdForCreating: '',
      errors: '',
      managers: '',
      showDeleteModal: false,
      showUpdateModal: false,
    }
  },
  watch: {
    departmentManager: async function (val) {
      await this.getManagers(val);
    }
  },
  async mounted() {
    await this.getDepartments();
    await this.getAllManagers();
  },
  methods: {
    async getDepartments() {
      const response = await fetch(`http://department.biz/api/admin/departments?name=${this.departmentName}&manager=${this.departmentManager}`, {
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
    selectManager(managerName) {
      this.departmentManager = managerName;
    },
    async getManagers(val) {
      const response = await fetch(`http://department.biz/api/admin/users?name=${val}&email=&role=MANAGER`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
      });
      if (response.status == 500) {
      } else {
        this.departmentManagers = await response.json();
        this.departmentManagers = this.departmentManagers.data;
      }
    },
    async getAllManagers() {
      const response = await fetch(`http://department.biz/api/admin/managers`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
      });
      if (response.status == 500) {
      } else {
        this.managers = await response.json();
        this.managers = this.managers.data;
      }
    },
    async createDepartment() {
      const response = await fetch(`http://department.biz/api/admin/departments/create`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
        body: JSON.stringify({
          'name': this.departmentNameForCreating,
          'manager_id': this.managerIdForCreating,
          'description': this.departmentDescriptionForCreating
        })
      })
      const responseData = await response.json();
      if (response.status == 422) {
        this.errors = new Object();
        const errorsName = responseData.errors.name
        const errorsDescription = responseData.errors.description
        const errorsManager = responseData.errors.manager_id
        this.errors.name = errorsName ? errorsName[0] : null
        this.errors.description = errorsDescription ? errorsDescription[0] : null
        this.errors.manager = errorsManager ? errorsManager[0]: null
      } else {
        this.departments.push(responseData.data);
        this.showCreateModal = false
        this.resetData();
      }
    },
    openCreateModal()
    {
      this.showCreateModal = true;
    },
    closeCreateDepartmentModal()
    {
      this.showCreateModal = false;
      this.resetData();
    },
    openDeleteModal(val) {
      this.showDeleteModal = true;
      this.departmentId = val;
    },
    resetData()
    {
      this.departmentDescriptionForCreating = '';
      this.departmentNameForCreating = '';
      this.managerIdForCreating = '';
      this.errors = '';
    },
    async deleteDepartment() {
      const response = await fetch(`http://department.biz/api/admin/departments/delete/${this.departmentId}`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
      })
      this.showDeleteModal = false;
      const responseData = await response.json();
      for(let i = 0; i < this.departments.length; i++) {
        if(this.departments[i].id == this.departmentId) {
          this.departments.splice(i, 1);
          break;
        }
      }
    },
    async updateDepartment() {
        const response = await fetch(`http://department.biz/api/admin/departments/update/${this.departmentId}`, {
          headers: {
            'Content-Type': 'application/json',
            "Accept": "application/json",
            'Authorization': "Bearer " + localStorage.getItem('jwt')
          },
          method: 'post',
          body: JSON.stringify({
            'name': this.departmentNameForCreating,
            'manager_id': this.managerIdForCreating,
            'description': this.departmentDescriptionForCreating
          })
        })
        const responseData = await response.json();
        if (response.status == 422) {
          this.errors = new Object();
          const errorsName = responseData.errors.name
          const errorsDescription = responseData.errors.description
          const errorsManagers = responseData.errors.manager_id
          this.errors.name = errorsName ? errorsName[0] : null
          this.errors.description = errorsDescription ? errorsDescription[0] : null
          this.errors.manager = errorsManagers ? errorsManagers[0] : null
        } else {
          this.showUpdateModal = false
          for (let i = 0; i < this.departments.length; i++) {
            if (this.departments[i].id == this.departmentId) {
              this.departments[i].name = responseData.data.name;
              this.departments[i].description = responseData.data.description;
              this.departments[i].department_id = responseData.data.department_id;
              break;
            }
          }
          this.resetData();
      }
    },
    closeUpdateDepartmentModal() {
      this.showUpdateModal = false;
      this.resetData();
    },
    openUpdateModal(departmentId) {
      this.showUpdateModal = true;
      this.departmentId = departmentId
      for(let i = 0; i < this.departments.length; i++) {
        if(this.departments[i].id == this.departmentId) {
          this.departmentNameForCreating = this.departments[i].name;
          this.departmentDescriptionForCreating = this.departments[i].description
          break;
        }
      }
    }
  }
}
</script>

<style scoped>

</style>
