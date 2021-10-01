<template>
  <div class="text-center" style="max-width: 400px; margin: 0 auto">
    <br>
    <h1 class="my-0 mr-md-auto font-weight-normal"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-badge-ad-fill" viewBox="0 0 16 16">
      <path d="M11.35 8.337c0-.699-.42-1.138-1.001-1.138-.584 0-.954.444-.954 1.239v.453c0 .8.374 1.248.972 1.248.588 0 .984-.44.984-1.2v-.602zm-5.413.237-.734-2.426H5.15l-.734 2.426h1.52z"/>
      <path d="M2 2a2 2 0 0 0-2 2v8a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2H2zm6.209 6.32c0-1.28.694-2.044 1.753-2.044.655 0 1.156.294 1.336.769h.053v-2.36h1.16V11h-1.138v-.747h-.057c-.145.474-.69.804-1.367.804-1.055 0-1.74-.764-1.74-2.043v-.695zm-4.04 1.138L3.7 11H2.5l2.013-5.999H5.9L7.905 11H6.644l-.47-1.542H4.17z"/>
    </svg>Department</h1>
    <br>
<!--    <button @click="vkAuthorize" class="btn btn-primary">Войти через VK</button>-->
    <div class="text-danger" v-if="errors.invalid_credentials">{{ errors.invalid_credentials }}</div>
    <div class="text-danger" v-if="errors.email">{{ errors.email }}</div>
    <div v-else><br></div>
    <input v-model="email" type="email" id="inputEmail" class="form-control" placeholder="Почта" required="" autofocus="">
    <div class="text-danger" v-if="errors.password">{{ errors.password }}</div>
    <div v-else><br></div>
    <input v-model="password" type="password" id="inputPassword" class="form-control" placeholder="Пароль" required="">
    <div class="checkbox mb-3">
      <label>
        <input type="checkbox" value="remember-me"> Запомнить меня
      </label>
    </div>
    <button class="btn btn-lg btn-primary btn-block" @click="loginUser">Авторизироваться</button>
    <br>
    <p class="mt-3 mb-3 text-muted">© 2021-2022</p>
  </div>
</template>

<script>
export default {
  name: "Login",

  data() {
    return {
      token: '',
      email: '',
      password: '',
      errors: ''
    }
  },
  methods: {
    async loginUser() {
      const response = await fetch('http://localhost:8000/api/login', {
        headers: {
          "Content-Type": "application/json",
          "Accept": "application/json"
        },
        method: "post",
        body: JSON.stringify({
          password: this.password,
          email: this.email
        })
      });
      let responseData = await response.json();
      if (response.ok) {
        // this.$router.push('/');
        this.token = responseData.token;
        localStorage.setItem('jwt', this.token);
        this.$router.push({path: '/'});
      } else {
        this.errors = new Object();
        if (response.status == 401) {
            this.errors.invalid_credentials = responseData.message
        } else if (response.status == 422) {
          let passwordError = responseData.errors.password;
          let emailError = responseData.errors.email;

          this.errors.email = emailError ? emailError[0] : null;
          this.errors.password = passwordError ? passwordError[0] : null;
        }
      }
    },
    // TODO авторизация по vk на front
    // async vkAuthorize()
    // {
    //   const response = await fetch('http://localhost:8000/api/vk/auth', {
    //     headers: {
    //       "Content-Type": "application/json",
    //       'Accept': "application/json"
    //     },
    //   })
    //   let responseData = await response.json();
    //
    //   if (response.ok) {
    //     let token = responseData.token;
    //     localStorage.setItem('jwt', token);
    //     this.$router.push({path: '/'});
    //   }
    // }
  }
}
</script>

<style scoped>

</style>
