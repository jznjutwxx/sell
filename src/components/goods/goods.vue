<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item, index) in goods" :key="index" class="menu-item">
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
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll';

const ERR_OK = 0;// 状态码

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      goods: {},
      listHeight: []
    };
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
    _initScroll: function() {
      this.meunScroll = new BScroll(this.$refs.menuWrapper, {});
      this.foodScroll = new BScroll(this.$refs.foodWrapper, {});
    },
    _caculateHeight: function() {
      this.foodlist = this.$refs.foodWrapper.getElementsByClassName('');
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
          hieght: 14px;
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
      }
    }
  }
}

</style>
