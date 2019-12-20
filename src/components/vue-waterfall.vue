<template>
  <div class="vue-waterfall">
    <div class="item-box" v-for="item in imageList_c || videoList" :key="item.src" ref="imageItem">
      <a :href="item.href">
        <img class="image" :src="item.src" :alt="item.alt" v-if="imageList">
        <video src="item.src" :poster="item.poster || ''" v-else></video>
        <div class="item-info">
          <p class="some-info">第1张图片</p>
          <p class="some-info">一些图片描述文字</p>
        </div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imageList: {
      type: Array
    },
    videoList: {
      type: Array
    },
    srcKey: {
      type: String,
      default: "src"
    }
  },
  data() {
    return {
      heightArr: [],
      imageArr: [],
      imageList_c: [],
      imgBoxEls: [],
      maxHeight: 0,
      loadedCount: 0,
      beginIndex: 0
    };
  },
  watch: {
    imageList: "preload"
  },
  mounted() {
    this.preload();
    this.$on("preload", () => {
      this.$nextTick(() => {
        this.imgBoxEls = this.$el.getElementsByClassName("item-box");
        this.waterfall();
      });
    });
  },
  methods: {
    preload() {
      this.imageList_c = [...this.imageList];
      this.imageList_c.forEach(imgItem => {
        let oImg = new Image();
        oImg.src = imgItem[this.srcKey];
        oImg.onload = () => {
          this.loadedCount++;
          imgItem.height = oImg.height;
          this.heightArr.push(oImg.height);
          if (this.loadedCount === this.imageList_c.length) {
            this.$emit("preload");
          }
        };
      });
    },
    waterfall() {
      let top,
        left,
        height,
        colWidth = this.imgBoxEls[0].offsetWidth - 1;
      if (this.beginIndex == 0) this.heightArr = []
      for (let i = 0; i < this.imageList_c.length; i++) {
        if (!this.imgBoxEls[i]) return;
        height = this.imgBoxEls[i].offsetHeight;
        if (i < 2) {
          this.heightArr.push(height);
          top = 0;
          left = i * colWidth;
        } else {
          let minHeight = Math.min(...this.heightArr); 
          let minIndex = this.heightArr.indexOf(minHeight); 
          top = minHeight;
          left = minIndex * colWidth;
          // 很关键
          this.heightArr[minIndex] = minHeight + height;
        }
        this.imgBoxEls[i].style.left = left + "px";
        this.imgBoxEls[i].style.top = top + "px";
      }

    }
  }
};
</script>

<style lang="scss" scoped>
.vue-waterfall {
  position: relative;
  display: flex;
  width: 100%;
  flex-wrap: wrap;
  > .item-box {
    position: absolute;
    width: 50%;
    display: flex;
    flex-direction: column;
    text-align: center;
    padding: 4px;
  }
}
img {
  width: 100%;
}
</style>