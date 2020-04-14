<template>
  <div id="cart">
    <p>Cart component</p>
    <img
      class="cart-img"
      alt="Cart"
      src="@/assets/cart.png"
      @click="showCart"
    />
    <div id="cartModal" class="modal">
      <div class="modal-content">
        <span class="close">&times;</span>
        <div v-if="cartProducts.length">
          <div
            class="cart-product"
            v-for="product in cartProducts"
            :key="`${product.code}_${product.color}`"
          >
            <button @click="removeProductFromCart(product)">x</button>
            <p>{{ product.code }}</p>
            <p>color: {{ product.color }}</p>
            <p>pieces: {{ product.pieces }}</p>
            <p>price: {{ product.price }}$ / per piece</p>
          </div>
          <p>Total: {{ total }}$</p>
          <button @click="emptyCart">Empty cart</button>
          <button @click="submitCart">Submit cart</button>
        </div>
        <div v-else>
          Your shop cart is so empty!
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cartProducts: {
      type: Array
    }
  },
  methods: {
    showCart() {
      // Get the modal
      var cartModal = document.getElementById("cartModal");
      // Get the <span> element that closes the modal
      var btnClose = document.getElementsByClassName("close")[0];
      // When the user clicks the button, open the modal
      cartModal.style.display = "block";
      // When the user clicks on (x), close the modal
      btnClose.onclick = function() {
        cartModal.style.display = "none";
      };
      // When the user clicks anywhere outside of the modal, close it
      window.onclick = function(event) {
        if (event.target == cartModal) {
          cartModal.style.display = "none";
        }
      };
    },
    emptyCart() {
      this.$emit("emptyCart");
    },
    removeProductFromCart(product) {
      let productIndex = this.cartProducts.findIndex(
        cartProduct =>
          product.code == cartProduct.code && product.color == cartProduct.color
      );
      this.cartProducts.splice(productIndex, 1);
      this.$emit("removeProductFromCart", product);
    },
    submitCart() {
      console.log(this.cartProducts);
      this.$emit("submitCart");
    }
  },
  computed: {
    total() {
      let total = 0;
      this.cartProducts.forEach(product => {
        total += product.pieces * product.price;
      });
      return total;
    }
  }
};
</script>

<style scoped lang="scss">
.cart-img {
  width: 5%;
  &:hover {
    cursor: pointer;
  }
}

.cart-product {
  margin: 10px 0;
  border-bottom: 1px solid #888;
}

/* The Modal (background) */
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding: 5% 0; /* Location of the box */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 90%; /* Full height - padding */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0, 0, 0); /* Fallback color */
  background-color: rgba(0, 0, 0, 0.4); /* Black w/ opacity */
}

/* Modal Content */
.modal-content {
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  border: 3px solid #888;
  width: 80%;
}

/* The Close Button */
.close {
  color: #c01c1c;
  float: right;
  font-size: 20px;
  font-weight: bold;
  &:hover {
    color: #000;
    cursor: pointer;
  }
}
</style>
