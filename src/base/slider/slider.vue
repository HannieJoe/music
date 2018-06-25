<template>
  <div class="slider" ref="slider">
    <div class="slider-group" ref="sliderGroup">
      <slot></slot>
    </div>
    <div class="dots">
      <span class="dot" v-for="(item,index) in dots" :key="index" :class="{active:currentPageIndex===index?true:false}"></span>
    </div>
  </div>
</template>

<script>
import BScroll from "better-scroll";
import { addClass } from "@/js/dom.js";
export default {
  data() {
    return {
      dots: [],
      currentPageIndex: 0
    };
  },
  props: {
    loop: {
      type: Boolean,
      default: true
    },
    autoPlay: {
      type: Boolean,
      default: true
    },
    interval: {
      type: Number,
      default: 4000
    }
  },
  mounted() {
    setTimeout(() => {
      this._setSliderWidth();
      this._initDots();
      this._initSlider();
      if(this.autoPlay){
        this._play();
      }
    }, 20);

    // 监听窗口改变事件 重置sliderWidth
    window.addEventListener('resize',()=>{
      if(!this.slider){
        return;
      }
      this._setSliderWidth(true);
      // refresh better-scroll接口
      this.slider.refresh();
    });

  },
  methods: {
    _setSliderWidth(isResize) {
      this.children = this.$refs.sliderGroup.children;
      let width = 0;
      let sliderWidth = this.$refs.slider.clientWidth;
      for (let i = 0; i < this.children.length; i++) {
        let child = this.children[i];

        addClass(child, "slider-item");
        child.style.width = sliderWidth + "px";
        width += sliderWidth;
      }
      if (this.loop&&!isResize) {
        width += sliderWidth * 2;
      }
      this.$refs.sliderGroup.style.width = width + "px";
    },
    _initDots() {
      this.dots = new Array(this.children.length);
    },
    _initSlider() {
      this.slider = new BScroll(this.$refs.slider, {
        scrollX: true,
        scrollY: false,
        momentum: false,
        snap: true,
        snapLoop: this.loop,
        snapThreshold: 0.3,
        snapSpeed: 400,
        click: true
      });
      this.slider.on("scrollEnd", () => {
        let pageIndex = this.slider.getCurrentPage().pageX;
        if (this.loop) {
          pageIndex -= 1;
        }
        this.currentPageIndex = pageIndex;
        if(this.autoPlay){
          clearTimeout(this.timer);
          this._play();
        }
      });
    },
    _play() {
      const self=this;
      let pageIndex = this.currentPageIndex + 1;
      if (this.loop) {
        pageIndex += 1;
      }
      this.timer = setTimeout(() => {
        self.slider.goToPage(pageIndex,0,400);
         pageIndex = this.currentPageIndex + 1;
      }, 3000);
    }
  },
  destroyed(){
    // 销毁组件时,清除定时器
    clearTimeout(this.timer);
  },
};
</script>

<style lang="less" scoped>
@import "~@/styles/vars.less";
.slider {
  min-height: 1px;
}
.slider-group {
  position: relative;
  overflow: hidden;
  white-space: nowrap;
}
.slider-item {
  float: left;
  box-sizing: border-box;
  overflow: hidden;
  text-align: center;
}
.slider-item a {
  display: block;
  width: 100%;
  overflow: hidden;
  text-decoration: none;
}
.slider-item img {
  display: block;
  width: 100%;
}
.dots {
  position: absolute;
  right: 0;
  left: 0;
  bottom: 12px;
  text-align: center;
}
.dot {
  display: inline-block;
  margin: 0 4px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.5);
}
.dot.active {
  width: 20px;
  border-radius: 5px;
  background-color: rgba(255, 255, 255, 0.5);
}
</style>


