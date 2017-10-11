<template>
  <div :class="starType" class="star">
    <span v-for="item in items" :class="item" class="star-item"></span>
  </div>
</template>

<script>
  const LENGTH = 5
  const CLSON = 'on'
  const CLSHAlF = 'half'
  const CLSOFF = 'off'
  export default {
    props: {
      size: {
        type: Number
      },
      score: {
        type: Number
      }
    },
    computed: {
      starType () {
        return 'star-' + this.size
      },
      items () {
        let result = []
        let score = Math.floor(this.score * 2) / 2
        // 是否有半星
        let hasDecimal = score % 1 !== 0
        let integer = Math.floor(score)
        for (let i = 0; i < integer; i++) {
          result.push(CLSON)
        }
        if (hasDecimal) {
          result.push(CLSHAlF)
        }
        while (result.length < LENGTH) {
          result.push(CLSOFF)
        }
        return result
      }
    }
  }
</script>

<style>
  .star {
    font-size: 0;
  }
  .star .star-item {
    display: inline-block;
    background-repeat: no-repeat;  
  }
  .star-48 .star-item {
    width: 20px;
    height: 20px;
    margin-right: 22px;
    background-size: 20px 20px;
  }
  .star-48 .last-child {
     margin-right: 0;
  }
  .star-48 .on {
    background-image: url('./img/star48_on@2x.png');
  }
  .star-48 .half {
    background-image: url('./img/star48_half@2x.png');
  }
  .star-48 .off {
    background-image: url('./img/star48_off@2x.png');
  }
  .star-36 .star-item {
    width: 15px;
    height: 15px;
    margin-right: 3px;
    background-size: 10px 10px;
  }
  .star-36 .last-child {
     margin-right: 0;
  }
  .star-36 .on {
    background-image: url('./img/star36_on@2x.png');
  }
  .star-36 .half {
    background-image: url('./img/star36_half@2x.png');
  }
  .star-36 .off {
    background-image: url('./img/star36_off@2x.png');
  }
  .star-24 .star-item {
     width: 10px;
     height: 10px;
     margin-right: 3px;
     background-size: 10px 10px
  }
  .star-24 .last-child {
     margin-right: 0;
  }
  .star-24 .on {
    background-image: url('./img/star24_on@2x.png');
  }
  .star-24 .half {
    background-image: url('./img/star24_half@2x.png');
  }
  .star-24 .off {
    background-image: url('./img/star24_off@2x.png');
  }
</style>
