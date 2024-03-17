<template>
  <div class="home"></div>
  <section class="hero is-medium is-dark mb-6">
    <div class="hero-body has-text-centered">
      <p class="title mb-6">Bienvenido a Cinco Menos</p>
      <p class="subtitle">La mejor tienda online de colombia</p>
    </div>
  </section>
  <div class="columns is-multiline">
    <div class="column is-12">
      <h2 class="is-size-2 has-text-centered">latest products</h2>
    </div>

    <product-box
      v-for="product in latestProducts"
      :key="product.id"
      :product="product"
    ></product-box>
  </div>
</template>

<script>
import axios from "axios";
import ProductBox from "@/components/ProductBox.vue";

export default {
  name: "Home",
  data() {
    return {
      latestProducts: [],
    };
  },
  components: {
    ProductBox,
  },
  mounted() {
    this.getLatestProducts();
    document.title = " Home | Cincomenos";
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit("setIsLoading", true);
      await axios
        .get("/api/v1/latest-products/")
        .then((response) => {
          this.latestProducts = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
      this.$store.commit("setIsLoading", false);
    },
  },
};
</script>
