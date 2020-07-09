<template>
    <div id="login">
        <div class= "login-wrap">
            <ul calss = "menu-tab">
                <li  v-for="item in menuTab" :key="item.id" :class="{'current':item.current}" @click="toggleMneu(item)">{{item.txt}}</li>
            </ul>
            <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm"  class="login-form" size="medium">
          
            <el-form-item  prop="username" class="item-form">
                <label >邮箱</label>
                <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
            </el-form-item>
           
            <el-form-item  prop="password" class="item-form">
                <label >密码</label>
                <el-input type="password" v-model="ruleForm.password" autocomplete="off" minlength="6" maxlength="20"></el-input>
            </el-form-item>
             <el-form-item  prop="passwords" class="item-form" v-if ="model === 'register'">
                <label >确认密码</label>
                <el-input type="passwords" v-model="ruleForm.passwords" autocomplete="off" minlength="6" maxlength="20" ></el-input>
            </el-form-item>
            
            <el-form-item prop="code" class="item-form">
                <label >验证码</label>
                <el-row :gutter="10">
                  <el-col :span="12"><el-input v-model.number="ruleForm.code" minlength="6" maxlength="6"></el-input>
                  </el-col>
                  <el-col :span="10">
                    <el-button type="success">获取验证码</el-button>
                  </el-col>
                </el-row>
            </el-form-item>   
            <el-form-item>
                <el-button type="danger" @click="submitForm('ruleForm')" class="login-btn block">提交</el-button>
                
            </el-form-item>
            </el-form>
        </div>
    </div>
</template>
<script>

export default {
    name:'login',
    data(){
      
      //验证用户名
      var validateusername = (rule, value, callback) => {
        let rag=/^[A-Za-z0-9\u4e00-\u9fa5]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
        if (value === '') {
          callback(new Error('请输入用户名'));
        } else if(!rag.test(value)){
          callback(new Error('用户名格式有误'));
        }else{
          callback();
        }
      };
      //验证密码
      var validatepassword = (rule, value, callback) => {
        let rag=/^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{6,20}$/;
        value = this.ruleForm.password
        if (value === '') {
          callback(new Error('请输入密码'));
        } else if (!rag.test(value)){
          callback(new Error('密码为6~20位的数字加字母!'));
        } else {
          callback();
        }
      };
      //验证确认密码
       var validatepasswords = (rule, value, callback) => {
        let rag=/^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{6,20}$/;
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value != this.ruleForm.password){
          callback(new Error('重复密码不正确'));
        } else {
          callback();
        }
      };
      //验证验证码
         var checkcode = (rule, value, callback) => {
           let rag=/^[a-z0-9]{6}$/;
         if (value === '') {
            return callback(new Error('请输入验证码'));
        }else if (!rag.test(value)){
            return callback(new Error('验证码格式有误!'));
        }else{
            callback();
        }
      };
        return{
            menuTab:[
                { txt:'登录',current: true,type:'login'},
                { txt:'注册',current: false,type:'register'}
            ],
            model: 'login',
            //表单的数据
         ruleForm: {
         username: '',
          password: '',
          passwords: '',
          code: ''
        },
        rules: {
         username: [
            { validator: validateusername, trigger: 'blur' }
          ],
         password: [
            { validator: validatepassword, trigger: 'blur' }
          ],
          passwords: [
            { validator: validatepasswords, trigger: 'blur' }
          ],
          code: [
            { validator: checkcode, trigger: 'blur' }
          ]
        }
        }
    },
    created(){},
    //加载完自动执行
    mounted(){
       
    },
     //Vue数据渲染
    methods:{
       toggleMneu(data){
           console.log(data)
           this.menuTab.forEach((elem,index) => {
             elem.currnet= false    
           });
           data.current = true
           //修改模块值
          this.model=data.type
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
    }
}
</script>
<style lang="scss" scoped>
#login {
    height: 100vh;
    background-color: rgb(0, 162, 255);
}
.login-wrap {
    width: 330px;
    margin: auto;
}
.menu-tab{
    text-align: center;
    li{
        display: inline-block;
        width: 88px;
        line-height: 36px;
        font-size: 14px;
        color: chartreuse;
        border-radius: 2px;
        cursor:pointer;
    }
    .current{
        background-color: rgb(255, 0, 85);
    }
}
.login-form{
    margin-top:29px;
    label{
        display: block;
        font-size: 14px;
        margin-bottom: 3px;
        color: #ffffff;
    }
  .item-form{
    margin-bottom: 13px;

  }
  .block{
    width:100%;
    display: block;
  }
  .login-btn{margin-top: 19px;}
}
</style>