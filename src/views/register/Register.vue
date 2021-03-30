<template>
  <div class="wrapper">
    <img class="wrapper__img" src='http://www.dell-lee.com/imgs/vue3/user.png' />
    <div class="wrapper__input">
      <input class="wrapper__input__content" v-model="username" placeholder="请输入用户名">
    </div>
    <div class="wrapper__input">
      <input type="password" class="wrapper__input__content" v-model="password" autocomplete="new-password" placeholder="请输入密码">
    </div>
    <div class="wrapper__input">
      <input type="password" class="wrapper__input__content" v-model="ensurement" placeholder="确认密码">
    </div>
    <div class="wrapper__register-button" @click="handleRegister">注册</div>
    <div class="wrapper__register-link" @click="handleLoginClick">已有账号去登录</div>
    <Toast v-if="show" :message="toastMessage"/>
  </div>
</template>

<script>
import { reactive, toRefs } from 'vue'
import { useRouter } from 'vue-router'
import { post } from './../../util/request'
import Toast, { useToastEffect } from './../../components/Toast'

const userRegisterEffect = (showToast) => {
  const router = useRouter()
  const data = reactive({
    username:"",
    password:"",
    ensurement:""
  })
  const handleRegister = async () => {
    // 登录
    try {
      const {username,password,ensurement} = data
      if (username == '' || password == '' || ensurement == '') {
        showToast('填写表单不能为空')
        return
      }
      const result = await post('/api/user/register', {
        username:data.username,
        password:data.password
      })
      if (result?.errno == 0) {
        router.push({name:"Login"})
      } else {
        showToast('注册失败')
      }
    } catch (error) {
      showToast('请求失败')
    }
  }
  const {username ,password,ensurement} = toRefs(data)
  return {
    username ,password,ensurement,
    handleRegister
  }
}
const userLoginEffect = () =>{
  const router = useRouter()
  const handleLoginClick = () => {
    router.push({name:"Login"})
  }
  return {
    handleLoginClick
  }
}
  export default {
    name:"register",
    components:{Toast},
    setup() {
      const router = useRouter()
      const { show,toastMessage, showToast } = useToastEffect()
      const {username ,password,ensurement,handleRegister} = userRegisterEffect(showToast)
      const { handleLoginClick } = userLoginEffect()
      return {
        username,
        password,
        ensurement,
        handleRegister,
        show,
        toastMessage,
        handleLoginClick
      }
    }
  }
</script>
<style lang="scss" scoped>
@import '../../style/viriables.scss';
.wrapper {
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  transform: translateY(-50%);
  &__img {
    display: block;
    margin: 0 auto .4rem auto;
    width: .66rem;
    height: .66rem;
  }
  &__input {
    height: .48rem;
    margin: 0 .4rem .16rem .4rem;
    padding: 0 .16rem;
    background: #F9F9F9;
    border: .01rem solid rgba(0,0,0,0.10);
    border-radius: .06rem;
    border-radius: .06rem;
    &__content {
      margin-top: .12rem;
      line-height: .22rem;
      border: none;
      outline: none;
      width: 100%;
      background: none;
      font-size: .16rem;
      color: $content-notice-fontcolor;
      &::placeholder {
        color: $content-notice-fontcolor;
      }
    }
  }
  &__register-button {
    margin: .32rem .4rem .16rem .4rem;
    line-height: .48rem;
    background: #0091FF;
    box-shadow: 0 .04rem .08rem 0 rgba(0,145,255,0.32);
    border-radius: .04rem;
    border-radius: .04rem;
    color: $bgColor;
    font-size: .16rem;
    text-align: center;
  }
  &__register-link {
    text-align: center;
    font-size: .14rem;
    color: $content-notice-fontcolor;
  }
}
</style>