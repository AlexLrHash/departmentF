<template>
  <div class="">
    <main role="main" v-if="departmentId">
      <div class="position-relative overflow-hidden p-3 p-md-5 m-md-3 text-center bg-light">
        <div class="col-md-5 p-lg-5 mx-auto my-5">
          <img :src="department.background" width="150" height="150" alt="">
          <h1 class="display-4 font-weight-normal">{{ department.name }}</h1>
          <p class="lead font-weight-normal">{{ department.description }}</p>
        </div>
        <div class="product-device box-shadow d-none d-md-block"></div>
        <div class="product-device product-device-2 box-shadow d-none d-md-block"></div>
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
      const response = await fetch('http://localhost:8000/api/departments', {
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
      const response = await fetch('http://localhost:8000/api/departments/' + id, {
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
