// 商品列表组件
<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
       <ul>
        <li v-for="(item, index) in goods" :key="index" class="menu-item" :class="{'current': currentIndex === index}" @click="selectMenu(index, $event)">
          <span class="text">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul> 
    </div>
    <div class="foods-wrapper" ref="foodWrapper">
      <ul>
        <li v-for="(item, index) in goods" :key="index" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="(food, index) in item.foods" :key="index" class="food-item border-1px">
              <div class="icon">
                <img :src="food.icon" width="100%"/>
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售 {{food.sellCount}}</span><span>好评率 {{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥ {{food.price}}</span><span v-show="food.oldPrice" class="old">￥ {{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol @cartAdd="_drop" :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart ref="shopcart" :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
  </div>
</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll';
import shopcart from 'components/shopcart/shopcart';
import cartcontrol from 'components/cartcontrol/cartcontrol';

const ERR_OK = 0;// 状态码

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      goods: [],
      listHeight: [0], // 高度临界值
      scrollY: 0 // 滚动值
    };
  },
  computed: {
    currentIndex() {
      for (let i = 0; i < this.listHeight.length; i++) {
        let heightstart = this.listHeight[i];
        let heightend = this.listHeight[i + 1];
        if (!heightend || this.scrollY >= heightstart && this.scrollY < heightend) {
          return i;
        }
      }
      return 0;
    },
    selectFoods() {
      let foods = [];
      this.goods.forEach((good) => {
        // 每一个榜单
        good.foods.forEach((food) => {
          // 每一个食品
          if (food.count) {
            foods.push(food);
          }
        });
      });
      return foods;
    }
  },
  mounted() {
    this.$http.get('./api/goods').then((response) => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.goods = response.data;
        this.$nextTick(() => {
          // this.goods改变了数据，但是dom还未更新，下面的操作需要使用dom，nextTick表示当dom更新之后再执行
          this._initScroll();
          this._caculateHeight();
        });
      }
    });

    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
  methods: {
    // 点击menu-item定位
    selectMenu: function(index, event) {
      if (!event._constructed) {
        return false;
      }
      // 原生点击事件不具有constructed属性
      let foodlist = this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
      this.foodScroll.scrollToElement(foodlist[index], 200);
    },
    // 初始化滚动
    _initScroll: function() {
      this.meunScroll = new BScroll(this.$refs.menuWrapper, {
        click: true // 派发点击事件
      });
      this.foodScroll = new BScroll(this.$refs.foodWrapper, {
        click: true, // 派发点击事件
        'probeType': 3 // 配置滚动
      });
      this.foodScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));// 返回滚动值
      });
    },
    // 计算food-list高度
    _caculateHeight: function() {
      let foodlist = this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
      let height = 0;

      for (let i = 0; i < foodlist.length; i++) {
        height += foodlist[i].clientHeight;
        this.listHeight.push(height);
      }
    },
    _drop: function(target) {
      this.$refs.shopcart.drop(target);
    }
  },
  components: {
    shopcart,
    cartcontrol
  },
  events: {
    'cart.add' (target) {
      this._drop(target);
    }
  }
};
</script>

<style lang="scss" scope>
@import '../../common/scss/mixin.scss';

.goods {
  display: flex;
  position: absolute;
  top: 168px;
  bottom: 46px;
  width: 100%;
  overflow: hidden;

  .menu-wrapper {
    flex: 0 0 80px;
    width: 80px;// 兼容安卓 
    background: #f3f5f7;

    .menu-item {
      display: table;
      width: 56px;
      height: 54px;
      padding: 0 12px;   
      line-height: 14px;

      &.current {
        position: relative;
        z-index: 10;
        margin-top: -1px;
        font-weight: 700;
        background: #fff;

        .text {
          @include border-none();
        }
      }

      .icon {
        display: inline-block;
        vertical-align: top;              
        width: 12px;
        height: 12px;
        margin-right: 2px;

        &.decrease {
          @include bg-image('./decrease_3');
        }
        &.discount {
          @include bg-image('./discount_3');
        }
        &.guarantee {
          @include bg-image('./guarantee_3');
        }
        &.invoice {
          @include bg-image('./invoice_3');
        }
        &.special {
          @include bg-image('./special_3');
        }
      }
      .text {
        display: table-cell;
        vertical-align: middle;
        width: 56px;     
        font-size: 12px;
        @include border-1px(rgba(7, 17, 27, 0.1));
      }
    }
  }
  .foods-wrapper {
    flex: 1;

    .title {
      padding-left: 14px;
      height: 26px;
      line-height: 26px;
      border-left: 2px solid #d9dde1;
      font-size: 12px;
      color: rgb(147, 153, 159);
      background: #f3f5f7;
    }
    .food-item {
      display: flex;
      margin: 18px;
      padding-bottom: 18px;
      @include border-1px(rgba(7, 17, 27, 0.1));

      &:last-child {
        @include border-none();
        margin-bottom: 0;
      }
      .icon {
        flex: 0 0 57px;
        margin-right: 10px;
      }
      .content {
        flex: 1;

        .name {
          margin: 2px 0 8px 0;
          height: 14px;
          line-height: 14px;
          font-size: 14px;
          color: rgb(7, 17, 27);
        }
        .desc, .extra {
          line-height: 10px;
          font-size: 10px;
          color: rgb(147, 153, 159);
        }
        .desc {
          margin-bottom: 8px;  
          line-height: 12px;        
        }
        .extra {
          .count {
            margin-right: 12px;
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
          right: 0;
          bottom: 12px;
        }
      }
    }
  }
}

</style>
