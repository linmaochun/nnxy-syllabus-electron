<template>
  <div id="Login">
    <div class="vh-100">

      <div class="d-flex align-items-center frame-height drag bg-light">
        <NavBarCtrlBtn></NavBarCtrlBtn>
      </div>

      <div class="container w-25 position-absolute top-50 start-50 translate-middle p-1">
        <div class="d-grid mx-auto mt-3">
          <span class="navbar-brand text-black-50 text-center w-100">南院课表</span>
        </div>
        <input @keydown.enter="login" @focusin="tip = ''" v-model="xh" type="text" class="form-control mt-3 bg-transparent text-black-50"
               placeholder="学号">
        <input @keydown.enter="login" @focusin="tip = ''" v-model="pwd" type="password"
               class="form-control mt-3 bg-transparent text-black-50" placeholder="密码">
        <div class="d-grid mx-auto mt-3">
          <button @click="login" class="btn btn-light text-black-50" type="button">登录</button>
        </div>
        <p class="text-center text-black-50 pt-2"><strong>{{tip}}</strong></p>
      </div>

    </div>

  </div>

</template>

<script>
import NavBarCtrlBtn from "@/components/NavBarCtrlBtn";

export default {
  name: 'Login',
  components: {
    NavBarCtrlBtn
  },
  data() {
    return {
      xh: '',
      pwd: '',
      tip: ''
    }
  },
  methods: {
    login() {
      window.api
          .getAuthUser(this.xh, this.pwd)
          .then(r => {
            if (r.success) {
              localStorage.setItem('userData', JSON.stringify(r))
              window.api
                  .updateUserData()
                  .then(() => {
                    this.$router.push({path: '/'})
                  })
            } else this.tip = '学号或密码错误'
          })
    }
  }
}
</script>

<style scoped>

.frame-height {
  height: 36px;
}

* {
  /*border: solid 1px;*/
}

.drag {
  -webkit-app-region: drag
}

input::placeholder {
  color: gray;
}
</style>
