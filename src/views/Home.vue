<template>
  <div class="home">
    <div class="product-list" v-if="productLists">
      <div
        class="product"
        v-for="product in productList"
        v-bind:key="product.id"
      >
        <div class="product-name" v-html="product.name" />
        <div
          class="price"
          v-html="getProductPrice(product.priceRange || product.price)"
        />
        <div :class="hello" @click="getcarousel(product)">
          <a @click="$event.target.classList.toggle('z-index-top')">
            <img
              :alt="product.hero.alt"
              :src="product.hero.href"
              class="test"
            />
          </a>
          <ImageCarousel :images="product.images" class="image-carousel" />
          <span
            class="close-btn"
            v-bind:key="product.id"
            v-show="isActive"
            @click="closeCarousel()"
            >X
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Vue from "vue";
import axios from "axios";
import responseData from "../response.json";
import ImageCarousel from "@/components/ImageCarousel.vue";

Vue.use(axios);

export default {
  name: "Home",
  components: {
    ImageCarousel
  },
  data() {
    return {
      productList: "",
      response: responseData,
      productImages: "",
      loadImageCarousel: false,
      isActive: false
    };
  },
  mounted() {
    axios
      .get(
        "https://www.westelm.com/services/catalog/v4/category/shop/new/all-new/index.json",
        {
          headers: {
            "Content-Type": "x-www-form-urlencoded"
          }
        }
      )
      .then(response => {
        this.productList = response.data.groups;
      })
      .catch(() => {
        this.productList = this.response.groups;
      });
  },
  computed: {
    productLists() {
      return this.productList;
    },
    hello() {
      let className = "hello";
      return className;
    }
  },
  methods: {
    getProductPrice: function(productPrice) {
      if (productPrice) {
        let highPrice = productPrice?.selling?.high || "";
        let lowPrice = productPrice?.selling?.low || "";
        if (highPrice === "") {
          highPrice = productPrice?.regular;
          lowPrice = productPrice?.selling;
          if (highPrice === lowPrice) {
            return `$${highPrice}`;
          } else {
            return `$${lowPrice}`;
          }
        }
        return `$${lowPrice} - $${highPrice}`;
      }
    },
    getcarousel: function(images) {
      this.isActive = !this.isActive;
      console.log("product images", images);
    },
    closeCarousel: function() {
      this.isActive = !this.isActive;
    }
  }
};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.product-list {
  padding: 0px;
  margin: 0px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(20%, 1fr));
  grid-gap: 1rem;
  .product {
    position: relative;
    overflow: hidden;
    .product-name {
      background: rgba($color: #fff, $alpha: 0.7);
      color: #666;
      font-size: 12px;
      text-transform: uppercase;
      font-weight: bold;
      text-align: left;
      position: absolute;
      padding: 5px 15px;
      width: 100%;
      box-sizing: border-box;
    }
    .price {
      position: absolute;
      background: #666;
      color: #fff;
      padding: 3px 5px;
      font-weight: 400;
      bottom: 30px;
      left: 10px;
    }
    a {
      display: inline-block;
      position: relative;
      width: 100%;
      cursor: pointer;
      img {
        width: 100%;
      }
      z-index: 10;
    }
    .image-carousel {
      position: absolute;
      height: 100%;
      width: 100%;
      left: 0px;
      top: 0px;
      z-index: 9;
      .VueCarousel {
        border: #000 solid 1px;
      }
    }
  }
}
.close-btn {
  background: rgba($color: #000000, $alpha: 0.5);
  color: #fff;
  padding: 5px 10px 5px 10px;
  line-height: 20px;
  vertical-align: top;
  border-radius: 100%;
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
}
.z-index-top {
  z-index: 12;
}
</style>
