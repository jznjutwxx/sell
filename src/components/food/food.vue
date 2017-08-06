<template>
  <transition name="move">
    <div v-show="showFlag" class="food" ref="food">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
          <div class="back" @click="hide">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <div class="title">{{food.name}}</div>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div> 
          <div class="price">
            <span class="now">￥ {{food.price}}</span><span v-show="food.oldPrice" class="old">￥ {{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <cartcontrol @cartAdd="_drop" :food="food"></cartcontrol>
          </div>
          <transition name="fade">
            <div @click.stop.prevent="addFirst" class="buy" v-show="!food.count || food.count === 0">加入购物车</div>
          </transition>
       </div>
       <split></split>
        <div class="info" v-show="food.info">
          <div class="title">商品详情</div>
          <div class="text">{{food.info}}</div>
        </div>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <ratingselect :select-typeprops="selectTypeprops" :only-content="onlyContentprops" :desc="desc" :ratings="food.ratings"></ratingselect>
        </div>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  import BScroll from 'better-scroll';
  import cartcontrol from 'components/cartcontrol/cartcontrol';
  import split from 'components/split/split';
  import ratingselect from 'components/ratingselect/ratingselect';

  const ALL = 2;

  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        showFlag: false,
        selectTypeprops: ALL,
        onlyContentprops: true,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      };
    },
    methods: {
      show() {
        this.showFlag = true;
        // 初始化
        this.selectTypeprops = ALL;
        this.onlyContentprops = true;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      },
      hide() {
        this.showFlag = false;
      },
      addFirst(event) {
        if (!event._constructed) {
          return;
        }
        this.$emit('cartAdd', event.target);
        Vue.set(this.food, 'count', 1);
      },
      _drop(target) {
        // 过程有点复杂，是这样的，首先cartcontrol控件分发一个cartAdd事件，然后food接收到cartAdd事件执行_drop事件，
        // _drop再次奋发一个emit给goods,goods组件接收到cartAdd事件，在触发小球下落逻辑
        this.$emit('cartAdd', target);
      }
    },
    components: {
      cartcontrol,
      split,
      ratingselect
    }
  };
</script>

<style lang="scss" scope>
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

    .image-header {
      position: relative;
      width: 100%;
      // 黑魔法,图片加载是异步过程，如果高度不是一定会出现加载完成后页面布局抖动
      height: 0;
      padding-top: 100%;// 相对于宽度设置，所以就是一个宽高相等的容器

      img {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;        
        height: 100%;
      }
      .back {
        position: absolute;
        top: 10px;
        left: 0;

        .icon-arrow_lift {
          display: block;
          padding: 10px;// 扩大点击范围
          font-size: 20px;
          color: #fff;
        }
      }
    }
    .content {
      position: relative;
      padding: 18px;
      
      .title {
        line-height: 14px;
        margin-bottom: 8px;
        font-size: 14px;
        font-weight: 700;
        color: rgb(7, 17, 27);
      }
      .detail {
        margin-bottom: 18px;
        line-height: 10px;
        font-size: 0;

        .sell-count, .rating {
          font-size: 10px;
          color: rgb(147, 153, 159);          
        }
        .sell-count {
          margin-right: 10px;
        }
      }
      .price {
          font-weight: 700;
          line-height: 24px;

          .now {
            margin-right: 8px;
            font-size: 14px;
            color: rgb(240, 20, 20);
          }
          .old {
            text-decoration: line-through;
            font-size: 10px;
            color: rgb(147, 152, 159);
          }
        }
      .cartcontrol-wrapper {
        position: absolute;
        right: 12px;
        bottom: 12px;
      }
      .buy {
        position: absolute;
        right: 18px;
        bottom: 18px;
        z-index: 10;
        line-height: 24px;
        padding: 0 12px;
        font-size: 10px;
        color: #fff;
        box-sizing: border-box;
        border-radius: 12px;
        background: rgb(0, 160, 220);
        opacity: 1;

        &.fade-enter-active, &.fade-leave-active {
          transition: all 0.2s 
        }
        &.fade-enter, &.fade-leave-active {
          opacity: 0;
          z-index: -1;
        }
      }
    }     
    .info {
      padding: 18px;

      .title {
        line-height: 14px;
        margin-bottom: 6px;
        font-size: 14px;
        color: rgb(7, 17, 27);
      }
      .text {
        line-height: 24px;
        padding: 0 8px;
        font-size: 12px;
        color: rgb(77, 85, 93);
      }
    }
    .rating {
      padding-top: 18px;

      .title {
        line-height: 14px;
        margin-left: 18px;
        font-size: 14px;
        color: rgb(7, 17, 27);
      }
    }
  }
</style>
