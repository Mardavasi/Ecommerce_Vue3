<template>
  <div id="wrapper">
    <nav class="navbar is-dark">
      <div class="navbar-brand">
        <router-link to="/" class="navbar-item"
          ><strong>Cinco Menos</strong>
        </router-link>
        <a
          class="navbar-burger burger"
          aria-label="menu"
          aria-expanded="false"
          data-target="navbar-menu"
          @click="showMobileMenu = !showMobileMenu"
        >
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>
      <div
        class="navbar-menu"
        id="navbar-menu"
        :class="{ 'is-active': showMobileMenu }"
      >
        <div class="navbar-start">
          <div class="navbar-item">
            <form method="get" action="/search">
              <div class="field has-addons"></div>
              <div class="control">
                <input
                  type="text"
                  class="input"
                  placeholder="Encuentra lo que quieras"
                  name="query"
                />
              </div>
              <div class="control">
                <button class="button is-success">
                  <span class="icon"><i class="fas fa-search"></i></span>
                </button>
              </div>
            </form>
          </div>
        </div>
        <div class="navbar-end">
          <router-link to="/computadores" class="navbar-item"
            >Computadores</router-link
          >
          <router-link to="/televisores" class="navbar-item"
            >Televisores</router-link
          >
          <div class="navbar-item">
            <div class="buttons">
              <router-link to="/log-in" class="button is-light">
                <strong>Log in</strong>
              </router-link>

              <router-link to="/cart" class="button is-success">
                <span class="icon"><i class="fas fa-shopping-cart"></i> </span>
                <span> Cart({{ cartTotalLength }})</span>
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </nav>
    <div
      class="is-loading-bar has-text-centered"
      :class="{ 'is-loading ': $store.state.isLoading }"
    >
      <div class="lds-dual-ring"></div>
    </div>
    <section class="section"><router-view /></section>
    <footer class="footer">
      <p class="has-text-centered">Copyright (c)2024</p>
    </footer>
  </div>
</template>
<script>
export default {
  data() {
    return {
      showMobileMenu: false,
      cart: {
        items: [],
      },
    };
  },
  beforeCreate() {
    this.$store.commit("initializeStore");
  },
  mounted() {
    this.cart = this.$store.state.cart;
  },
  computed: {
    cartTotalLength() {
      let totalLenght = 0;

      for (let i = 0; i < this.cart.items.length; i++) {
        totalLenght += this.cart.items[i].quantity;
      }

      return totalLenght;
    },
  },
};
</script>

<style lang="scss">
@import "../node_modules/bulma";

.lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid #3c3c3c;
  border-color: #3c3c3c transparent #3c3c3c transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}

@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.is-loading-bar {
  height: 0;
  overflow: hidden;
  -webkit-transition: all 0.3s;
  transition: all 0.3s;

  &.is-loading {
    height: 80px;
  }
}
</style>
