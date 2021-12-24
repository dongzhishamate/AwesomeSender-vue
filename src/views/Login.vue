<template>
    <div style="width: 100%; height: 100vh; background-color: darkcyan; overflow: hidden;">
        <div style="width: 500px; margin: 150px auto;">
            <div style="color: #ccc; font-size: 60px; text-align: center; padding: 30px 0;">欢迎登录</div>
            <el-form refw="form" :model="form" size="normal">
                <el-form-item>
                    <el-input v-model="form.name">
                        <template #prefix>
                            <el-icon class="el-input__icon"><user /></el-icon>
                        </template>
                    </el-input>
                </el-form-item>
                <el-form-item>
                    <el-input v-model="form.password" show-password="true">
                        <template #prefix>
                            <el-icon class="el-input__icon"><lock /></el-icon>
                        </template>
                    </el-input>
                </el-form-item>
                <el-form-item>
                    <el-button style="width: 100%;" type="primary" @click="login">登录</el-button>
                </el-form-item>
                <el-form-item>
                    <el-button style="width: 100%;" type="primary" @click="register">注册</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
    import { Lock,User } from '@element-plus/icons';
    import request from "../utils/request";

    export default {
        name: "Login",
        components:{
            User,
            Lock
        },
        data() {
            return {
                form: {}
            }
        },
        methods: {
            login() {
                request.post("/student/login", {
                    name: this.form.name,
                    password: this.form.password
                }).then(res => {
                    if (res.code == '0') {
                        this.$message({
                            type: "success",
                            message: "登录成功"
                        })
                        sessionStorage.setItem("user", JSON.stringify(res.data))
                        this.$router.push("/")
                    }
                    else {
                        this.$message({
                            type: "error",
                            message: res.msg
                        })
                    }
                })
            },
            register() {
                this.$router.push("/register")
            }
        },
    }
</script>

<style scoped>

</style>
