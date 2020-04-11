<template>
  <div class="product">
    <p>{{ product.name }}</p>
    <p>{{ product.description }}</p>
    <div class="product-colors">
      <button
        class="color"
        v-for="subproduct in subproducts"
        :key="subproduct.color"
        :style="{ backgroundColor: subproduct.color }"
        :class="selectedSubproduct.color == subproduct.color ? 'selected' : ''"
        @click="selectColor(subproduct)"
      ></button>
      <div>{{ selectedSubproduct.price }}$ / per piece</div>
      <div>
        {{ selectedSubproduct.count }}
        {{ selectedSubproduct.count == 1 ? "piece" : "pieces" }} available
      </div>
      <button class="decrement" @click="decrement">-</button>
      <span class="count">{{ selectedSubproduct.pieces }}</span>
      <button class="increment" @click="increment">+</button>
      <div>
        <button
          class="add-to-cart"
          @click="addToCart"
          :disabled="selectedSubproduct.pieces == 0"
          :class="selectedSubproduct.pieces == 0 ? 'disabled' : ''"
        >
          Add to cart
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    product: Object
  },
  data() {
    return {
      subproducts: null,
      selectedSubproduct: null
    };
  },
  methods: {
    selectColor(subproduct) {
      this.selectedSubproduct = subproduct;
      if (
        !Object.prototype.hasOwnProperty.call(
          this.selectedSubproduct,
          "addedToCart"
        )
      ) {
        this.selectedSubproduct.addedToCart = false;
      }
      if (!this.selectedSubproduct.addedToCart) {
        this.selectedSubproduct.pieces
          ? this.selectedSubproduct.pieces
          : (this.selectedSubproduct.pieces = 0);
      } else {
        this.selectedSubproduct.pieces = 0;
      }
    },
    decrement() {
      this.selectedSubproduct.addedToCart = false;
      if (this.selectedSubproduct.pieces > 0) {
        this.selectedSubproduct.pieces--;
        this.selectedSubproduct.count++;
      }
    },
    increment() {
      this.selectedSubproduct.addedToCart = false;
      if (this.selectedSubproduct.count > 0) {
        this.selectedSubproduct.pieces++;
        this.selectedSubproduct.count--;
      }
    },
    addToCart() {
      let cartProduct = {
        code: this.product.code,
        color: this.selectedSubproduct.color,
        pieces: this.selectedSubproduct.pieces,
        price: this.selectedSubproduct.price
      };
      this.selectedSubproduct = {
        ...this.selectedSubproduct,
        ...{
          pieces: 0,
          addedToCart: true
        }
      };
      this.subproducts.find(subproduct => {
        if (subproduct.color == this.selectedSubproduct.color) {
          subproduct.pieces = 0;
          subproduct.addedToCart = true;
        }
      });
      this.$emit("addToCart", cartProduct);
    }
  },
  created() {
    this.subproducts = this.product.subproducts;
    this.selectedSubproduct = this.product.subproducts[0];
    this.selectedSubproduct.pieces = 0;
    this.selectedSubproduct.addedToCart = false;
  }
};
</script>

<style scoped lang="scss">
.product {
  border: 1px solid purple;
  margin: 1%;
  min-width: 23%;
}
.color {
  margin: 10px;
  width: 20px;
  height: 20px;
  border: 1px solid black;
  border-radius: 50%;
  &:hover {
    cursor: pointer;
  }
}

.selected {
  width: 25px;
  height: 25px;
}
.product-colors {
  padding: 10px;
}
.increment,
.decrement {
  margin: 0 5px;
  &:hover {
    cursor: pointer;
  }
}
.add-to-cart {
  width: 50%;
  margin: 10px 0;
  cursor: pointer;
  &.disabled {
    cursor: not-allowed;
  }
}
</style>
