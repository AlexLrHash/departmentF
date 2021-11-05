<template>
  <div class="container" style="margin-top: 100px;">
    <table class="table table-striped table-sm">
      <thead>
      <tr>
        <th>#</th>
        <th>Кто лайкнул</th>
        <th>Кого лайкнули</th>
        <td></td>
      </tr>
      </thead>
      <tbody v-if="likes.length">
      <tr v-for="like in likes">
        <td></td>
        <td>{{ like.user.name }}</td>
        <td>{{ like.foreign.name }}</td>
        <td>
          <button class="btn btn-danger" @click="openDeleteModal(like.id)"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-file-earmark-x-fill" viewBox="0 0 16 16">
            <path d="M9.293 0H4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V4.707A1 1 0 0 0 13.707 4L10 .293A1 1 0 0 0 9.293 0zM9.5 3.5v-2l3 3h-2a1 1 0 0 1-1-1zM6.854 7.146 8 8.293l1.146-1.147a.5.5 0 1 1 .708.708L8.707 9l1.147 1.146a.5.5 0 0 1-.708.708L8 9.707l-1.146 1.147a.5.5 0 0 1-.708-.708L7.293 9 6.146 7.854a.5.5 0 1 1 .708-.708z"/>
          </svg>
          </button>
        </td>
      </tr>
      </tbody>
      <tbody v-else>
        <tr>Пока нет лайков</tr>
      </tbody>
    </table>
    <b-modal id="bv-modal-example" v-model="showDeleteModal" hide-footer>
      <template #modal-title>
        Удаление лайки
      </template>
      <div class="d-block text-center">
        Вы действительно хотите удалить?
      </div>
      <div class="row text-center">
        <b-button class="col-5 ml-4 mr-2 mt-3 btn btn-danger" block @click="deleteLike">Удалить лайк</b-button>
        <b-button class="col-5 ml-2 mt-3" block @click="showDeleteModal = false">Отмена</b-button>
      </div>
    </b-modal>
    </div>
</template>

<script>
export default {
  name: "AdminLikes",
  data() {
    return {
      likes: '',
      likeId: '',
      showDeleteModal: false
    }
  },
  async mounted() {
    const response = await fetch(`http://department.biz/api/admin/teachers/likes`, {
      headers: {
        'Content-Type': 'application/json',
        "Accept": "application/json",
        'Authorization': "Bearer " + localStorage.getItem('jwt')
      },
    });
    if (response.status == 500) {

    } else {
      this.likes = await response.json();
      this.likes = this.likes.data;
    }
  },
  methods: {
    openDeleteModal(id) {
      this.showDeleteModal = true
      this.likeId = id
    },
    async deleteLike() {
      const response = await fetch(`http://department.biz/api/admin/teachers/likes/delete/${this.likeId}`, {
        headers: {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          'Authorization': "Bearer " + localStorage.getItem('jwt')
        },
        method: 'post',
      })
      this.showDeleteModal = false;
      const responseData = await response.json();
      for(let i = 0; i < this.likes.length; i++) {
        if(this.likes[i].id == this.likeId) {
          this.likes.splice(i, 1);
          break;
        }
      }
    }
  }
}
</script>

<style scoped>

</style>
