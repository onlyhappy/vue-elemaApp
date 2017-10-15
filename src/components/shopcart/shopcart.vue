<template>
  <div class="shopcart-box">
    <!-- 购物车 -->
    <div class="shopcart">
      <div class="content" @click="toggleList">
        <div class="content-left">
          <!-- 左侧购物车小图标 -->
          <div class="logo-wrapper">
            <div class="logo" :class="{'highlight': totalCount>0}">
              <i class="icon-shopping_cart" :class="{'highlight': totalCount>0}"></i>
            </div>
            
            <div v-show="totalCount>0"  class="num">{{totalCount}}</div>
          </div>
          <div :class="{'priceLight':totalCount>0}" class="price">¥{{totalPrice}}</div>
          <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
        </div>
        <div @click.stop.prevent="pay" class="content-right">
          <div class="pay" :class="{'payLight':this.totalPrice>=this.minPrice}">
            {{payDesc}}
          </div>
        </div>
      </div>
      <!-- 飞行小球 -->
      <div class="ball-container">
        <div transition="drop" v-for="ball in balls" v-show="ball.show" class="ball">
          <div class="inner inner-hook"></div>
        </div>
      </div>

      <transition name="fold">
        <div class="shopcart-list" v-show="listShow">
          <div class="list-header">
            <h1 class="title">购物车</h1>
            <span @click="empty()" class="empty">清空</span>
          </div>
          <div ref="listContent" class="list-content">
              <ul>
                <li v-for="food in selectFoods" class="food">
                  <span class="name">{{food.name}}</span>

                  <div class="price">
                    <span>¥{{food.price*food.count}}</span>
                  </div>

                 <div class="cartcontrol-wrapper">
                    <cartcontrol @add="addFood" :food="food"></cartcontrol>
                  </div>
                </li>
              </ul>
            </div>
        </div>
      </transition>
    </div>
   <transition name="fade">
    <div v-show="listShow" @click="hideList" class="list-mask"></div>
  </transition>
  </div>
 
 
</template>
<script>
  import BScroll from 'better-scroll'
  import cartcontrol from '../cartcontrol/cartcontrol'
  export default {
    props: {
      selectFoods: {
        type: Array,
        default () {
          return [
            {
              price: 10,
              count: 1
            }
          ]
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    data () {
      return {
        balls: [
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          }
        ],
        dropBalls: [],
        fold: true
      }
    },
    computed: {
      totalPrice () {
        let total = 0
        this.selectFoods.forEach((food) => {
          total += food.price * food.count
        })
        return total
      },
      totalCount () {
        let count = 0
        this.selectFoods.forEach((food) => {
          count += food.count
        })
        return count
      },
      payDesc () {
        if (this.totalPrice === 0) {
          return `¥${this.minPrice}起送`
        } else if (this.totalPrice < this.minPrice) {
          return `还差¥${this.minPrice - this.totalPrice}起送`
        } else {
          return '去结算'
        }
      },
      payClass () {
        if (this.totalPrice < this.minPrice) {
          return 'not-enough'
        } else {
          return 'enough'
        }
      },
      listShow () {
        if (!this.totalCount) {
          this.fold = true
          return false
        }
        let show = !this.fold
        if (show) {
          this.$nextTick(() => {
            if (!this.scroll) {
              this.scroll = new BScroll(this.$refs.listContent, {
                click: true
              })
            } else {
              this.scroll.refresh()
            }
          })
        }
        return show
      }
    },
    methods: {
      drop (el) {
        for (let i = 0; i < this.balls.length; i++) {
          let ball = this.balls[i]
          if (!ball.show) {
            ball.show = true
            ball.el = el
            this.dropBalls.push(ball)
            return
          }
        }
      },
      beforeDrop (el) {
        let count = this.balls.length
        while (count--) {
          let ball = this.balls[count]
          if (ball.show) {
            let rect = ball.el.getBoundingClientRect()
            let x = rect.left - 32
            let y = -(window.innerHeight - rect.top - 22)
            el.style.display = ''
            el.style.webkitTransform = `translate3d(0,${y}px,0)`
            el.style.transform = `translate3d(0,${y}px,0)`
            let inner = el.getElementsByClassName('inner-hook')[0]
            inner.style.webkitTransform = `translate3d(${x}px,0,0)`
            inner.style.transform = `translate3d(${x}px,0,0)`
          }
        }
      },
      dropping (el, done) {
        /* eslint-disable no-unused-vars */
        let rf = el.offsetHeight
        this.$nextTick(() => {
          el.style.webkitTransform = 'translate3d(0,0,0)'
          el.style.transform = 'translate3d(0,0,0)'
          let inner = el.getElementsByClassName('inner-hook')[0]
          inner.style.webkitTransform = 'translate3d(0,0,0)'
          inner.style.transform = 'translate3d(0,0,0)'
          el.addEventListener('transitionend', done)
        })
      },
      afterDrop (el) {
        let ball = this.dropBalls.shift()
        if (ball) {
          ball.show = false
          el.style.display = 'none'
        }
      },
      toggleList () {
        if (this.totalCount === 0) {
          return
        }
        this.fold = !this.fold
      },
      hideList () {
        this.fold = true
      },
      empty () {
        this.selectFoods.forEach((food) => {
          food.count = 0
        })
      },
      pay () {
        if (this.totalPrice < this.minPrice) {
          return
        }
        window.alert(`支付${this.totalPrice}元`)
      },
      addFood (target) {
        this.drop(target)
      }
    },
    components: {
      cartcontrol
    }
  }
</script>
<style>
  .shopcart {
    position: fixed;
    left: 0;
    bottom: 0;
    z-index: 50;
    width: 100%;
    height: 48px;
    background: #ccc;
  }
  .shopcart .content {
    display: flex;
    background: #141d27;
    font-size: 0;
    color: rgba(255,255,255,0.4);
  }
  .shopcart .content .content-left {
    flex: 1;
  }
  .shopcart .content-left .logo-wrapper{
    display: inline-block;
    vertical-align: top;
    position: relative;
    top: -10px;
    margin: 0 12px;
    padding: 6px;
    width: 56px;
    height: 56px;
    box-sizing: border-box;
    vertical-align: top;
    border-radius: 50%;
    background: #141d27;
    text-align: center;
  }
  .shopcart .content-left .logo-wrapper .logo {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: #2b343c;
  }
 .shopcart .content-left .logo .icon-shopping_cart {
    font-size: 24px;
    color: #80858a;
    line-height: 44px;
  }
  .shopcart .content-left .logo-wrapper .highlight {
    background-color: rgb(0, 160, 220);
  }
  .shopcart .content-left .logo-wrapper .highlight .icon-shopping_cart {
    color: #fff;
  }
  .shopcart .content-left .logo-wrapper .num {
    position: absolute;
    right: 0;
    top: 0;
    width: 24px;
    height: 16px;
    line-height: 16px;
    border-radius: 12px 12px;
    font-size: 9px;
    font-weight: 700;
    color: #fff;
    background-color: rgb(240, 20, 20);
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4);
  }

  .shopcart .content-left .price {
    display: inline-block;
    vertical-align: top;
    line-height: 24px;
    margin-top: 12px;
    padding-right: 12px;
    box-sizing: border-box;
    border-right: 1px solid rgba(255,255,255,0.1);
    font-size: 16px;
    font-weight: 700;
    color: rgba(255,255,255,0.4);
  }
   .shopcart .content-left .priceLight {
    color: #fff;
  }
  .shopcart .content-left .desc {
    display: inline-block;
    vertical-align: top;
    line-height: 24px;
    margin: 12px 0 0 12px;
    font-size: 10px;
  }

  .shopcart .content .content-right {
    flex: 0 0 105px;
    width: 105px;
    text-align: center;
  }
  /* 适配 iPhone 5 */
  @media screen and (max-width: 320px) {
    .shopcart .content-right {
      flex: 0 0 80px;
      width: 80px;
      text-align: center;
    }
  }
  .shopcart .content-right .pay {
    height: 48px;
    line-height: 48px;
    text-align: center;
    font-size: 12px;
    font-weight: 700;
    background-color: #2b333b;
  }
  .shopcart .content-right .payLight {
    background-color: #00b43c;
    color: #fff;
  }
  .shopcart .ball-container .ball {
    position: fixed;
    left: 32px;
    bottom: 22px;
    z-index: 200;
    transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
  }
  .shopcart .ball-container .ball .drop-transition {
    transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
  }
  .shopcart .ball-container .inner {
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: rgb(0,160,220);
    transition: all 0.4s linear;
  }

  .shopcart .shopcart-list {
    position: absolute;
    top: 0;
    left: 0;
    z-index: -1;
    width: 100%;
    transform: translate3d(0, -100%, 0);
  }
  .shopcart .fold-enter-active {
    transition: all 0.5s;
  }
  .shopcart .fold-leave-active {
    transition: all 0.5s;
    transform: translate3d(0, 100%, 0);
  }
  .shopcart .fold-enter, .fold-leave-active {
    transform: translate3d(0, 0, 0);
  }

  .shopcart .list-header {
    height: 40px;
    line-height: 40px;
    padding: 0 18px;
    background-color: #f3f5f7;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
  }
  .shopcart .list-header .title {
    float: left;
    font-size: 14px;
    color: rgba(7, 17, 27);
  }
  .shopcart .list-header .empty {
    float: right;
    font-size: 12px;
    color: rgba(0, 160, 150);
  }
  .shopcart .list-header .title {
    float: left;
    font-size: 14px;
    color: rgb(7, 17, 27);
  }
  .shopcart .list-content {
    max-height: 217px;
    padding: 0 18px;
    background-color: #fff;
    overflow: hidden;
  }
  .shopcart .list-content .food {
    position: relative;
    padding: 12px 0;
    box-sizing: border-box;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
  }
  .shopcart .list-content .food .name {
    line-height: 24px;
    font-size: 14px;
    color: rgb(7, 17, 27);
  }
  .shopcart .list-content .food .price {
    position: absolute;
    right: 90px;
    bottom: 12px;
    line-height: 24px;
    font-size: 14px;
    font-weight: 700;
    color: rgb(240, 20, 20);
  }
  .shopcart .list-content .food .cartcontrol-wrapper {
    position: absolute;
    right: 0;
    bottom: 6px;
  }
  .list-mask {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 40;
    opacity: 1;
    backdrop-filter: blur(10px);
    background-color: rgba(7, 17, 27, 0.6);
  }
  .shopcart-box .fade-enter-active {
    transition: all .5s;
  }
  .shopcart-box .fade-leave-active {
    transition: all .5s;
  }
  .shopcart-box .fade-enter,
  .shopcart-box .fade-leave-active {
    transition: all .5s;
    background: rgba(7, 17, 27, 0);
    opacity: 0;
  }
</style>