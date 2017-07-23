<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <ul class="tab">
      <li class="tab-item border-1px">
        <router-link to="/goods">商品</router-link>
      </li>
      <li class="tab-item border-1px">
         <router-link to="/ratings">评价</router-link>
      </li>
      <li class="tab-item border-1px">
        <router-link to="/seller">商家</router-link>
      </li>      
    </ul>
   <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import header from 'components/header/header';

const ERR_OK = 0;// 状态码

export default {
  name: 'app',
  data() {
    return {
      seller: {}
    };
  },
  mounted() {
    this.$http.get('/api/seller').then((response) => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.seller = response.data;
      }
    });
  },
  components: {
    'v-header': header
  }
};
</script>

<style lang="scss" scope>
// 1.位置属性(position, top, right, z-index, display, float,vertical-align等)
// 2.大小(width, height, padding, margin)
// 3.文字系列(font, line-height, letter-spacing, color- text-align等)
// 4.背景(background, border等)
// 5.其他(animation, transition,overflow等)

// 使用混合依旧需要单独引入
@import './common/scss/mixin.scss';

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  /*平滑字体*/
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.tab {
  display: flex;
  width: 100%;
  height: 40px;
  line-height: 40px;
  list-style: none;
  @include border-1px(rgba(7, 17, 27, 0.1));

  .tab-item{
    flex: 1;
    text-align: center;

    & > a {
      display: block;// 撑满一个块级，增强体验
      font-size: 14px;
      color: rgb(77,85,93);

      &.active {
        color: rgb(240,20,20);
      }
    }
  } 
}


</style>
