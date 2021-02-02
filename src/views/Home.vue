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
        <a @click="getcarousel(product.images)">
          <img :alt="product.hero.alt" :src="product.hero.href" />
        </a>
        <ImageCarousel :images="productImages" />
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
      productImages: ""
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
        console.log("response", response.data);
        this.productList = response.data.groups;
      })
      .catch(error => {
        console.warn("Not good man", error);
        this.productList = this.response.groups;
        console.log(this.productList);
      });
  },
  computed: {
    productLists() {
      console.log("helo", this.productList);
      console.log("productImages", this.productImages);
      return this.productList;
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
      return images;
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
      width: 100%;
      cursor: pointer;
      img {
        width: 100%;
      }
    }
  }
}
</style>
