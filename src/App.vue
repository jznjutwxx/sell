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
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>    
  </div>
</template>

<script type="text/ecmascript-6">
import {urlParse} from './common/js/util';
import header from 'components/header/header';

const ERR_OK = 0;// 状态码

export default {
  name: 'app',
  data() {
    return {
      seller: {
        id: (() => {
          let queryParam = urlParse();
          return queryParam.id;
        })()
      }
    };
  },
  mounted() {
    this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
      response = response.body;
      if (response.errno === ERR_OK) {
        // this.seller = response.data;
        // 扩展this.seller, 给对象扩展属性
        this.seller = Object.assign({}, this.seller, response.data);
      }
    });
  },
  components: {
    'v-header': header
  }
};
</script>

<style lang="scss" scope>
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
