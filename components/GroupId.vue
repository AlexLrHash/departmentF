<template>
<div class="">
  <div class="pricing-header px-3 py-3 pt-md-5 pb-md-4 mx-auto text-center">
    <h1 class="display-4">{{ group.name }}</h1>
    <p class="lead">{{ group.description }}</p>
  </div>
  <div class="row">

    <div class="table-responsive">
      <h3>Все задания</h3>
      <table class="table table-striped table-sm">
        <thead>
        <tr>
          <th>#</th>
          <th>Header</th>
          <th>Header</th>
          <th>Header</th>
          <th>Header</th>
        </tr>
        </thead>
        <tbody>
        <tr>
          <td>1,001</td>
          <td>Lorem</td>
          <td>ipsum</td>
          <td>dolor</td>
          <td>sit</td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: "GroupId",
  data() {
    return {
      group: ''
    }
  },
  async mounted() {
    await this.getGroup()
  },
  methods: {
    async getGroup() {
      const response = await fetch(`http://department.biz/api/groups/${this.$route.params.id}`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt'),
        },
      });
      if (response.ok) {
        this.group = await response.json();
        this.group = this.group.data;
      } else if (response.status == 401) {
        this.$router.push('/errors/unauthenticated');
      } else if (response.status == 403) {
        this.$router.push('/errors/permissions');
      }
    }
  }
}
</script>

<style scoped>

</style>
