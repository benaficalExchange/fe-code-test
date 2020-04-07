<template>
  <div
    class="c-product"
    :style="'background-image: url(' + product.image + ')'"
  >
    <div class="c-product-tab">
      <div>
        <h3 class="c-product-tab-title" v-text="product.title"></h3>
        <div class="c-product-tab-price">
          <span
            :class="{ '-discount-fade': product.discount }"
            v-text="price"
          ></span
          >&nbsp;
          <span class="-discount" v-if="product.discount && price !== 0"
            >&nbsp; <span v-text="discount"></span
          ></span>
        </div>
        <div
          class="c-product-tab-description"
          v-text="product.description"
        ></div>
      </div>
      <div>
        <div class="c-product-tab-avatar"></div>
      </div>
    </div>
  </div>
</template>

<script>
import numeral from "numeral";

export default {
  name: "Product",
  props: {
    product: {
      required: true
    }
  },
  computed: {
    price() {
      return this.formatValue(this.product.price);
    },

    discount() {
      if (this.product.discount) {
        return this.formatValue(
          this.product.price -
            this.product.price * numeral(this.product.discount).value()
        );
      }

      return null;
    }
  },
  methods: {
    formatValue(price) {
      return "£" + numeral(price / 100).format("0,0.00");
    }
  }
};
</script>
<style lang="scss">
.c-product {
  width: 100%;
  max-width: 465px;
  height: 350px;
  background-size: cover;
  background-position: center;
  position: relative;
  overflow: hidden;
  border-radius: 8px;
  transition: all 0.3s ease-in-out;

  &:hover {
    cursor: pointer;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
  }

  &-tab {
    display: flex;
    justify-content: space-between;
    background-color: #282828;
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    min-height: 100px;
    padding: 16px;
    box-sizing: border-box;
    text-align: left;

    &-title,
    &-price {
      color: #ffffff;
      font-size: 16px;
      font-weight: 600;
      margin: 0 0 8px;
    }

    &-price {
      .-discount {
        color: #00ff50;
      }

      .-discount-fade {
        opacity: 0.25;
      }
    }

    &-description {
      color: #bdbdbd;
      font-size: 12px;
    }

    &-avatar {
      width: 32px;
      height: 32px;
      background-image: url("https://picsum.photos/id/1/32/32");
      border-radius: 100px;
    }
  }
}
</style>
