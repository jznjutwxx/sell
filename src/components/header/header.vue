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
        <div class="supports" v-if="seller.supports">
          <span class="icon"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
    </div>
    <div class="bulletin-wrapper"></div>
  </div>
</template>

<script type="text/ecmascript-6">
export default{
  props: {
    seller: {
      type: Object
    }
  }
};
</script>

<style lang="scss" scope>
    .header {
      color: #fff;
      background: #000;

      .content-wrapper {
        padding: 24px 18px 12px 24px;
        font-size: 0;
        
        .avatar {
          display: inline-block;
          vertical-align: bottom;
        }
        .content {
          display: inline-block;
          margin-left: 16px;
          font-size: 14px;

          .title {
            margin: 2px 0 8px 0;

            .brand {
              display: inline-block;
              width: 30px;
              height: 18px;
            }
          }
        }
      }
    }
</style>
