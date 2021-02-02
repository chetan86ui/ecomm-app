<template>
  <div>
    <carousel
      :perPageCustom="[
        [480, 1],
        [768, 1]
      ]"
    >
      <slide v-for="(item, index) in images" v-bind:key="index">
        <img :alt="item.alt" :src="item.href" />
      </slide>
    </carousel>
  </div>
</template>

<script>
import { Carousel, Slide } from "vue-carousel";

export default {
  name: "ImageCarousel",
  props: {
    images: Array
  },
  components: {
    Carousel,
    Slide
  },
  mounted() {
  }
};
</script>

<style lang="scss">
.VueCarousel-slide {
  position: relative;
  background: #42b983;
  color: #fff;
  font-family: Arial;
  font-size: 24px;
  text-align: center;
  min-height: 100px;
  img {
    width: 100%;
    height: 100%;
  }
}
.VueCarousel-pagination {
  background: rgba($color: #000000, $alpha: 0.5);
  position: relative;
  top: 48vh;
}
.VueCarousel-dot-container {
    margin-top: 0px !important;
}

.VueCarousel-dot {
    margin-top: 0px !important;
}

.label {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
