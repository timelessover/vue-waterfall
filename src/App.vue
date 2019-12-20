<template>
  <div id="app">
    <vue-waterfall :imageList="imageList"/>
  </div>
</template>

<script>
import vueWaterfall from "./components/vue-waterfall.vue";
import axios from 'axios';

export default {
  name: "app",
  components: {
    vueWaterfall
  },
  data() {
    return {
      imageList:[]
    }
  },
  created () {
    this.getImage();
  },
  methods: {
    getImage() {
      axios.get('/static/mock/data-change.json') // 真实环境中，后端会根据参数group返回新的图片数组，这里我用一个静态json文件模拟
        .then(res => {
          this.imageList = res.data
        })
    }
  },

};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

a {
  color: #000;
  text-decoration: none;
  &:active {
    color: #000;
  }
}
html,
body,
#app {
  height: 100%;
}

#app {
  position: relative;
  #header {
    display: block;
    text-align: center;
    background: #000;
    color: #cccccc;
    line-height: 32px;
    font-size: 16px;
    font-weight: bold;
    letter-spacing: 2px;
    position: fixed;
    z-index: 999;
    width: 100%;
  }
  #content {
    position: absolute;
    top: 32px;
    bottom: 0;
    width: 100%;
  }
}

#app {
  overflow: auto;
  position: relative;
  .some-info {
    line-height: 1.6;
    text-align: center;
  }
}
</style>
