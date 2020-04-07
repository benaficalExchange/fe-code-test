<template>
  <div class="c-product-list">
    <v-select
      :items="filters"
      @change="
        selected => {
          selectedFilter = selected;
        }
      "
    ></v-select>

    <h1>
      SELECTED FILTER: {{ filters.find(k => k.key === selectedFilter).name }}
    </h1>

    <div class="c-product-list-container">
      <transition-group tag="div" name="list" class="c-product-list-row">
        <div
          class="c-product-list-col"
          v-for="product in products"
          :key="product.id"
        >
          <v-product :product="product"></v-product>
        </div>
      </transition-group>
    </div>
  </div>
</template>

<script>
const productItems = require("@/assets/products.json");

import VProduct from "@/components/Product";
import VSelect from "@/components/Select";

export default {
  name: "ProductList",

  components: { VProduct, VSelect },

  computed: {
    products() {
      var fitleredProducts = productItems;

      if (this.selectedFilter === "unpurchased") {
        fitleredProducts = productItems.filter(p => {
          return !p.purchased;
        });
      } else if (this.selectedFilter === "purchased") {
        fitleredProducts = productItems.filter(p => {
          return p.purchased;
        });
      } else if (this.selectedFilter === "one_time_purchases") {
        fitleredProducts = productItems.filter(p => {
          return p.type === "onetime";
        });
      } else if (this.selectedFilter === "subscriptions") {
        fitleredProducts = productItems.filter(p => {
          return p.type === "recurring";
        });
      }

      return fitleredProducts.sort((a, b) => a.order - b.order);
    }
  },

  data() {
    return {
      selectedFilter: "all",

      filters: [
        { key: "all", name: "All" },
        { key: "purchased", name: "Purchased" },
        { key: "unpurchased", name: "Unpurchased" },
        { key: "one_time_purchases", name: "One time purchases" },
        { key: "subscriptions", name: "Subscriptions" }
      ]
    };
  }
};
</script>
<style lang="scss">
$spacing: 7px;

.c-product-list {
  &-container {
    max-width: 1600px;
    margin: 0 auto;
  }
  &-row {
    margin: 0;
    display: flex;
    flex-wrap: wrap;
  }
  &-col {
    box-sizing: border-box;
    justify-content: center;
    display: flex;
    padding-left: #{$spacing};
    padding-right: #{$spacing};
    width: 100%;
    margin: 0 0 15px 0;
    @media only screen and (min-width: 601px) {
      flex: 0 0 50%;
      max-width: 50%;
    }
    @media only screen and (min-width: 801px) {
      flex: 0 0 33.3333333333%;
      max-width: 33.3333333333%;
    }
    @media only screen and (min-width: 1025px) {
      flex: 0 0 25%;
      max-width: 25%;
    }
  }
}

/*Transitions animation*/
.list-enter-active,
.list-leave-active,
.list-move {
  transition: 500ms cubic-bezier(0.59, 0.12, 0.34, 0.95);
  transition-property: opacity, transform;
}

.list-enter {
  opacity: 0;
  transform: translateX(50px) scaleY(0.5);
}

.list-enter-to {
  opacity: 1;
  transform: translateX(0) scaleY(1);
}

.list-leave-active {
  position: absolute;
}

.list-leave-to {
  opacity: 0;
  transform: scaleY(0);
  transform-origin: center top;
}
</style>
