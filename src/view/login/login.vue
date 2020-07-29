<style lang="less">
  @import './login.less';
</style>

<template>
  <div class="login">
    <div class="login-con">
      <Card icon="log-in" title="欢迎登录" :bordered="false">
        <div class="form-con">
          <login-form @on-success-valid="handleSubmit"></login-form>
          <p class="login-tip">输入任意用户名和密码即可</p>
        </div>
      </Card>
    </div>
  </div>
</template>

<script>
import LoginForm from '_c/login-form'
import { mapActions } from 'vuex'
export default {
  components: {
    LoginForm
  },
  methods: {
    ...mapActions([
      'handleLogin',
      'getUserInfo'
    ]),
    
    handleSubmit ({ uname, upass }) {
      this.loading = true
      this.handleLogin({ uname, upass }).then(res => {
        this.getUserInfo().then(res => {
          this.$router.push({
            name: 'home'
          })
          this.$sen.requestAuth({
            url: '/system/auth/getUserInfo'
          }).then(res => {
            if (res.info.modifyPwd === true) {
              this.$Message.error('初始秘密未修改，请先修改！')
            }
          })
        }).catch(res => {
        this.loading = false
      })
      }).catch(res => {
        this.loading = false
      })
    }
  }
}
</script>

<style>

</style>
