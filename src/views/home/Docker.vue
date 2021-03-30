<template>
  <div class="docker">
    <div v-for="(item,index) in dockerList" 
      :key="index" 
      class="docker__item docker__item--active" 
      :class="{'docker__item':true,'docker__item--active' : index == currentIndex}">
      <div class="iconfont" v-html="item.icon"></div>
      <div class="docker__title">{{item.text}}</div>
    </div>
  </div>
</template>
<script>
import { ref } from 'vue'
export default {
  setup () {
    const  currentIndex = ref(0)
    const dockerList = [
      {icon: '&#xe6f3;', text: '首页', to: {name: 'Home'}},
      {icon: '&#xe7e5;', text: '购物车', to: {name: 'CartList'}},
      {icon: '&#xe61e;', text: '订单', to: {name: 'OrderList'}},
      {icon: '&#xe660;', text: '我的', to: {name: 'PersonalInfo'}},
    ]
    return {
      dockerList,
      currentIndex
    }
  }
}
</script>
<style lang="scss" scoped>
@import '../../style/viriables.scss';
.docker {
  display: flex;
  box-sizing: border-box;
  position: absolute;
  padding: 0 0.18rem;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 0.49rem;
  color: $content-fontcolor;
  border-top: .1px solid $content-bgColor;
  &__item {
    flex: 1;
    text-align: center;
    .iconfont {
      margin: 0.07rem 0 0.02rem 0;
      font-size: .2rem;
    }
    &--active{
      color: $btn-bgColor;
    }
  }
  &__title {
    font-size: .2px;
    transform: scale(0.5, 0.5);
    transform-origin:center top;
  }
}
</style>