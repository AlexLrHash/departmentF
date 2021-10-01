<template>
  <body>
  {{ userAvatar }}
  <nav class="site-header sticky-top py-1">
    <div class="container d-flex flex-column flex-md-row justify-content-between">
      <a class="py-2" href="#">
        <h5 class="my-0 mr-md-auto font-weight-normal"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-badge-ad-fill" viewBox="0 0 16 16">
          <path d="M11.35 8.337c0-.699-.42-1.138-1.001-1.138-.584 0-.954.444-.954 1.239v.453c0 .8.374 1.248.972 1.248.588 0 .984-.44.984-1.2v-.602zm-5.413.237-.734-2.426H5.15l-.734 2.426h1.52z"/>
          <path d="M2 2a2 2 0 0 0-2 2v8a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2H2zm6.209 6.32c0-1.28.694-2.044 1.753-2.044.655 0 1.156.294 1.336.769h.053v-2.36h1.16V11h-1.138v-.747h-.057c-.145.474-.69.804-1.367.804-1.055 0-1.74-.764-1.74-2.043v-.695zm-4.04 1.138L3.7 11H2.5l2.013-5.999H5.9L7.905 11H6.644l-.47-1.542H4.17z"/>
        </svg>Department</h5>
      </a>
      <a class="py-2 d-none d-md-inline-block" href="/">Home</a>
    </div>
  </nav>

  <div class="position-relative overflow-hidden p-3 p-md-5 m-md-3 text-center bg-light">
    <div class="col-md-5 p-lg-5 mx-auto my-5">
      <img class="rounded-circle" :src="user.avatar" width="250" height="250" alt="">
      <div class="row">
        <input type="file" @change="onAttachmedChange" class="form-control mt-2 col-8 mr-2">
        <button type="submit" class="btn btn-primary mt-2" @click="updateUserFace">Загрузить</button>
      </div>
      <h1 class="display-4 font-weight-normal">{{ user.name }}</h1>
      <p class="lead font-weight-normal">{{ user.email }}</p>
      <h6>Хотите изменить имя?</h6>
      <div class="row">
        <input type="text" class="form-control col-8 mr-2" placeholder="Введите новое имя" v-model="userName">
        <button @click="updateUserProfile" class="btn btn-outline-secondary">Изменить</button>
      </div>
    </div>
    <div class="product-device box-shadow d-none d-md-block"></div>
    <div class="product-device product-device-2 box-shadow d-none d-md-block"></div>
  </div>
  <div class="">

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
      userAvatar: ""
    }
  },
  async mounted() {
    await this.getUser();
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
      if (response.ok) {
        this.user = await response.json();
        this.user = this.user.data;
      }
    },
    async updateUserFace()
    {
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
    },
    onAttachmedChange(e)
    {
      this.userAvatar = e.target.files[0]
    }
  }
}
</script>

<style scoped>

</style>
