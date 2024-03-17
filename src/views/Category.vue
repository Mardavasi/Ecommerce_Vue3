<template>
  <div class="page-category">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">{{ category.name }}</h2>
      </div>
      <product-box
        v-for="product in category.products"
        :key="product.id"
        :product="product"
      ></product-box>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { toast } from "bulma-toast";
import ProductBox from "@/components/ProductBox.vue";

export default {
  name: "Category",
  components: {
    ProductBox,
  },
  data() {
    return {
      category: {
        products: [],
      },
    };
  },
  mounted() {
    this.getCategory();
  },
  watch: {
    $route(to, from) {
      this.getCategory();
    },
  },
  methods: {
    async getCategory() {
      const categorySlug = this.$route.params.category_slug;
      this.$store.commit("setIsLoading", true);
      axios
        .get(`/api/v1/products/${categorySlug}/`)
        .then((response) => {
          this.category = response.data;
          document.title = this.category.name + "| Cincomenos";
        })
        .catch((error) => {
          console.log(error);

          toast({
            message: `Error loading category ${categorySlug} products`,
            type: "is-danger",
            position: "bottom-center",
            closeOnClick: true,
            pauseOnHover: true,
            duration: 3000,
            position: "bottom-right",
          });
        });
      this.$store.commit("setIsLoading", false);
    },
  },
};
</script>
