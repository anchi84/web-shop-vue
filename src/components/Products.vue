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
  }
};
</script>

<style scoped lang="scss">
.products {
  display: flex;
  flex-direction: column;
  @media (min-width: 768px) {
    flex-direction: row;
  }
}
</style>
