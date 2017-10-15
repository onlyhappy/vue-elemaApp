<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}" @click="selectMenu(index,$event)" ref="menuList">
          <span class="text">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
        </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook" ref="foodList">
          <h1 class="titile">{{item.name}}</h1>
          <ul>
            <li @click="selectFood(food,$event)" v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img :src="food.icon" width="57px" height="57px">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>

                <p class="desc">{{food.description}}</p>

                <div class="extra">
                  <span>月销{{food.sellCount}}份</span><span>好评率{{food.rating}}</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>

                <div class="cartcontrol-wrapper">
                  <cartcontrol @add="addFood" :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart ref="shopcart" :selectFoods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
    <food @add="addFood" :food="selectedFood" ref="food"></food>
  </div>
</template>
<script>
  import BScroll from 'better-scroll'
  import shopcart from '../shopcart/shopcart'
  import cartcontrol from '../cartcontrol/cartcontrol'
  import food from '../food/food'
  const ERR_OK = 0
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0,
        selectedFood: {}
      }
    },
    computed: {
      currentIndex () {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i]
          let height2 = this.listHeight[i + 1]
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i
          }
        }
        return 0
      },
      selectFoods () {
        let foods = []
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food)
            }
          })
        })
        return foods
      }
    },
    created () {
      this.$http.get('/api/goods').then((response) => {
        response = response.body
        if (response.errno === ERR_OK) {
          this.goods = response.data
          this.$nextTick(() => {
            this.initScroll()
            this.calculateHeight()
          })
        }
      })
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    },
    methods: {
      selectMenu (index, event) {
        if (!event._constructed) {
          return
        }
        let foodList = this.$refs.foodList
        let el = foodList[index]
        this.foodsScroll.scrollToElement(el, 300)
      },
      selectFood (food, event) {
        if (!event._constructed) {
          return
        }
        this.selectedFood = food
        this.$refs.food.show()
      },
      addFood (target) {
        this._drop(target)
      },
      _drop (target) {
        // 体验优化,异步执行下落动画
        this.$nextTick(() => {
          this.$refs.shopcart.drop(target)
        })
      },
      initScroll () {
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        })
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3
        })
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      calculateHeight () {
        let foodList = this.$refs.foodList
        let height = 0
        this.listHeight.push(height)
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i]
          height += item.clientHeight
          this.listHeight.push(height)
        }
      }
    },
    components: {
      shopcart,
      cartcontrol,
      food
    }
  }
</script>
<style>
  .goods {
    position: absolute;
    top: 174px;
    bottom: 46px;
    width: 100%;
    display: flex;
    overflow: hidden;
  }
  .goods .menu-wrapper {
    flex: 0 0 80px;
    width: 80px;
    background: #f3f5f7;
  }
  .goods .menu-wrapper .menu-item {
    display: table;
    height: 54px;
    width: 56px;
    line-height: 14px;
    padding: 0 12px;
  }
  .goods .menu-wrapper .current{
    position: relative;
    margin-top: -1px;
    background-color: #fff;
    font-weight: 700;
    z-index: 10;
  }
  .goods .menu-wrapper .current .text {
    border-bottom: 1px solid #f3f5f7;
  }
  .goods .menu-wrapper .menu-item .text {
    font-size: 12px;
    display: table-cell;
    vertical-align: middle;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
  }
  .goods .menu-wrapper .menu-item:last-child .text {
    border-bottom: 1px solid #f3f5f7;
  }
  .goods .menu-wrapper .menu-item .icon{
    display: inline-block;
    width: 12px;
    height: 12px;
    vertical-align: top;
    margin-right: 6px;
    background-size: 12px 12px;
    background-repeat: no-repeat;
    }
  .goods .menu-wrapper .decrease {
    background-image: url('./img/decrease_3@2x.png');
  }
  .goods .menu-wrapper .discount {
    background-image: url('./img/discount_3@2x.png');
  }
  .goods .menu-wrapper .guarantee {
    background-image: url('./img/guarantee_3@2x.png');
  }
  .goods .menu-wrapper .invoice {
    background-image: url('./img/invoice_3@2x.png');
  }
  .goods .menu-wrapper .special {
    background-image: url('./img/special_3@2x.png');
  }

  .goods .foods-wrapper {
    flex: 1;
  }
  .goods .foods-wrapper .titile {
    height: 26px;
    line-height: 26px;
    padding-left: 14px;
    border-left: 2px solid #d9dde1;
    font-size: 12px;
    color: rgb(147,153,159);
    background: #f3f5f7;
  }
   
  .goods .food-item {
    position: relative;
    display: flex;
    margin: 18px;
    padding-bottom: 18px;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
  }
   .goods .food-item:last-child {
    margin-bottom: 1px solid #fff;
   }
  .goods .food-item .icon {
    flex: 0 0 57px;
    margin-right: 10px;
    width: 57px;
    height: 57px;
  }
  .goods .food-item .content {
    flex: 1;
  }
  .goods .food-item .content .name {
    margin: 2px 0 8px 0;
    height: 14px;
    line-height: 14px;
    font-size: 14px;
    color: rgb(7, 17, 27);
  }
  .goods .food-item .desc,.extra {
    line-height: 10px;
    font-size: 10px;
    color: rgb(147, 153, 159);
  }
  .goods .food-item .desc {
    margin-bottom: 8px;
  }
  .goods .food-item .extra {
    line-height: 10px;
  }
  .goods .food-item .price{
    font-weight: 700;
    line-height: 24px;
  }
  .goods .food-item .price .now {
    margin-right: 8px;
    font-size: 14px;
    color: rgb(240,20,20);
  }
  .goods .food-item .price .old {
    text-decoration: line-through;
    font-size: 10px;
    color: rgb(147,153,159);
  }
  .goods .food-item .cartcontrol-wrapper {
    position: absolute;
    right: 0;
    bottom: 12px;
  }
</style>
