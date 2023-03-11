<template>
  <div
    class="box"
    @mouseenter="MouseFun('移入')"
    @mouseleave="MouseFun('移出')"
  >
    <a href="javascript:;" class="left jiantou" @click="throttle(prev)">
      <span>&lt;</span>
    </a>
    <a href="javascript:;" class="right jiantou" @click="throttle(next)">
      <span>&gt;</span>
    </a>
    <ul class="pic" :style="{ left: `-${leftVal}px`, transition: `${ition}s` }">
      <TheChart v-for="image in images" :key="image.id" :imag="image" />
      <TheChart :imag="images[0]" />
    </ul>
    <TheDots :images="images" :imgShow="imgShow" :instFun="instFun" />
  </div>
</template>

<script>
import TheChart from "./TheChart.vue";
import TheDots from "./TheDots.vue";

export default {
  name: "rotationChart",
  components: { TheChart, TheDots },
  data() {
    return {
      images: [
        {
          id: "001",
          url: require("../assets/1.jpg"),
        },
        {
          id: "002",
          url: require("../assets/2.png"),
        },
        {
          id: "003",
          url: require("../assets/3.jpg"),
        },
        {
          id: "004",
          url: require("../assets/4.png"),
        },
        {
          id: "005",
          url: require("../assets/5.png"),
        },
      ],
      leftVal: 0, // 轮播图盒子的偏移值
      flag: true, // 用来节流防止重复点击
      start: null, // 自动执行下一张定的时器
      imgWidth: 520, // 在这里填写你需要的图片宽度
      ition: 0.8, // 设置轮播图过度时间
      imgShow: 0, // 表示当前显示的图片索引
    };
  },
  mounted() {
    this.timer();
  },
  methods: {
    // 这里定义一个鼠标移入移出事件，鼠标悬停时停止自动轮播，鼠标移出则重新执行自动轮播
    MouseFun(type) {
      // 停止定时器            // 重新执行定时器
      type == "移入" ? clearTimeout(this.start) : this.timer();
    },
    timer() {
      this.start = setInterval(() => {
        this.next();
      }, 4000);
    },
    // 封装的节流函数
    throttle(fun) {
      if (this.flag) {
        this.flag = false;
        fun(); // 此为模板中传递进来的prev()或next()函数
        setTimeout(() => {
          this.flag = true;
        }, 1200); // 节流间隔时间
      }
    },
    // 上一张
    prev() {
      if (this.leftVal == 0) {
        this.ition = 0; // 将过渡时间变成0，瞬间位移到最后一张图
        this.imgShow = this.images.length - 1; // 将高亮小点改为最后一张图
        this.leftVal = this.images.length * this.imgWidth;
        // 通过延时障眼法,归原过渡时间,执行真正的“上一张”函数
        setTimeout(() => {
          this.ition = 0.8;
          this.leftVal -= this.imgWidth;
        }, this.ition * 1000);
      } else {
        // 判断显示的图片不是第一张时执行
        this.ition = 0.8;
        this.leftVal -= this.imgWidth;
        this.imgShow--;
      }
    },
    next() {
      if (this.leftVal == (this.images.length - 1) * this.imgWidth) {
        this.ition = 0.8;
        this.leftVal += this.imgWidth;
        this.imgShow = 0;
        setTimeout(() => {
          this.ition = 0;
          this.leftVal = 0;
        }, this.ition * 1000);
      } else {
        // 判断显示的图片不是最后一张时执行
        this.ition = 0.8;
        this.leftVal += this.imgWidth;
        this.imgShow++;
      }
    },
    // 点击小圆点
    instFun(index) {
      this.ition = 0.8;
      this.leftVal = index * this.imgWidth;
      this.imgShow = index;
    },
  },
};
</script>

<style scoped>
.box {
  position: relative;
  overflow: hidden;
  margin: 100px auto;
  width: 520px;
  height: 280px;
  border-radius: 15px;
}

.jiantou {
  position: absolute;
  font-size: 24px;
  text-decoration: none;
  text-align: center;
  width: 25px;
  height: 30px;
  line-height: 30px;
  background: rgba(158, 154, 154, 0.7);
  color: white;
  z-index: 999;
  /* display: none; */
}

.left {
  top: 125px;
  left: 0;
  border-radius: 0 15px 15px 0;
}

.left span {
  margin-left: -6px;
}

.right {
  top: 125px;
  right: 0;
  border-radius: 15px 0 0 15px;
}

.right span {
  margin-right: -6px;
}

.pic {
  position: absolute;
  top: 0;
  left: 0;
  width: 600%;
}
</style>