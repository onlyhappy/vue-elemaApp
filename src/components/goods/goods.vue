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
            <li v-for="food in item.foods" class="food-item">
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
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
  import BScroll from 'better-scroll'
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
        scrollY: 0
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
      initScroll () {
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {})
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
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
    }
  }
</script>
<style>
  .goods {
    display: flex;
    position: absolute;
    top: 176px;
    bottom: 64px;
    width: 100%;
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
  .goods .menu-wrapper .menu-item .current{
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

  <!-- 右边食品 -->
  .goods .foods-wrapper {
    flex:1;
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
</style>
