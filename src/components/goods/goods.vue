<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="(item, index) in goods" :key="index" class="menu-item">
          <span class="text">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper"></div>
  </div>
</template>

<script type="text/ecmascript-6">

const ERR_OK = 0;// 状态码

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      goods: {}
    };
  },
  mounted() {
    this.$http.get('./api/goods').then((response) => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.goods = response.data;
      }
    });

    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
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
    flex: 1
  }
}

</style>
