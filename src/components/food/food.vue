<template>
  <transition name="move">
    <div v-show="showFlag" class="food">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
        </div>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        showFlag: false
      };
    },
    methods: {
      show() {
        this.showFlag = true;
      }
    }
  };
</script>

<style lang="scss">
  .food {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 48px;
    z-index: 30;
    width: 100%; 
    background: #fff;
    transform: translate(0, 0, 0);
    &.move-enter, &.move-leave-to {
      transform: translate3d(100%, 0, 0);
    }
    &.move-enter-active, &.move-leave-active {
      transition: all 0.2s linear
    }
  }
</style>
