<template>
    <div class="login_container">
        <div class="login_box">
            <div class="avatar_box">
                <img src="../assets/logo.png"> 
            </div>
            
                <el-form ref="loginFormRef" label-width="0px" class="login_form" :model="loginForm" :rules="loginFormRules" >
                    <el-form-item prop="username">
                        <el-input prefix-icon="iconfont icon-user" v-model="loginForm.username" placeholder="用户名"></el-input>
                    </el-form-item>
                    <el-form-item prop="password">
                        <el-input prefix-icon="iconfont icon-3702mima" v-model="loginForm.password" type="password" placeholder="密码"></el-input>
                    </el-form-item>
                    <el-form-item class="btns">
                       <el-button type="primary" @click="login">登录</el-button>
                       <el-button type="info" @click="resetForm">重置</el-button>
                    </el-form-item>
                </el-form>
            
        </div>
    </div>
</template>

<script>
import { async } from 'q'
export default {
    data() {
        return {
            loginForm:{
                username:'admin',
                password:'123456',

            },
            loginFormRules:{
                username:[
                    { required: true, message: '请输入用户名', trigger: 'blur' },
                    { min: 3, max: 15, message: '长度在 3 到 15个字符', trigger: 'blur' }
                ],
                password:[
                    { required: true, message: '请输入密码', trigger: 'blur' },
                    { min: 6, max: 15, message: '长度在 6 到 15个字符', trigger: 'blur' } 
                ]
            }
        }
    },
    methods: {
        resetForm(){
            //console.log(this)
            //this.$refs.loginFormef.resetFiels()
            this.$refs.loginFormRef.resetFields()
        },
        login(){
            this.$refs.loginFormRef.validate(async (valid) => {
                //console.log(valid);
                if(!valid)
                return
                //const result=await this.$http.post("login",this.loginForm)
                const { data:res }=await this.$http.post("login",this.loginForm)
                if(res.meta.status!=200)
                return this.$message.error("账户密码错误")
                this.$message.success("登录成功")
                window.sessionStorage.setItem("token",res.data.token)
                this.$router.push("/home")                                           
            })
        }

    },
    
}
</script>

<style lang="less" scoped>
.login_container {
   background-color: #2b4b6b;
   height: 100%
}

.login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 5px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%); 

    .avatar_box {
    height: 120px;
    width: 120px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 5px;
    box-shadow: 0 0 10px rgb(238, 221, 221);
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;

    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }

}

.login_form {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    box-sizing: border-box;
}
.btns {
  display: flex;
  justify-content: flex-end;
}
</style>