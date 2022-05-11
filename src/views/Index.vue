<template>
  <div class="w-100 h-100">
    <div>
      <h1 class="text-center my-4">Zoom Playground</h1>
      <div class="row mx-0 justify-content-center">
        <div class="col-6">
          <div class="card shadow border-0">
            <div class="card-body">
              <h4 class="text-center mt-2 mb-5">
                Inputan Zoom
              </h4>
              <form action="" @submit.prevent="submitEvent">
                <div class="form-group row align-items-center mx-0 mb-3">
                  <div class="col-4">
                    <label for="name" class="">Name</label>
                  </div>
                  <div class="col-8">
                    <input type="text" v-model="formInput.nama" placeholder="Masukkan Nama" required name="name" id="name" class="form-control">
                  </div>
                </div>
                <div class="form-group row align-items-center mx-0 mb-3">
                  <div class="col-4">
                    <label for="idMeeting" class="">ID Meeting Zoom </label>
                    <div>
                      <small>Ex : 89779482681</small>
                    </div>
                  </div>
                  <div class="col-8">
                    <input type="number" name="idMeeting" v-model="formInput.idMeeting" required placeholder="Masukkan Id Meeting" id="idMeeting" class="form-control">
                  </div>
                </div>
                <div class="form-group row align-items-center mx-0 mb-3">
                  <div class="col-4">
                    <label for="passwordMeeting" class="">Password Meeting Zoom</label>
                    <div>
                      <small>Ex: 123456</small>
                    </div>
                  </div>
                  <div class="col-8">
                    <input type="password" v-model="formInput.passwordMeeting" name="passwordMeeting" required id="passwordMeeting" placeholder="Masukkan Password Meeting" class="form-control">
                  </div>
                </div>
                <div class="form-group row align-items-center mx-0 mb-3">
                  <div class="col-4">
                    <label for="roleMeeting" class="">Role</label>
                  </div>
                  <div class="col-8">
                    <select v-model="formInput.role" name="roleMeeting" required class="form-select" id="roleMeeting">
                      <option value="" selected> Pilih Role</option>
                      <option value="1">Host</option>
                      <option value="0">User</option>
                    </select>
                  </div>
                </div>
                <div class="row mx-0 my-4 justify-content-center">
                  <div class="col-auto">
                    <button class="btn btn-primary rounded px-5">Submit</button>
                  </div>
                </div>
              </form>
              <div>
                {{formInput}}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      formInput: {
        nama: '',
        idMeeting: '',
        passwordMeeting: '',
        role: ''
      }
    }
  },
  methods: {
    submitEvent() {
      // console.log('a')
      this.$router.push({
        path: '/home',
        query: {
          meetingId: this.formInput.idMeeting,
          name: this.formInput.nama,
          passwordMeeting: this.formInput.passwordMeeting,
          role: this.formInput.role
        }
      })
    }
  }

}
</script>

<style>

</style>