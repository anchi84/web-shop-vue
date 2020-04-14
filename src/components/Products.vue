<template>
  <div>
    <div class="products">
      <Product
        v-for="product in products"
        :product="product"
        :key="product.code"
        @addToCart="addToCart"
      />
    </div>
  </div>
</template>

<script>
import items from "@/db.json";
import Product from "@/components/Product";

export default {
  components: {
    Product
  },
  props: {
    removedCartProducts: {
      type: Array
    }
  },
  data() {
    return {
      products: items.products
    };
  },
  methods: {
    addToCart(product) {
      this.$emit("addToCart", product);
    }
  },
  watch: {
    removedCartProducts() {
      this.removedCartProducts.forEach(cartProduct => {
        this.products.find(
          product =>
            product.code == cartProduct.code &&
            product.subproducts.map(subproduct => {
              if (subproduct.color == cartProduct.color) {
                subproduct.addedToCart = false;
                subproduct.count += cartProduct.pieces;
              }
            })
        );
      });
    }
  },
  created() {
    // console.log(this.products);
  }
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.products {
  display: flex;
}
</style>
