<template>
  <div class="login-form">
    <div class="g-form">
      <!--<div class="g-form-line" v-for="formLine in formData">-->
      <div class="g-form-line">
        <span class="g-form-label">用户名</span>
        <div class="g-form-input">
          <input type="text" v-model="name" placeholder="请输入用户名" >
        </div>
      </div>
      <div class="g-form-line">
        <span class="g-form-label">密码</span>
        <div class="g-form-input">
          <input type="password" v-model="password1" placeholder="请输入密码">
        </div>
      </div>
      <div class="g-form-line">
        <span class="g-form-label">再一次输入密码</span>
        <div class="g-form-input">
          <input type="password" v-model="password2" placeholder="请输入密码">
        </div>
      </div>
      <div class="g-form-line">
        <div class="g-form-btn">
          <a class="button" @click="onReg">注测</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    'isShow': 'boolean'
  },
  data () {
    return {
      name: '',
      passwordl: '',
      passwordl2: '',
      errorText: ''
    }
  },
  computed: {
    userErrors () {
      let status, errorText
      if (!/@/g.test(this.name)) {
        status = false
        errorText = '必须包含@'
      } else {
        status = true
        errorText = ''
      }
      return {
        status,
        errorText
      }
    },
    passwordErrors () {
      let status, errorText
      if (!/^\w{1,6}$/g.test(this.password1)) {
        status = false
        errorText = '密码不是1-6位'
      } else {
        status = true
        errorText = ''
      }
      return {
        status,
        errorText
      }
    },
    check () {
      let status
      if (this.password1 === this.password2) {
        status = true
      } else {
        status = false
      }
      return {
        status
      }
    }
  },
  methods: {
    closeMyself () {
      this.$emit('on-close')
    },
    onReg () {
      if (!this.userErrors.status || !this.passwordErrors.status || this.check().status) {
        this.errorText = '部分选项未通过'
      } else {
        this.errorText = ''
        this.$http.get('api/login')
          .then((res) => {
            this.data = res.data
            this.$emit('has-log', res.data)
            console.log('4564')
          }, (error) => {
            console.log(error)
          })
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.dialog-wrap {
  position: fixed;
  width: 100%;
  height: 100%;
}
.dialog-cover {
  background: #000;
  opacity: .3;
  position: fixed;
  z-index: 5;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.dialog-content {
  width: 50%;
  position: fixed;
  max-height: 50%;
  overflow: auto;
  background: #fff;
  top: 20%;
  left: 50%;
  margin-left: -25%;
  z-index: 10;
  border: 2px solid #464068;
  padding: 2%;
  line-height: 1.6;
}
.dialog-close {
  position: absolute;
  right: 5px;
  top: 5px;
  width: 20px;
  height: 20px;
  text-align: center;
  cursor: pointer;
}
.dialog-close:hover {
  color: #4fc08d;
}
</style>
