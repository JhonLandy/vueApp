<template>
  <div class="login-section">
    <!-- :rules="rules" -->
    <el-form 
      label-position="top"
       :rules="relues"
       :model="form"
       status-icon
       ref="loginForm" label-width="100px" class="demo-ruleForm"
    >
      <el-form-item label="用户名" prop="username">
        <el-input type="text" v-model="form.username" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" v-model="form.password" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('loginForm')">提交</el-button>
        <el-button @click="resetForm('loginForm')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
import {login} from '@/service/api';

export default {
  data() {
    return {
      form: {
        username: "",
        password: ""
      },
      relues: {
        username: [
          {required: true, message: '不能为空',trigger: 'blur'}
        ],
        password: [
          {required: true, message: '不能为空', trigger: 'blur'}
        ]
      }
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate( valid => {
        if(valid) {
          login({
            name: this.form.username,
            password: this.form.password
          }).then((data) => {
            if(data.code === 0) {  // 成功
              localStorage.setItem('token', data.data.token);
              window.location.href = '/';
            }
            if(data.code === 1){
              this.$message.error(data.mes);
            }
          })
        } else {
          this.$message.error("登录失败！");
          return false;
        }
      })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  }
}
</script>

<style lang="stylus">
.login-section
  padding 0px 20px
</style>
