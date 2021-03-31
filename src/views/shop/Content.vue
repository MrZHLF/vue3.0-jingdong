<template>
  <div class="content">
    <div class="category">
      <div 
        :class="{'category__item':true ,'category__item--active' : curentTab == item.tab}" 
        v-for="item in categories" :key="item.name"
        @click="()=>handleTabClick(item.tab)">{{item.name}}</div>
    </div>
    <div class="product">
      <div class="product__item" v-for="item in list" :key="item._id">
        <img class="product__item__img" :src="item.imgUrl" alt="">
        <div class="product__item__detail">
          <h4 class="product__item__title">{{item.name}}</h4>
          <p class="product__item__sales">月售{{item.sales}}件</p>
          <p class="product__item__price">
            <span class="product__item__yen">&yen;</span>{{item.price}}
            <span class="product__item__origin">&yen;{{item.oldPrice}}</span>
          </p>
        </div>
        <div class="product__number">
          <span class="product__number__minus iconfont" @click="()=>{ changeCartItemInfo(shopId,item._id,item, -1) }">&#xe691;</span>
            {{item.count || 0}}
          <span class="product__number__plus iconfont" @click="()=>{ changeCartItemInfo(shopId,item._id,item, 1) }">&#xe668;</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive,ref, toRefs,watchEffect } from 'vue'
import { get } from '../../util/request'
import { useRoute } from 'vue-router'
import { useCommonCartEffect } from './commonCartEffect'
const categories = [
  { name: '全部商品', tab: 'all' },
  { name:'秒杀', tab: 'seckill' },
  { name: '新鲜水果', tab: 'fruit'}
]

//左侧导航列表
const userTabEffect = () => {
  const curentTab = ref(categories[0].tab)
  const handleTabClick = (tab) => {
    //点击tab切换
    curentTab.value = tab
  }
  return {
    curentTab,
    handleTabClick
  }
}

//右侧列表内容
const userCurrentListEffect = (curentTab,shopId) => {
  const content = reactive({list:[]})
  const getContentData = async () => {
    const result = await get(`/api/shop/${shopId}/products`,{tab:curentTab.value})
    if (result?.errno == 0 && result?.data) {
      content.list = result.data
    }
  }
  watchEffect(() => {
    console.log(4545);
    getContentData()
  })
  const { list } = toRefs(content)
  return {
    list
  }
}

export default {
  name:"Content",
  setup () {
    const route = useRoute()
    const shopId = route.params.id
    const {curentTab,handleTabClick } = userTabEffect()
    const {list} = userCurrentListEffect(curentTab,shopId)
    const {changeCartItemInfo } = useCommonCartEffect()
    return {
      shopId,
      list,
      categories,
      curentTab,
      handleTabClick,
      changeCartItemInfo
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../../style/viriables.scss';
@import '../../style/mixins.scss';
.content {
  display: flex;
  position: absolute;
  left: 0;
  right: 0;
  top: 1.5rem;
  bottom: .5rem;
}
.category {
  overflow-y: scroll;
  height: 100%;
  width: .76rem;
  background: $search-bgColor;
  &__item {
    line-height: .4rem;
    text-align: center;
    font-size: .14rem;
    color: #333;
    &--active {
      background: $bgColor;
    }
  }
}
.product {
  flex: 1;
  overflow-y: scroll;
  &__item {
    position: relative;
    display: flex;
    padding: .12rem 0;
    margin: 0 .16rem;
    border-bottom: .01rem solid $content-bgColor;
    &__detail {
      overflow: hidden;
    }
    &__img {
      width: .68rem;
      height: .68rem;
      margin-right: .16rem;
    }
    &__title {
      margin: 0;
      line-height: .2rem;
      font-size: .14rem;
      color: $content-fontcolor;
      @include ellipsis;
    }
    &__sales {
      margin: .06rem 0;
      font-size: .12rem;
      color: $content-fontcolor;
    }
    &__price {
      margin: 0;
      line-height: .2rem;
      font-size: .14rem;
      color: $hightlight-fontColor;
    }
    &__yen {
      font-size: .12rem;
    }
    &__origin {
      margin-left: .06rem;
      line-height: .2rem;
      font-size: .12rem;
      color: $light-fontColor;
      text-decoration: line-through;
    }
    .product__number {
      position: absolute;
      right: 0;
      bottom: .12rem;
      line-height: .18rem;
      &__minus {
        position:relative;
        top: .02rem;
        color: $medium-fontColor;
        margin-right: .05rem;
      }
      &__plus {
        position:relative;
        top: .02rem;
        color: $btn-bgColor;
        margin-left: .05rem;
      }
    }
  }
}
</style>