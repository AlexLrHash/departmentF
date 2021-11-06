<template>
  <div class="container">
    <Header></Header>
    <h3>На вашу почту отправлено сообщение с кодом</h3>
    <div class="text-danger ml-3" v-if="verificationError">
      {{ verificationError }}
    </div>
    <div v-else><br></div>
    <div class="row ml-2">
      <input type="text" class="mt-2 ml-2 col-6 form-control" required placeholder="Введите верификационный номер" v-model="verifyToken">
      <button type="submit" @click="verify" class="mt-2 ml-2 btn btn-outline-primary">Подтвердить</button>
    </div>
    <div class="ml-3 mt-3">
      <h6>Не пришло сообщение? </h6>
      <button type="submit" @click="resendEmail" class="btn btn-secondary">Отправить повторно</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "verify",
  data() {
    return {
      verifyToken: '',
      token: '',
      email: "",
      verificationError: ""
    }
  },
  mounted() {
  },
  methods: {
    async verify() {
      const response = await fetch('http://department.biz/api/user/token/verify/', {
        method: "post",
        headers: {
          'Content-Type': "application/json",
          'Accept': 'application/json',
          'Authorization': "Bearer " + localStorage.getItem('jwt'),
        },
        body: JSON.stringify({
          verify_token: this.verifyToken,
        })
      });
      let responseData = await response.json();

      if (response.ok) {
        this.$router.push({path: '/'});
      } else if (response.status == 422) {
        this.verificationError = responseData.errors.verify_token[0]
      } else {
        this.verificationError = responseData.message;
      }
    },
    async resendEmail() {
      const response = await fetch('http://department.biz/api/user/token/verify/resend', {
        method: "POST",
        headers: {
          'Content-Type': "application/json",
          'Accept': 'application/json',
          'Authorization': "Bearer " + localStorage.getItem('jwt'),
        }
      });
      let responseData = await response.json();
      if (response.ok) {
        console.log(responseData);
      }
    }
  }
}
</script>

<style scoped>

</style>
