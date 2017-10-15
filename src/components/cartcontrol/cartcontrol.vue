<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>
</template>
<script>
  import Vue from 'vue'
  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart (event) {
        if (!event._constructed) {
          return
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
        }
        this.$emit('add', event.target)
      },
      decreaseCart (event) {
        if (!event._constructed) {
          return
        }
        if (this.food.count) {
          this.food.count--
        }
      }
    }
  }
</script>
<style>
  .cartcontrol {
    font-size: 0;
  }
  .cartcontrol .cart-decrease {
    padding: 6px;
    display: inline-block;
    line-height: 24px;
    font-size: 24px;
    color: rgb(0, 160, 220);
  }
  .cartcontrol .cart-decrease .init {
    display: inline-block;
    line-height: 24px;
    font-size: 24px;
    color: rgb(0, 160, 220);
  }
  .cartcontrol .cart-count {
    display: inline-block;
    vertical-align: top;
    width: 12px;
    padding-top: 6px;
    line-height: 24px;
    text-align: center;
    font-size: 10px;
    color: rgb(147, 153, 159);
  }
  .cartcontrol .cart-add {
    display: inline-block;
    padding: 6px;
    line-height: 24px;
    font-size: 24px;
    color: rgb(0, 160, 220);
  }
  .cartcontrol .move-enter-active {
    animation: move-in .3s;
  }
  .cartcontrol .move-leave-active {
    animation: move-out .3s;
  }
  @keyframes move-in {
    0% {
      transform: translateX(15px) rotate(0deg);
      opacity: 0;
    }
    25% {
      transform: translateX(10px) rotate(45deg);
      opacity: 0.25
    }
    50% {
      transform: translateX(5px) rotate(90deg);
      opacity: 0.5;
    }
    100% {
      transform: translateX(0px) rotate(180deg);
      opacity: 0.75;
    }
  }
  @keyframes move-out {
    0% {
      transform: translateX(0px) rotate(180deg);
      opacity: 0.75;
    }
    25% {
      transform: translateX(5px) rotate(90deg);
      opacity: 0.5;
    }
    50% {
      transform: translateX(10px) rotate(45deg);
      opacity: 0.25;
    }
    100% {
      transform: translateX(15px) rotate(0deg);
      opacity: 0;
    }
  }
</style>