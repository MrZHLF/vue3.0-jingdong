<template>
  <div class="toast">
    {{message}}
  </div>
</template>

<script>
import { reactive,toRefs } from 'vue'
export default {
  name:"toast",
  props:['message'],
}
export const useToastEffect = () => {
  const toastData = reactive({
    show:false,
    toastMessage:""
  })
  const showToast = (message) => {
    toastData.show = true
    toastData.toastMessage = message
    setTimeout(() =>{
      toastData.show = false
      toastData.toastMessage = ""
    },2000)
  }
  const {show,toastMessage } = toRefs(toastData)
  return {
    toastMessage,
    show,
    showToast
  }
}
</script>

<style lang="scss" scoped>
.toast {
  position: fixed;
  left: 50%;
  top: 50%;
  padding: .1rem;
  border-radius: .05rem;
  transform: translate(-50%, -50%);
  background: rgba(0, 0, 0, .35);
  color: #fff;
}
</style>