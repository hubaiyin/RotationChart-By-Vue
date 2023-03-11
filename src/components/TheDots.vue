<template>
  <ul class="dots">
    <li
      v-for="(image, index) in images.length"
      :key="index"
      :class="[index == imgShow ? 'current' : '']"
      @click="change(index)"
    ></li>
  </ul>
</template>

<script>
export default {
  name: "TheDots",
  props: ["images", "imgShow", "instFun"],
  methods: {
    change(index) {
      console.log(index);
      this.instFun(index);
    },
  },
};
</script>

<style scoped>
.dots {
  position: absolute;
  bottom: 10px;
  left: 50%;
  margin-left: -35px;
  /* width: 70px; */
  height: 13px;
  background: rgba(255, 255, 255, 0.3);
  border-radius: 6.5px;
}

.dots > li {
  float: left;
  width: 8px;
  height: 8px;
  background-color: #fff;
  border-radius: 50%;
  margin: 3px;
  cursor: pointer;
  list-style: none;
}

.dots > .current {
  background-color: #ff5000;
}
</style>