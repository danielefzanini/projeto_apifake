<template>
  <div>
    <div class="row">
      
    </div>
    <div style="" class="row row-cols-1 row-cols-sm-2 row-cols-lg-4">
      <div v-for="product in products" :key="product.id" class="mt-2 mb-2 col">
        <div class="card p-2 shadow" aria-hidden="true">
          <img
            :src="product.images[0]"
            class="img-fluid"
            alt="..."
            style="margin: 0 auto; width: 16rem; height: 16rem"
          />
          <div class="card-body">
            <h5 class="card-title placeholder-glow" v-if="!showPlaceholder">
              {{product.title }}
            </h5>
            <span class="placeholder col-12" v-if="showPlaceholder"></span>
            <p class="display-6 text-secondary" v-if="!showPlaceholder">
              R$ {{ product.price }}
            </p>
            <p class="bg-light rounded p-1">
              Avaliação: {{ product.rating }}
            </p>
            <button
              type="button"
              class="btn btn-info"
              data-bs-toggle="modal"
              data-bs-target="#staticBackdrop"
              @click="getProduct(product.id)"
            >
              Informações
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal -->
    <div
      class="modal fade"
      id="staticBackdrop"
      data-bs-backdrop="static"
      data-bs-keyboard="false"
      tabindex="-1"
      aria-labelledby="staticBackdropLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="staticBackdropLabel">
              {{ product.title }}
            </h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <img
              :src="product.images[0]"
              alt=""
              style="width: 16rem; display: block; margin: 2rem auto"
            />
            <p>{{ product.description }}</p>
            
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Fechar
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import api from "@/api-conf.json";
import { onMounted, reactive, ref } from "vue";
import axios from "axios";

const url = api.baseURL;

const products = ref([]);




const showPlaceholder = ref(true);

const product = reactive({
  id: null,
  title: "",
  price: 0,
  description: "",
  images: ["...", "...", "..."],
  rating: 0,
});

const getAllProducts = () => {[]
  axios
    .get(url + "/products?sort=asc")
    .then((response) => {
      products.value = response.data.products;
    })
    .catch((error) => {
      alert("Erro: " + error.response.data);
    });

  setTimeout(() => {
    showPlaceholder.value = false;
  }, 2000);
};

const getProduct = (id) => {
  axios
    .get(url + "/products/" + id)
    .then((response) => {
      product.id = response.data.id;
      product.title = response.data.title;
      product.description = response.data.description;
      product.rating = response.data.rating;
      product.images = response.data.images;
    })
    .catch((error) => {
      alert("Erro: " + error.response.data);
    });
};



onMounted(() => {
  getAllProducts();
  });
</script>

<style lang="scss" scoped></style>
