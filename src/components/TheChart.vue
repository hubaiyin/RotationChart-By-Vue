<template>
  <li>
    <a href="#">
      <img :src="img_src" alt="" />
    </a>
  </li>
</template>

<script>
export default {
  name: "TheChart",
  props: ["imag"],
  data() {
    return {
      img_src: this.imag.url,
    };
  },
};
</script>

<style scoped>
li {
  float: left;
  list-style: none;
}

img {
  width: 520px;
  height: 280px;
}
</style>