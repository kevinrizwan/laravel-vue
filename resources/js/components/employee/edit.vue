<template>
  <div>
    <div class="row">
      <router-link to="/employee" class="btn btn-primary"
        >List Pegawai
      </router-link>
    </div>

    <div class="row justify-content-center">
      <div class="col-xl-12 col-lg-12 col-md-12">
        <div class="card shadow-sm my-5">
          <div class="card-body p-0">
            <div class="row">
              <div class="col-lg-12">
                <div class="login-form">
                  <div class="text-center">
                    <h1 class="h4 text-gray-900 mb-4">Ubah Pegawai</h1>
                  </div>

                  <form
                    class="user"
                    @submit.prevent="employeeUpdate"
                    enctype="multipart/form-data"
                  >
                    <div class="form-group">
                      <div class="form-row">
                        <div class="col-md-6">
                          <img
                            :src="'/' + form.photo"
                            style="height: 400px; width: 400px"
                          />
                        </div>

                        <div class="col-md-6">
                          <br />
                          <input
                            type="text"
                            class="form-control"
                            id="exampleInputFirstName"
                            placeholder="Masukkan Nama Anda"
                            v-model="form.name"
                          />
                          <small class="text-danger" v-if="errors.name">
                            {{ errors.name[0] }}
                          </small>
                          <br />
                          <input
                            type="email"
                            class="form-control"
                            id="exampleInputFirstName"
                            placeholder="Masukkan Email Anda"
                            v-model="form.email"
                          />
                          <small class="text-danger" v-if="errors.email">
                            {{ errors.email[0] }}
                          </small>
                          <br />
                          <input
                            type="text"
                            class="form-control"
                            id="exampleInputFirstName"
                            placeholder="Masukkan NIP/NIK Anda"
                            v-model="form.nid"
                          />
                          <small class="text-danger" v-if="errors.nid">
                            {{ errors.nid[0] }}
                          </small>
                          <br />
                          <input
                            type="text"
                            class="form-control"
                            id="exampleInputFirstName"
                            placeholder="Masukkan No Telp Anda"
                            v-model="form.phone"
                          />
                          <small class="text-danger" v-if="errors.phone">
                            {{ errors.phone[0] }}
                          </small>
                          <br />
                          <input
                            type="date"
                            class="form-control"
                            id="exampleInputFirstName"
                            placeholder="Masukkan Tanggal Bergabung"
                            v-model="form.joining_date"
                          />
                          <small class="text-danger" v-if="errors.joining_date">
                            {{ errors.joining_date[0] }}
                          </small>
                          <br />
                        </div>
                        <div class="col-md-5">
                          <input
                            type="file"
                            class="custom-file-input"
                            id="customFile"
                            @change="onFileSelected"
                          />

                          <small class="text-danger" v-if="errors.photo">
                            {{ errors.photo[0] }}
                          </small>
                          <label class="custom-file-label" for="customFile"
                            >Pilih Foto</label
                          >
                        </div>
                      </div>
                    </div>

                    <div class="form-group">
                      <button type="submit" class="btn btn-primary btn-block">
                        Update
                      </button>
                    </div>
                  </form>
                  <hr />
                  <div class="text-center"></div>
                  <div class="text-center"></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/javascript">
export default {
  created() {
    if (!User.loggedIn()) {
      this.$router.push({ name: "/" });
    }
  },

  data() {
    return {
      form: {
        name: "",
        email: "",
        phone: "",
        photo: "",
        newphoto: "",
        nid: "",
        joining_date: "",
      },
      errors: {},
    };
  },
  created() {
    let id = this.$route.params.id;
    axios
      .get("/api/employee/" + id)
      .then(({ data }) => (this.form = data))
      .catch(console.log("error"));
  },

  methods: {
    onFileSelected(event) {
      let file = event.target.files[0];
      if (file.size > 1048770) {
        Notification.image_validation();
      } else {
        let reader = new FileReader();
        reader.onload = (event) => {
          this.form.newphoto = event.target.result;
        };
        reader.readAsDataURL(file);
      }
    },
    employeeUpdate() {
      let id = this.$route.params.id;
      axios
        .patch("/api/employee/" + id, this.form)
        .then(() => {
          this.$router.push({ name: "employee" });
          Notification.success();
        })
        .catch((error) => (this.errors = error.response.data.errors));
    },
  },
};
</script>

<style type="text/css"></style>