// 数量增减按钮组件
<template>
<div class="cartcontrol">
  // 平移和滚动特效
  <transition name="move">
    <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart($event)">
      <span class="inner icon-remove_circle_outline"></span>
    </div>
  </transition>
  <div class="cart-count" v-show="food.count">{{food.count}}</div>
  <div class="cart-add icon-add_circle" @click.stop.prevent="addCart($event)"></div>
</div>
</template>
<script type="text/ecmascript-6">
  import Vue from 'vue';

  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart: function(event) {
        if (!event._constructed) {
          return false;
        }
        if (!this.food.count) {
          // 给一个对象新增一个相应的属性，需要使用vue.set
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
        // 派发一个添加购物车事件
        this.$emit('cartAdd', event.target);
      },
      decreaseCart: function(event) {
        if (!event._constructed) {
          return false;
        }
        if (this.food.count) {
          this.food.count--;
        }
      }
    }
  };
</script>
<style lang="scss" scope>
.cartcontrol {
  font-size: 0;

  .cart-decrease {
    display: inline-block;
    padding: 6px;// 增大点击范围
    opacity: 1;
    transform: translate3d(0, 0, 0);// 3d开启硬件加速，动画流畅
    .inner {
      display: inline-block;
      line-height: 24px;
      font-size: 24px;
      color: rgb(0, 160, 220);
      transform: rotate(0);
      transition: all 0.4s linear;
    }

    &.move-enter-active, &.move-leave-active {
      // 动画持续0.4s
      transition: all 0.4s linear;
    }
    &.move-enter, &.move-leave-active{
      // 进入时的初始状态
      opacity: 0;
      transform: translate3d(24px, 0, 0); 
      .inner {
        transform: rotate(180deg);
      }       
    }
  }
  .cart-count {
    display: inline-block;
    vertical-align: top;
    width: 12px;
    line-height: 24px;
    padding-top: 6px;
    font-size: 10px;
    color: rgb(143, 153, 159);
    text-align: center;
  }
  .cart-add {
    display: inline-block;
    padding: 6px;// 增大点击范围
    line-height: 24px;
    font-size: 24px;
    color: rgb(0, 160, 220);
  }
}
</style>
