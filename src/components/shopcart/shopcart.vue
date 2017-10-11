<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight': totalCount>0}">
            <i class="icon-shopping_cart" :class="{'highlight': totalCount>0}"></i>
          </div>
          
          <div v-show="totalCount>0"  class="num">{{totalCount}}</div>
        </div>
        <div :class="{'priceLight':totalCount>0}" class="price">¥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <div class="pay"  :class="{'payLight':this.totalPrice>=this.minPrice}">
          {{payDesc}}
        </div>
      </div>
    </div>
    <div class="ball-container">
      <div class="ball" transition="drop" v-for="ball in balls" v-show="ball.show"></div>
    </div>
  </div>
</template>
<script>
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
        ]
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
      }
    },
    methods: {
      drop (el) {
        console.log(el)
        for (let i = 0; i < this.balls.length; i++) {
          let ball = this.balls[i]
          if (!ball.show) {
            ball.show = true
            ball.el = el
            this.dropBalls.push(ball)
            return
          }
        }
      }
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
  }
  .shopcart .ball-container .ball .drop-transition {
    transition: all 0.4s;
  }
  .shopcart .ball-container .ball .inner {
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: rgb(0,160,220);
    transition: all 0.4s;
  }
</style>