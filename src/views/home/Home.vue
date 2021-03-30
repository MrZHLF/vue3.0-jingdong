<template>
  <div class="wrapper">
    <static-part/>
    <Nearby/>
  </div>
  <Docker/>
  <!-- <router-view /> -->
</template>
<script>
import StaticPart from './StaticPart.vue'
import Nearby from './Nearby.vue'
import Docker from './Docker.vue'
  export default {
    name: 'Home',
    components:{
      StaticPart,
      Nearby,
      Docker
    }
  }
</script>

<style lang="scss" scoped>
.wrapper {
  overflow-y: auto;
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: .5rem;
  padding: 0 .18rem .3rem .18rem;
}
</style>
