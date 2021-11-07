<template>
  <div class="">
    <main role="main" v-if="departmentId">
      <h1 class="text-center text-dark">{{ department.name }}</h1>
      <div class="row">
        <div class="col-6 mr-5">
          <img width="600" class="mt-2" :src="department.background" alt="">
          <div class="pt-5">
            <p class="lead font-weight-normal">{{ department.description }}</p>
          </div>
          <div class="product-device box-shadow d-none d-md-block"></div>
          <div class="product-device product-device-2 box-shadow d-none d-md-block"></div>
        </div>
          <nav class="col-md-4 d-none d-md-block bg-light sidebar">
            <div class="sidebar-sticky">
              <h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-muted">
                <span>Отделения</span>
                <a class="d-flex align-items-center text-muted" href="#" style="margin-left: 10px">
                  <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-question-circle" viewBox="0 0 16 16">
                    <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
                    <path d="M5.255 5.786a.237.237 0 0 0 .241.247h.825c.138 0 .248-.113.266-.25.09-.656.54-1.134 1.342-1.134.686 0 1.314.343 1.314 1.168 0 .635-.374.927-.965 1.371-.673.489-1.206 1.06-1.168 1.987l.003.217a.25.25 0 0 0 .25.246h.811a.25.25 0 0 0 .25-.25v-.105c0-.718.273-.927 1.01-1.486.609-.463 1.244-.977 1.244-2.056 0-1.511-1.276-2.241-2.673-2.241-1.267 0-2.655.59-2.75 2.286zm1.557 5.763c0 .533.425.927 1.01.927.609 0 1.028-.394 1.028-.927 0-.552-.42-.94-1.029-.94-.584 0-1.009.388-1.009.94z"/>
                  </svg>
                </a>
              </h6>
              <h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-muted">
                <span>Заведующий отделений</span>
                <div v-if="department.manager">
                  <NuxtLink :to="`/managers/${department.manager ? department.manager.id :'' }`" class="d-flex align-items-center text-muted" style="margin-left: 10px;">{{ department.manager ? department.manager.name : ''}}</NuxtLink>
                </div>
              </h6>
              <br>
              <h5 class="nav-link active">Все преподаватели отделения</h5>
              <div class="" v-if="department.teachers && department.teachers.length != 0">
                <ul class="nav flex-column" v-for="teacher in department.teachers">
                  <li class="nav-item">
                    <NuxtLink :to="`/teachers/${teacher.id}`" class="nav-link active" href="#">
                      <span>{{ teacher.name }}</span>
                    </NuxtLink>
                  </li>
                </ul>
              </div>
              <div v-else class="ml-3">
                Тут пока пусто
              </div>
            </div>
          </nav>
      </div>
    </main>
    <main v-else>
      <section class="jumbotron text-center">
        <div class="container">
          <h1 class="jumbotron-heading">Наши отделения</h1>
          <p class="lead text-muted"></p>
          <p>
          </p>
        </div>
      </section>

      <div class="album py-5 bg-light">
        <div class="container">
          <div class="row">
            <div class="col-md-4" v-for="department in departments.data">
              <div class="card mb-4 box-shadow">
                <img class="card-img-top" :src="department.background" alt="Thumbnail [100%x225]" style="height: 225px; width: 100%; display: block;" data-holder-rendered="true">
                <div class="card-body">
                  <p class="card-text">{{ department.name }}</p>
                  <div class="d-flex justify-content-between align-items-center">
                    <div class="btn-group">
                      <NuxtLink :to="`departments/${department.id}`" type="button" class="btn btn-sm btn-outline-secondary">Подробннее</NuxtLink>
                    </div>
                    <small class="text-muted"></small>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "Departments",
  data() {
    return {
      departmentId: this.$route.params.id,
      department: '',
      departments: '',
    }
  },
  async mounted() {
    if (this.departmentId) {
      await this.getDepartment(this.departmentId);
    } else {
      await this.getDepartments();
    }
  },
  methods: {
    async getDepartments() {
      const response = await fetch('http://department.biz/api/departments', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
        },
      });
      if (response.status == 500) {

      } else {
        this.departments = await response.json();
      }
    },
    async getDepartment(id) {
      const response = await fetch('http://department.biz/api/departments/' + id, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
        },
      });
      if (response.status == 500) {

      } else {
        this.department = await response.json();
        this.department = this.department.data
      }
    }
  }
}
</script>

<style scoped>

</style>
