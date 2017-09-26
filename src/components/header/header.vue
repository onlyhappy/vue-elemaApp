<template>
  <!-- 头部 -->
   <div class="header">
    <!--主要内容-->
    <div class="content-wrapper">
       <div class="avater">
         <img width="64" height="64" :src="seller.avatar">
       </div>

       <!-- 文字描述 -->
       <div class="content">
         <!-- 标题 -->
         <div class="title">
           <span class="brand"></span>
           <span class="name">{{seller.name}}</span>
         </div>

         <!-- 描述 -->
         <div class="description">
            {{seller.description}} / {{seller.deliveryTime}}分钟送达
         </div>

         <!-- 活动 -->
         <div v-if="seller.supports" class="supports">
           <span class="supports-icon" :class="classMap[seller.supports[0].type]"></span>
           <span class="supports-text">{{seller.supports[0].description}}</span>
         </div>

         <!-- 活动个数 -->
         <div v-if="seller.supports" class="support-count" @click="showDetail" >
           <span class="count">{{seller.supports.length}}个</span>
           <i class="icon-keyboard_arrow_right"></i>
         </div>
       </div>
     </div>

     <!-- 公告栏 -->
     <div class="bulletion-wrapper" @click="showDetail" >
       <span class="bulletion-title"></span>
       <span class="bulletion-text">{{seller.bulletin}}</span>
       <i class="icon-keyboard_arrow_right"></i>
     </div>

     <!-- 背景图片 -->
     <div class="background">
       <img :src="seller.avatar" width="100%" height="100%">
     </div>
      
     <!-- 遮挡详情 -->
     <transition name="fade">
     <div v-show="detailShow" class="detail" >
       <div class="detail-wrapper clearfix">
         <div class="detail-main">
           <h1 class="name">{{seller.name}}</h1>
      <!-- <star :size="48" :score="seller.score"></star> -->
         </div>
         <div class="title">
           <div class="line"></div>
           <div class="text">优惠信息</div>
           <div class="line"></div>
         </div>
         <ul v-if="seller.supports" class="supports">
           <li class="support-item" v-for='(item,index) in seller.supports'>
           <span class="icon" :class="classMap[seller.supports[index].type]"></span>
           <span class="text">{{seller.supports[index].description}}</span>
           </li>
         </ul>
         <div class="title">
           <div class="line"></div>
           <div class="text">商家公告</div>
           <div class="line"></div>
         </div>
         <div class="bulletin">
           <p class="content">{{seller.bulletin}}</p>
         </div>
      </div>
        <!-- 关闭按钮 -->
       <div class="detail-close" @click="hideDetail">
         <i class="icon-close"></i>
       </div>
     </div>
     </transition>
   </div>
</template>
<script type="text/ecmascript-6">
// import star from '../star/star.vue'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail () {
      this.detailShow = true
    },
    hideDetail () {
      this.detailShow = false
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  }
}
</script>
<style>
    html,body {
      line-height: 1;
      font-weight: 200;
      font-family: 'Avenir', Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
    }

    .header {
      color: #fff;
      position: relative;
      overflow:hidden;
      background-color: rgba(7, 17,27, 0.2)
    }
    .header .content-wrapper {
      position: relative;
      padding:24px 12px 18px 24px;
    }
    .header .content-wrapper .avater {
      display: inline-block;
      vertical-align:top;
    }
    .header .avater img {
      border-radius: 5px;
    }
    .header .content{
      display: inline-block;
      font-size: 14px;
    }
    .header .content .title {
      margin: 2px 0 8px 0;
    }

    .header .content .title .brand {
     display: inline-block;
     width: 30px;
     height: 18px;
     background-image: url('./img/brand@2x.png') ;
     background-size: 30px 18px;
    }
    @media (-webki-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3){
      .header .title .brand {
        width: 45px;
        height: 27px;
        background-image: url('./img/brand@3x.png') ;
        background-size: 45px 27px;
      }
    }
    .header .title .name {
      margin-left: 6px;
      font-size: 16px;
      font-weight: bold;
      line-height: 18px;
    }
    .header .content .description {
      maring-bottom: 10px;
      font-size: 14px;
    }
    .content-wrapper .supports .supports-icon {
      display: inline-block;
      width: 12px;
      height: 12px;
      margin-top: 3px;
      margin-right: 4px;
      background-size: 12px 12px;
      background-repeat: no-repeat;
      vertical-align: top;
    }
   .content-wrapper .supports .decrease {
      background-image: url('./img/decrease_1@2x.png');
    }
    .content-wrapper .supports .discount {
      background-image: url('./img/discount_1@2x.png');
    }
    .content-wrapper .supports .guarantee {
      background-image: url('./img/guarantee_1@2x.png');
    }
    .content-wrapper .supports .invoice {
      background-image: url('./img/invoice_1@2x.png');
    }
    .content-wrapper.supports .special {
      background-image: url('./img/special_1@2x.png');
    }
    .content-wrapper.supports .supports-text {
      font-size: 10px;
      line-height: 12px;
    }
    .content .support-count {
      position: absolute;
      right: 12px;
      bottom: 14px;
      padding: 0 8px;
      heght: 24px;
      line-height: 24px;
      border-radius: 14px;
      background: rgba(0,0,0,0.2);
      text-align:center;
    }
    .content .support-count .count{
      font-size: 10px;
    }
    .header .support-count .icon-keyboard_arrow_right{
      margin-top: 4px;
      line-height: 24px;
      font-size: 14px;
    }
    .bulletion-wrapper{
      height: 28px;
      line-height: 28px;
      padding: 0 22px 0 12px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      font-size: 12px;
      position: relative;
      background-color: rgba(7, 17, 27, 0.2);
    }
    .bulletion-wrapper .bulletion-title{
      display: inline-block;
      width: 24px;
      height: 12px;
      background-size: 24px 12px;
      background-image: url('./img/bulletin@3x.png');
    }
    .bulletion-wrapper .bulletion-text{
      vertical-align: top;
      margin: 0 4px;
      font-size: 10px;
    }
    .bulletion-wrapper .icon-keyboard_arrow_right{
      position: absolute;
      font-size: 10px;
      right: 12px;
      top: 8px;
    }
    .header .background{
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      filter: blur(10px);
    }
    .detail{
      position: fixed;
      z-index: 100;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%; 
      background-color: rgba(7, 17, 27, 0.8);
      z-index: 100;    
      overflow: auto;
      backdrop-filter: blur(10px);
      transition: all 0.5s;
    }
    .header  .fade-enter-active {
    transition: all .5s ease;
    }
    .header .fade-leave-active {
      transition: all .5s ease;
    }
    .header .fade-enter, .fade-leave-active {
      transition: all .5s ease;
      opacity: 0;
    }
    .detail-wrapper{
      min-height: 100%;
      width: 100%;
    }
    .detail-wrapper .detail-main {
      margin-top: 64px;
      padding-bottom: 64px;
    }
    .detail-wrapper .detail-main .name{
      line-height: 16px;
      text-align: center;
    }
    .detail-wrapper .title{
      width: 80%;
      display: flex; 
      margin: 28px auto 24px auto;
    }
    .detail-wrapper .title .line{
      flex: 1;  
      position: relative;
      top: -6px;
      border-bottom: 1px solid rgba(255,255,255,0.2)
    }
    .detail-wrapper .title .text{
       padding: 0 12px;
       font-size: 14px;
       font-weight: 700;
    }
    .detail-wrapper .supports{
      width: 80%;
      margin: 0 auto;
      padding:0;
    }
    .detail-wrapper .supports .support-item{
      padding: 0 12px;
      margin-bottom: 12px;
      font-size: 0;
      &:last-child {
        margin-bottom: 0;
      }
    }
    .detail-wrapper .support-item .icon{
       display: inline-block;
       width: 16px;
       height: 16px;
       vertical-align: top;
       margin-right: 6px;
       background-size: 16px 16px;
       background-repeat: no-repeat;
    }
    .detail-wrapper .support-item .text{
       display: inline-block;
       line-height: 12px;
       font-size: 12px;
       color: #FFF;
    }
      .support-item .decrease {
    background-image: url('./img/decrease_2@2x.png');
  }
   .support-item .discount {
    background-image: url('./img/discount_2@2x.png');
  }
   .support-item .guarantee {
    background-image: url('./img/guarantee_2@2x.png');
  }
   .support-item .invoice {
    background-image: url('./img/invoice_2@2x.png');
  }
   .support-item .special {
    background-image: url('./img/special_2@2x.png');
  }

    .detail-wrapper .bulletin{
      width: 80%;
      margin: 0 auto;
    }
    .detail-wrapper .bulletin .content{
      padding: 0 12px;
      line-height: 24px;
      font-size: 12px;
    }

    .detail-close{
      position: relative;
      width: 32px;
      height: 32px;
      margin: -64px auto 0 auto;
      clear: both;
      font-size: 32px;
    }
</style>
