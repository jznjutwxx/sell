<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img class="avatar" width="64" height="64" :src="seller.avatar"></img>
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}} / {{seller.deliveryTime}}分钟送达
        </div>
         <icontext :listdata="seller.supports" :amount="1"></icontext> 
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail()">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail()">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <div class="supports-wrapper">
              <icontext :listdata="seller.supports" :amount="0"></icontext>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
import star from 'components/star/star';
import icontext from 'components/iconText/icontext';

export default{
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      detailShow: false
    };
  },
  mounted() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
  methods: {
    showDetail: function() {
      this.detailShow = true;
    },
    hideDetail: function() {
      this.detailShow = false;
    }
  },
  components: {
    star,
    icontext
  }
};
</script>

<style lang="scss" scope>
@import '../../common/scss/mixin.scss';

.header {
  position: relative;
  color: #fff;
  background: rgba(7, 17, 27, 0.5);
  overflow: hidden;// 虚化背景溢出隐藏

  .content-wrapper {
    position: relative;
    padding: 24px 18px 12px 24px;
    font-size: 0;
    
    .avatar {
      display: inline-block;
      vertical-align: top;
      border-radius: 5px;
    }
    .content {
      display: inline-block;
      margin-left: 16px;

      .title {
        margin: 2px 0 8px 0;

        .brand {
          display: inline-block;
          vertical-align: top;              
          width: 30px;
          height: 18px;
          margin-right: 6px;
          @include bg-image('./brand');
        }

        .name {
          font-weight: bold;              
          font-size: 16px;
          line-height: 18px;
          color: rgb(255,255,255);
        }
      }

      .description {
        margin-bottom: 10px;
        font-size: 12px;
        color: rgb(255, 255, 255); 
      }
    }

    .support-count {
      position:absolute;
      right: 17px;
      bottom: 11px;
      padding: 0 8px;
      height: 24px;
      font-size: 10px;
      line-height: 24px;
      border-radius: 14px;
      background: rgba(0, 0, 0, .2);
      text-align: center;

      .count {
        vertical-align: top;
      }

      .icon-keyboard_arrow_right {
        margin-left: 2px;
        line-height: 24px;
      }
    }
  }

  .bulletin-wrapper {
    position: relative;
    height: 28px;
    line-height: 28px;
    padding: 0 22px 0 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    background: rgba(7, 17, 27, .2);

    .bulletin-title{
      display: inline-block;
      vertical-align: top;
      width: 22px;
      height: 12px;
      margin-top: 8px;
      @include bg-image('./bulletin');
    }

    .bulletin-text {
      vertical-align: top;
      font-size: 10px;
    }

    .icon-keyboard_arrow_right {
      position: absolute;
      right: 12px;
      top: 8px;
      font-size: 10px;
    }
  }

  .background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px);// 不兼容ie
  }

  .detail {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 100;        
    width: 100%;
    height: 100%;
    overflow: auto;
    opacity: 1;
    background: rgba(7, 17, 27, 0.8);
    &.fade-enter-active, &.fade-leave-active{
      transition: all 0.5s;
    }
    &.fade-enter, &.fade-leave-active {
      opacity: 0;
      background: rgba(7, 17, 27, 0);
    }
    backdrop-filter: blur(10px);// 只有ios才可以实现

    .detail-wrapper {
      width: 100%;
      min-height: 100%;
      padding-top: 64px;
      box-sizing: border-box;

      .detail-main {
        padding-bottom: 64px;

        .name {
          font-size: 16px;
          line-height: 16px;
          text-align: center;
        }

        .star-wrapper {
          margin-top: 18px;
          padding: 2px 0;
          text-align: center;
        }

        .title {
          display: flex;
          width: 80%;
          margin: 28px auto 24px auto;

          .line {
            flex: 1;
            position: relative; 
            top: -6px;
            border-bottom: 1px solid rgba(255, 255,255, 0.2);
          }
          .text {
            padding: 0 12px;
            font-size: 14px;
            font-weight: 700;
          }
        }

        .supports-wrapper{
          width: 80%;
          margin: 0 auto;
        }

        .bulletin {
          width: 80%;
          margin: 0 auto;

          .content {
            padding: 0 12px;
            line-height: 24px;
            font-size: 12px;
          }
        }
      }
    }
    
    .detail-close {
      position: relative;
      top: 0;
      width: 32px;
      height: 32px;
      margin: -64px auto 0 auto;
      font-size: 32px;
      clear: both;
    }
  }
}
</style>
