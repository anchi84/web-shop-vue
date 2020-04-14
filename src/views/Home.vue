<template>
  <div class="home">
    <img class="logo" alt="web shop logo" src="@/assets/logo.png" />
    <h1>Welcome to car toys web shop</h1>
    <Products
      @addToCart="addToCart"
      :removedCartProducts="removedCartProducts"
    />
    <Cart
      :cartProducts="cartProducts"
      @emptyCart="emptyCart"
      @removeProductFromCart="removeProductFromCart"
      @submitCart="submitCart"
    />
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
      cartProducts: [],
      removedCartProducts: []
    };
  },

  methods: {
    addToCart(product) {
      this.removedCartProducts = [];
      let index = this.cartProducts.findIndex(
        item => item.code == product.code && item.color == product.color
      );

      if (index != -1) {
        this.cartProducts[index].pieces += product.pieces;
      } else {
        this.cartProducts.push(product);
      }
    },
    emptyCart() {
      this.removedCartProducts = this.cartProducts;
      this.cartProducts = [];
    },
    removeProductFromCart(product) {
      this.removedCartProducts = [];
      this.removedCartProducts.push(product);
    },
    submitCart() {
      this.cartProducts = [];
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
