<template>
   <div class="ratings">
     <!-- 内容 -->
     <div class="ratings-content">
       <div class="overview">
         <div class="overview-left">
           <h1 class="score">{{ratings.score}}</h1>
           <div class="title">综合评分</div>
           <div class="rank">高于周边商家:{{ratings.rankRate}}%</div>
         </div>
         <div class="overview-right"></div>
       </div>
     </div>
   </div>
</template>
<script type="text/ecmascript-6">
 export default {
   props: {
     ratings: {
       type: Object
     }
   }
 }
</script>
<style>
  .ratings {
    position:absolute;
    top: 176px;
    bottom: 0;
    left: 0;
    width: 100%;
    overflow: hidden;
  }
  .ratings .overview {
    display: flex;
    padding: 18px 0;
  }
  .ratings .overview .overview-left {
    flex: 0 0 137px;
    width: 137px;
    border-right: 1px solid rgba(7, 17, 27,0.2);
    text-align: center;
  }
  .ratings .overview-left .score {
    margin-bottom: 6px;
    line-height: 28px;
    font-size: 24px;
    color: rgba(255,253,0)
  }
  .ratings .overview-left .title {
    line-height: 12px;
    font-size: 12px;
    color: rgba(7,17,27);
  }
  .ratings .overview-left .rank {
    line-height: 10px;
    font-size: 10px;
    color: rgb(147,153,159);
  }

  .ratings .overview .overview-right{
    flex: 1;
    padding-left: 24px;
  }
  .ratings .ratings-content {
    
  }
</style>