<template>
  <div class="home">
    <img class="logo" alt="web shop logo" src="@/assets/logo.png" />
    <h1>Welcome to car toys web shop</h1>
    <Products @addToCart="addToCart" />
    <Cart :products="products" />
  </div>
</template>

<script>
// @ is an alias to /src
import Products from "@/components/Products.vue";
import Cart from "@/components/Cart.vue";

export default {
  components: {
    Products,
    Cart
  },
  data() {
    return {
      products: []
    };
  },

  methods: {
    addToCart(product) {
      let index = this.products.findIndex(
        item => item.code == product.code && item.color == product.color
      );

      if (index != -1) {
        this.products[index].pieces += product.pieces;
      } else {
        this.products.push(product);
      }
    }
  }
};
</script>

<style scoped>
.logo {
  width: 15%;
  margin: 10px;
}
</style>
