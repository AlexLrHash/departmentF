<template>
    <div class="d-flex flex-column flex-md-row align-items-center p-3 px-md-4 mb-3 bg-white border-bottom box-shadow">
      <h5 class="my-0 mr-md-auto font-weight-normal"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-badge-ad-fill" viewBox="0 0 16 16">
        <path d="M11.35 8.337c0-.699-.42-1.138-1.001-1.138-.584 0-.954.444-.954 1.239v.453c0 .8.374 1.248.972 1.248.588 0 .984-.44.984-1.2v-.602zm-5.413.237-.734-2.426H5.15l-.734 2.426h1.52z"/>
        <path d="M2 2a2 2 0 0 0-2 2v8a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2H2zm6.209 6.32c0-1.28.694-2.044 1.753-2.044.655 0 1.156.294 1.336.769h.053v-2.36h1.16V11h-1.138v-.747h-.057c-.145.474-.69.804-1.367.804-1.055 0-1.74-.764-1.74-2.043v-.695zm-4.04 1.138L3.7 11H2.5l2.013-5.999H5.9L7.905 11H6.644l-.47-1.542H4.17z"/>
      </svg><NuxtLink to="/" class="p-2 text-dark" href="#">Department</NuxtLink></h5>
      <div class="" v-if="!user">
        <NuxtLink to="/auth/login" class="btn btn-outline-primary" href="#">Войти</NuxtLink>
      </div>
      <div class="" v-else>
        <img class="rounded-circle" :src="user.avatar" width="50px" alt="">
        <NuxtLink to="/profile">{{ user.name }}</NuxtLink>
        <button class="btn btn-outline-primary ml-2" to="/" @click="logoutUser">Выход</button>
      </div>
    </div>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      user: '',
    }
  },
  async mounted() {
    await this.getUser();
  },
  methods: {
    logoutUser() {
      localStorage.removeItem('jwt');
      this.$router.push({path: '/auth/login'});
    },
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
        this.$emit('getUser', 'asd');
      }
    }
  }
}
</script>

<style scoped>

</style>
