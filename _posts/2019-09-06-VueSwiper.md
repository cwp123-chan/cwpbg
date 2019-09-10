---
title: swiper
date: 2019-09-06 23:40:49
---
# swiper

## 1. 在vue中使用swiper

```vue
//代码演示
<template>
  <div class="scroll">
    <swiper :options="swiperOption" ref="mySwiper">
    <swiper-slide>Slide 1</swiper-slide>
    <swiper-slide>Slide 2</swiper-slide>
    <swiper-slide>Slide 3</swiper-slide>
    <swiper-slide>Slide 4</swiper-slide>
    </swiper> 
    <!-- 分页器 -->
    <div class="swiper-pagination" slot="pagination"></div>
    <!-- 左右箭头 -->
    <div class="swiper-button-prev" slot="button-prev"></div>
    <div class="swiper-button-next" slot="button-next"></div>
  </div>
</template>

<script>
  // 导入 swiper插件
  import {swiper,swiperSlide} from 'vue-awesome-swiper'
  // 如果是 2.6版本以上 需要引入 swiper.css文件
  import 'swiper/dist/css/swiper.css'
export default {
    components : {
      swiper,
      swiperSlide
    },
    data(){
      return {
      // 如果要使用 swiper样式就必须实例化 swiperOption对象
          swiperOption:{
            // notNextTick 是为了获取当前轮播图的具体信息,如果为false 那么会参数错误
            notNextTick:true,
            //loop 轮播图是否循环
            loop:true,
            // 初始化索引值
            initialSlide:0,
            //自动播放
            // autoplay:true,
            autoplay : {
              // 等待时间
              delay:2000,
              //鼠标放入之后轮播图是否静止
              disableOnInteraction:true
            },
            // 轮播样式
            effect:'flip',
            // 规定左右箭头
            navigation:{
              nextEl :'.swiper-button-next',
              prevEl :'.swiper-button-prev'
            },
            pagination:{
              el:'.swiper-pagination',
              //是否可以被点击
              clickable:true
            }
          },
          // 声明swiperslide都有哪些
          swiperSlides:[1,2,3,4]
      }
    },
      computed:{
        swiper(){
            //  插件固定用法 初始化插件
            return this.$refs.mySwiper.swiper
        }
      }

};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">
.swiper-slide{
height: 200px;
}
</style>

```

https://github.com/surmon-china/vue-awesome-swiper			在vue中使用 swiper