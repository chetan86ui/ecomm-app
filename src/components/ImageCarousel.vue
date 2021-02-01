<template>
  <div>{{ images }}</div>
</template>

<script>
export default {
  name: "ImageCarousel",
  props: {
    images: Array
  },
  mounted() {
    console.log("hello", this.images);
  }
};
</script>

<style scoped lang="scss"></style>
