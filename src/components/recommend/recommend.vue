<template>
    <div class="recommend">
        <div class="recommend-content">
            <div v-if="recommends.length" class="slider-wrapper">
                <slider>
                    <div v-for="item in recommends" :key="item.index">
                        <a :href="item.linkUrl">
                            <img :src="item.picUrl">
                        </a>
                    </div>
                </slider>
            </div>
            <div class="recommend-list">
                <h1 class="list-title">热门歌单推荐</h1>
                <ul></ul>
            </div>
        </div>
    </div>
</template>
<script>
import Slider from "@/base/slider/slider.vue";
import { getRecommend,getDiscList } from "@/api/recommend";
import { ERR_OK } from "@/api/config";
export default {
  data() {
    return {
      recommends: []
    };
  },
  created() {
    this._getRecommend();
    this._getDiscList();
  },
  components: { Slider },
  methods: {
    _getRecommend() {
      getRecommend().then(res => {
        if (res.code === ERR_OK) {
          console.log(res.data.slider);
          this.recommends = res.data.slider;
        }
      });
    },
    // 获取歌单列表
_getDiscList(){
  getDiscList().then(res=>{
if(res.code===ERR_OK){
  console.log(res);
}
  });
}
  }
};
</script>

<style lang="less" scoped>
@import "~@/styles/vars.less";
#recommend {
  position: fixed;
  width: 100%;
  top: 88px;
  bottom: 0;
}
.recommend-content {
  height: 100%;
  overflow: hidden;
}
.slider-wrapper {
  position: relative;
}
.list-title {
  color: @nice-gold;
  font-size: 20px;
  text-align: center;
  margin-top: 5px;
}
</style>
