<template>
  <div>
    {{ images }}
    <div>
      <carousel
        :perPageCustom="[
          [480, 1],
          [768, 1]
        ]"
        ><slide><span class="label">1</span></slide>
        <slide><span class="label">2</span></slide>
        <slide><span class="label">3</span></slide>
        <slide><span class="label">4</span></slide>
        <slide><span class="label">5</span></slide>
        <slide><span class="label">6</span></slide>
      </carousel>
    </div>
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
    console.log("hello from images", this.images);
  }
};
</script>

<style scoped lang="scss">
.VueCarousel-slide {
  position: relative;
  background: #42b983;
  color: #fff;
  font-family: Arial;
  font-size: 24px;
  text-align: center;
  min-height: 100px;
}

.label {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
