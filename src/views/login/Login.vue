<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="login-page">
    <div class="login-box">
      <h1>e店QAQ 平台</h1>
      <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
        <el-form-item label="用户名" prop="username">
          <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item :label="`密\xa0\xa0\xa0\xa0码`" prop="password">
          <el-input type="password" v-model="ruleForm.password" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="验证码" prop="captchacode">
          <div class="captcha-box">
            <el-input v-model.number="ruleForm.captchacode"></el-input>
            <img height=40 :src="captchaSrc" alt="" @click="getCaptchacode">
          </div>
        </el-form-item>
        <el-form-item id="login-btn-box">
          <el-button class="login-btn" type="primary" @click="submitForm('ruleForm')">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import {validateUsername} from "@/utils/validate"
import axios from "axios"
export default {
  data (){
    return{
      ruleForm:{
        username: "",
        password: "",
        captchacode:"",
      },
      rules:{
        username:[
          {
            required:true, // 必填项
            message:'用户名不能为空', // 未填入的提示语
            trigger:'blur',
          },
          // 自定义校验写法
          { validator: validateUsername, trigger: 'blur' }
        ],
        password:[
          {
            required:true, // 必填项
            message:'密码不能为空', // 未填入的提示语
            trigger:'blur',
          }
        ],
        captchacode:[
          {
            required:true, // 必填项
            message:'验证码不能为空', // 未填入的提示语
            trigger:'blur',
          }
        ],
      },
      // 图片验证码路径
      captchaSrc:"data:image/gif;base64,"
    }
  },
  created(){
    this.getCaptchacode()
  },
  methods:{
    getCaptchacode(){
      axios.get("http://xue.cnkdl.cn:23683/prod-api/captchaImage")
    .then(res=>{
      if(res.data.code===200){
        this.captchaSrc = "data:image/gif;base64," + res.data.img
      }
    })
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!');
        } else {
          this.$message({
            message: '请正确输入后登录',
            type: 'warning',
            // duration: 3000  默认值
          });
          return false;
        }
      });
    },
  }
}
</script>

<style lang="less" scoped>
.login-page{
  width: 100%;
  height: 100%;
  background: url(../../assets/images/loginBg.jpg) center top no-repeat;
  background-size:cover;
  position: relative;
  .login-box{
    width: 400px;
    background-color: #fff;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    padding-top: 20px;
    padding-right: 40px;
    border-radius: 16px;
    h1{
      text-align: center;
      margin: 20px;
      padding-left: 40px;
      font-size: 20px;
      color: rgb(84, 84, 84);
    }

    .captcha-box{
      display: flex;
      img{
        margin-left: 20px;
      }
    }

    .login-btn{
      width: 100%;
    }
    // 覆盖Element UI的样式!
    ::v-deep #login-btn-box .el-form-item__content{
      margin-left: 40px !important;
    }
  }
}
</style>