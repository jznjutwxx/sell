<template>
  <ul v-if="listdata" class="lists">
    <li v-for="(item, index) in newlistdata" class="list-item" :key="index">
      <span class="icon" :class="classMap[newlistdata[index].type]"></span>
      <span class="text">{{newlistdata[index].name}}</span>
    </li>
  </ul> 
</template>
<script>
  export default {
    props: {
      listdata: {
        type: Array
      },
      amount: {
        type: Number
      }
    },
    mounted() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    computed: {
      newlistdata() {
        if (!this.amount) {
          return this.listdata;
        } else {
          return this.listdata.slice(0, this.amount);
        }
      }
    },
    data() {
      return {
      };
    }
  };
</script>
<style lang="scss" scope>
@import '../../common/scss/mixin.scss';

.lists {
  .list-item {
    margin-bottom: 12px;            
    font-size: 0;
    &:last-child {
      margin-bottom: 0;
    }

    .icon {
      display: inline-block;
      vertical-align: top;              
      width: 16px;
      height: 16px;
      margin-right: 6px;

      &.decrease {
        @include bg-image('./decrease_2');
      }
      &.discount {
        @include bg-image('./discount_2');
      }
      &.guarantee {
        @include bg-image('./guarantee_2');
      }
      &.invoice {
        @include bg-image('./invoice_2');
      }
      &.special {
        @include bg-image('./special_2');
      }
    }
    .text {
      font-size: 12px;
      line-height: 16px;
    }
  }
}
</style>