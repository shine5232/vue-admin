<template>
  <div id="login">
    <div class="login-wrap">
      <ul class="menu-tab">
        <li v-for="item in menuTab" :key="item.id" :class="{ current: item.current }" @click="toggleMenu(item)">
          {{ item.text }}
        </li>
      </ul>
      <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" size="medium" class="ruleForm">
        <el-form-item prop="email" class="item-form">
          <label>邮箱</label>
          <el-input v-model="ruleForm.email" placeholder="请输入邮箱" autocomplete="off" ></el-input>
        </el-form-item>
        <el-form-item prop="pass" class="item-form">
          <label>密码</label>
          <el-input type="password"  v-model="ruleForm.pass" placeholder="请输入密码" autocomplete="off" maxlength="20" minlength="6"></el-input>
        </el-form-item>
        <el-form-item prop="pass2" class="item-form" v-show="model === 'register'">
          <label>重复密码</label>
          <el-input type="password"  v-model="ruleForm.pass2" placeholder="请再次输入密码" autocomplete="off" maxlength="20" minlength="6"></el-input>
        </el-form-item>
        <el-form-item prop="code" class="item-form">
          <label>验证码</label>
          <el-row :gutter="10">
            <el-col :span="15">
              <el-input v-model.number="ruleForm.code" placeholder="请输入验证码" autocomplete="off" maxlength="6" minlength="6"></el-input>
            </el-col>
            <el-col :span="9">
              <el-button type="success" class="block">获取验证码</el-button>
            </el-col>
          </el-row>
        </el-form-item>
        <el-form-item>
          <el-button type="danger" class="login-btn block" @click="submitForm('ruleForm')" >提交</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
import { stripscript,validateEmail,validatePassword } from '@/utils/validate';
export default {
  name: "login",
  data() {
    //验证邮箱
    var vEmail = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入邮箱'));
      } else if(!validateEmail(value)){
        callback(new Error('邮箱格式有误'));
      }else {
        callback();
      }
    };
    //验证密码
    var vPass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'));
      } else if(!validatePassword(value)){
        callback(new Error('密码为6~20位的数字、字母组合'));
      }else {
        callback();
      }
    };
    //再次验证密码
    var vPass2 = (rule, value, callback) => {
      if(this.model === 'login'){
        callback();
      }
      if (value === '') {
        callback(new Error('请再次输入密码'));
      } else if(value != this.ruleForm.pass){
        callback(new Error('两次密码不一致'));
      }else{
        callback();
      }
    };
    //验证验证码
    var vCode = (rule, value, callback) => {
      let reg = /^[a-z0-9]{6}$/;
      if (value === '') {
        callback(new Error('验证码不能为空'));
      }else if(!reg.test(value)){
        callback(new Error('验证码格式有误'));
      }else{
        callback();
      }
    };
    return {
      menuTab: [
        { text: "登录", current: true ,type: 'login'},
        { text: "注册", current: false ,type: 'register'},
      ],
      model: 'login',
      ruleForm: {
        email: '',
        pass: '',
        pass2: '',
        code: ''
      },
      rules: {
        email: [
          { validator: vEmail, trigger: 'blur' }
        ],
        pass: [
          { validator: vPass, trigger: 'blur' }
        ],
        pass2: [
          { validator: vPass2, trigger: 'blur' }
        ],
        code: [
          { validator: vCode, trigger: 'blur' }
        ]
      },
    }
  },
  created() {},
  mounted() {},
  methods: {
    toggleMenu(data) {
      this.menuTab.forEach((ele) => {
        ele.current = !ele.current;
      });
      this.model = data.type;
    },
    submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
  },
};
</script>
<style lang="scss" scoped>
#login {
  height: 100vh;
  background: #344a5f;
  display: flex;
  align-items: center;
  justify-content: center;
}
.login-wrap {
  width: 330px;
  margin: 0 auto;
}
.menu-tab {
  text-align: center;
  li {
    display: inline-block;
    width: 88px;
    line-height: 36px;
    font-size: 14px;
    color: #fff;
    border-radius: 2px;
    cursor: pointer;
  }
}
.current {
  background-color: rgba(0, 0, 0, 0.1);
}
.ruleForm{
  margin-top:29px;
  label{
    display:block;
    color:#fff;
    font-size:14px;
    margin-bottom:3px;
  }
  .item-form{
    margin-bottom:13px;
  }
  .block{
    width:100%;
    display:block;
  }
  .login-btn{
    margin-top:19px;
  }
}
</style>