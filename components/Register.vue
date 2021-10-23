<template>
  <div class="text-center" style="max-width: 400px; margin: 0 auto">
    <br>
    <h1 class="my-0 mr-md-auto font-weight-normal"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-badge-ad-fill" viewBox="0 0 16 16">
      <path d="M11.35 8.337c0-.699-.42-1.138-1.001-1.138-.584 0-.954.444-.954 1.239v.453c0 .8.374 1.248.972 1.248.588 0 .984-.44.984-1.2v-.602zm-5.413.237-.734-2.426H5.15l-.734 2.426h1.52z"/>
      <path d="M2 2a2 2 0 0 0-2 2v8a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2H2zm6.209 6.32c0-1.28.694-2.044 1.753-2.044.655 0 1.156.294 1.336.769h.053v-2.36h1.16V11h-1.138v-.747h-.057c-.145.474-.69.804-1.367.804-1.055 0-1.74-.764-1.74-2.043v-.695zm-4.04 1.138L3.7 11H2.5l2.013-5.999H5.9L7.905 11H6.644l-.47-1.542H4.17z"/>
    </svg>Department</h1>
    <br>
    <div class="text-danger" v-if="errors.invalid_credentials">{{ errors.invalid_credentials }}</div>
    <div class="text-danger" v-if="errors.name">{{ errors.name }}</div>
    <div v-else><br></div>
    <input v-model="name" type="email" id="inputName" class="form-control" placeholder="Имя" required="" autofocus="">
    <div class="text-danger" v-if="errors.email">{{ errors.email }}</div>
    <div v-else><br></div>
    <input v-model="email" type="email" id="inputEmail" class="form-control" placeholder="Почта" required="" autofocus="">
    <div class="text-danger" v-if="errors.password">{{ errors.password }}</div>
    <div v-else><br></div>
    <input v-model="password" type="password" id="inputPassword" class="form-control" placeholder="Пароль" required="">
    <div><br></div>
    <input v-model="passwordConfirmation" type="password" id="inputPasswordConfirmation" class="form-control" placeholder="Подтвердите пароль" required="">
    <div><br></div>
    <div class="">
      <select v-model="userRole" class="form-control" id="">
        <option value="" disabled>Выберите роль</option>
        <option value="TEACHER">Преподаватель</option>
        <option value="STUDENT">Студент</option>
        <option value="USER">Новый пользователь</option>
        <option value=""></option>
      </select>
    </div>
    <div class="checkbox mb-3">
      <label>
        <input type="checkbox" value="remember-me"> Запомнить меня
      </label>
      <label for="">
        <input type="checkbox" value="remember-me" v-model="is_consent_terms_of_use"> Принимаю политику конфиденциальности
      </label>
      <label for="">
        <input type="checkbox" value="remember-me" v-model="is_consent_privacy_policy"> Принимаю условия пользования
      </label>
    </div>
    <button class="btn btn-lg btn-primary btn-block" @click="registerUser">Зарегистрироваться</button>
    <br>
    <p class="mt-3 mb-3 text-muted">© 2021-2022</p>
  </div>
</template>

<script>
export default {
  name: "Register",
  data() {
    return {
      name: "",
      email: "",
      password: '',
      passwordConfirmation: '',
      is_consent_terms_of_use: "",
      is_consent_privacy_policy: "",
      errors: '',
      userRole: '',
    }
  },
  methods: {
    async registerUser() {
      const response = await fetch('http://laravel.test/api/register', {
        headers: {
          "Content-Type": "application/json",
          "Accept": "application/json"
        },
        method: "post",
        body: JSON.stringify({
          name: this.name,
          password: this.password,
          password_confirmation: this.passwordConfirmation,
          email: this.email,
          is_consent_privacy_policy: this.is_consent_privacy_policy ? 1 : 0,
          is_consent_terms_of_use: this.is_consent_terms_of_use ? 1 : 0,
          role: this.userRole
        })
      });
      let responseData = await response.json();
      if(response.ok) {
        let token = responseData.token
        localStorage.setItem('jwt', token);
        this.$router.push({path: '/auth/verify/'});
      } else {
        this.errors = new Object();
        let nameError = responseData.errors.name;
        let emailError = responseData.errors.email;
        let passwordError = responseData.errors.password;
        this.errors.name = nameError ? nameError[0] : null;
        this.errors.email = emailError ? emailError[0] : null;
        this.errors.password = passwordError ? passwordError[0] : null;
      }
    }
  }
}
</script>

<style scoped>

</style>
