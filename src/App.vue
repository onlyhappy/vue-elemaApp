<template>
  <div id='app'>
  <!-- 头部 -->
  <v-header :seller="seller"></v-header>
   <!-- 导航 -->
   <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
  <router-view></router-view>
  </div>
</template>
<script type="text/ecmascript-6">
  import header from './components/header/header.vue'
  const ERR_OK = 0
  export default {
    data () {
      return {
        seller: {}
      }
    },
    created () {
      this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
        response = response.body
        if (response.errno === ERR_OK) {
          this.seller = Object.assign({}, this.seller, response.data)
        }
      })
    },
    components: {
      'v-header': header
    }
  }
</script>

<style>
     #app {
      line-height:1;
      font-weight: 200;
       font-family: 'Avenir', Helvetica, Arial, sans-serif;
       -webkit-font-smoothing: antialiased;
       -moz-osx-font-smoothing: grayscale;
     }
     .tab {
       display: flex;
       width: 100%;
       height: 40px;
       line-height: 40px;
       text-align: center;
       position: relative;
      /* border-bottom: 1px solid rgba(7, 17, 27, 0.1); */
     }
     /* 解决移动端一像素问题 */
     .tab::after {
       position: absolute;
       display: block;
       content: ' ';
       left: 0;
       bottom: 0;
       width: 100%;
       border-top: 1px solid rgba(7, 17, 27,0.1);
     }
     .tab-item{
       flex: 1;
        text-align: center;
     }
     .tab-item a {
       display: block;
       text-decoration: none;
       font-size: 14px;
       color: rgb(77, 85, 93);
     }
     .tab-item a.router-link-active {
       color: rgb(240, 20, 20);
     }
</style>
