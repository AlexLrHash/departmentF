<template>
  <div class="row">
    <nav class="col-md-2 d-none d-md-block bg-light sidebar">
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
        <NuxtLink to="departments" class="nav-link active">Все отделения</NuxtLink>
        <ul class="nav flex-column" v-for="department in departments.data">
          <li class="nav-item">
            <NuxtLink :to="`departments/${department.id}`" class="nav-link active" href="#">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-home" style="--darkreader-inline-stroke: currentColor; --darkreader-inline-fill: none;" data-darkreader-inline-stroke="" data-darkreader-inline-fill=""><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg>
              {{ department.name }} <span class="sr-only">(current)</span>
            </NuxtLink>
          </li>
        </ul>
        <h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-muted">
          <span>Заведующие отделений</span>
          <a class="d-flex align-items-center text-muted" href="#" style="margin-left: 10px;">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-question-circle" viewBox="0 0 16 16">
              <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
              <path d="M5.255 5.786a.237.237 0 0 0 .241.247h.825c.138 0 .248-.113.266-.25.09-.656.54-1.134 1.342-1.134.686 0 1.314.343 1.314 1.168 0 .635-.374.927-.965 1.371-.673.489-1.206 1.06-1.168 1.987l.003.217a.25.25 0 0 0 .25.246h.811a.25.25 0 0 0 .25-.25v-.105c0-.718.273-.927 1.01-1.486.609-.463 1.244-.977 1.244-2.056 0-1.511-1.276-2.241-2.673-2.241-1.267 0-2.655.59-2.75 2.286zm1.557 5.763c0 .533.425.927 1.01.927.609 0 1.028-.394 1.028-.927 0-.552-.42-.94-1.029-.94-.584 0-1.009.388-1.009.94z"/>
            </svg>
          </a>
        </h6>
        <ul class="nav flex-column mb-2" v-for="manager in managers.data">
          <li class="nav-item">
            <NuxtLink :to="`/managers/${manager.id}`" class="nav-link" href="#">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text" style="--darkreader-inline-stroke: currentColor; --darkreader-inline-fill: none;" data-darkreader-inline-stroke="" data-darkreader-inline-fill=""><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path><polyline points="14 2 14 8 20 8"></polyline><line x1="16" y1="13" x2="8" y2="13"></line><line x1="16" y1="17" x2="8" y2="17"></line><polyline points="10 9 9 9 8 9"></polyline></svg>
              {{ manager.name }}
            </NuxtLink>
          </li>
        </ul>
      </div>
    </nav>
    <main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4"><div style="position: absolute; inset: 0px; overflow: hidden; pointer-events: none; visibility: hidden; z-index: -1;" class="chartjs-size-monitor"><div class="chartjs-size-monitor-expand" style="position:absolute;left:0;top:0;right:0;bottom:0;overflow:hidden;pointer-events:none;visibility:hidden;z-index:-1;"><div style="position:absolute;width:1000000px;height:1000000px;left:0;top:0"></div></div><div class="chartjs-size-monitor-shrink" style="position:absolute;left:0;top:0;right:0;bottom:0;overflow:hidden;pointer-events:none;visibility:hidden;z-index:-1;"><div style="position:absolute;width:200%;height:200%;left:0; top:0"></div></div></div>
      <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pb-2 mb-3 border-bottom">
        <h1 class="h2">Кафедра</h1>
        <div class="btn-toolbar mb-2 mb-md-0">
          <div class="btn-group mr-2">

            <button class="btn btn-sm btn-outline-secondary"><script src="https://yastatic.net/share2/share.js"></script>
              <div class="ya-share2" data-curtain data-limit="3" data-services="messenger,vkontakte,facebook,odnoklassniki,telegram,twitter,viber,whatsapp"></div></button>
            <button class="btn btn-sm btn-outline-secondary" @click="exportPdf">Экспортировать</button>
          </div>
          <button class="btn btn-sm btn-outline-secondary dropdown-toggle">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-calendar" style="--darkreader-inline-stroke: currentColor; --darkreader-inline-fill: none;" data-darkreader-inline-stroke="" data-darkreader-inline-fill=""><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg>
            На это неделе
          </button>
        </div>
      </div>

      <div class="container">
<!--        <img src="https://i.playground.ru/i/pix/2081201/image.jpg" style="max-width: 850px;" alt="">-->
        <img src="https://www.alternativ.be/wp-content/uploads/2019/07/Martex-Welcome-4.jpg" style="max-width: 850px" alt="">
      </div>

      <br>
      <h2>Дисциплины</h2>
      <div class="table-responsive">
        <table class="table table-striped table-sm">
          <thead>
          <tr>
            <th>#</th>
            <th>Имя</th>
            <th>Количество лабораторных</th>
            <th>Количество практик</th>
            <th>Преподаватели</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="discipline in disciplines.data">
            <td></td>
            <td>{{ discipline.name}}</td>
            <td>{{ discipline.number_of_labs }}</td>
            <td>{{ discipline.number_of_practices }}</td>
            <td><NuxtLink :to="`teachers/disciplines/` + discipline.id">Посмотреть преподавателей</NuxtLink></td>
          </tr>
          </tbody>
        </table>
      </div>
    </main>
  </div>

</template>
<script>
export default {
  name: "Main",
  data() {
    return {
      departments: '',
      managers: '',
      disciplines: ''
    }
  },
  async mounted() {
    await this.getDepartments();
    await this.getManagers();
    await this.getDisciplines();
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
    async getManagers() {
      const response = await fetch('http://localhost:8000/api/managers', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
        },
      });
      if (response.status == 500) {

      } else {
        this.managers = await response.json();
      }
    },
    async getDisciplines() {
      const response = await fetch('http://localhost:8000/api/disciplines', {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
        },
      });
      if (response.status == 500) {

      } else {
        this.disciplines = await response.json();
      }
    },
    async exportPdf() {
      const response = await fetch('http://localhost:8000/api/disciplines/export/pdf', {
        headers: {
          responseType: 'arraybuffer'
        },
      });
      if (response.status == 500) {
      } else {
        console.log(response);
        this.downloadFile(response, 'discipline');
      }
    },
    downloadFile(response, filename) {
      let blob = new Blob([response.body], { type: 'application/pdf' })
      let link = document.createElement('a')
      link.href = window.URL.createObjectURL(blob)
      link.download = filename + '.pdf';
      link.click()
    }
  }
}
</script>

<style scoped>

</style>
