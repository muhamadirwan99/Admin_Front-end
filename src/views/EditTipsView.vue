<template>
  <div class="content">
    <div class="row">
      <div class="col-md-12">
        <div class="card">
          <div class="card-header">
            <h4 class="card-title">Edit Tips And Trick</h4>
          </div>
          <div class="card-body">
            <form @submit.prevent="update()">
              <div class="mb-3">
                <label for="" class="form-label">Kategori</label>
                <select
                  class="form-select"
                  aria-label="Default select example"
                  v-model="tips.category"
                >
                  <option value="One">One</option>
                  <option value="Two">Two</option>
                  <option value="Three">Three</option></select
                ><br />
                <label for="" class="form-label">Judul</label>
                <input type="text" class="form-control" v-model="tips.name" />
                <div v-if="validation.name" class="text-danger">
                  {{ validation.name }}
                </div>
              </div>
              <div class="mb-3">
                <label for="" class="form-label">Deskripsi</label>
                <input type="text" class="form-control" v-model="tips.desc" />
                <div v-if="validation.desc" class="text-danger">
                  {{ validation.desc }}
                </div>
              </div>
              <button class="btn btn-primary">Submit</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, ref, onMounted } from "vue";
import { useRouter, useRoute } from "vue-router";
import axios from "axios";

export default {
  setup() {
    // data binding
    let tips = reactive({
      category: "",
      name: "",
      desc: "",
    });

    const validation = ref([]);

    const router = useRouter();
    const route = useRoute();

    onMounted(() => {
      axios
        .get(`http://34.128.78.90:5000/api/tips/${route.params.id}`)
        .then((result) => {
          tips.category = result.data.data.category;
          tips.name = result.data.data.name;
          tips.desc = result.data.data.desc;
        })
        .catch((err) => {
          console.log(err);
        });
    });

    function update() {
      const formData = new FormData();
      formData.append("category", tips.category);
      formData.append("name", tips.name);
      formData.append("desc", tips.desc);
      axios
        .put(`http://34.128.78.90:5000/api/tips/${route.params.id}`, formData)
        .then(() => {
          router.push({
            name: "tips",
          });
        })
        .catch((err) => {
          validation.value = err.response.data;
        });
    }

    return {
      tips,
      validation,
      router,
      update,
    };
  },
};
</script>
