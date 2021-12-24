<template>
    <div style="width: 100%; height: 100vh; background-color: darkcyan; overflow: hidden;">
        <div style="width: 500px; margin: 150px auto;">
            <div style="color: #ccc; font-size: 60px; text-align: center; padding: 30px 0;">欢迎注册</div>
            <el-form ref="form" :model="form" size="normal" :rules="rules">
                <el-form-item prop="name">
                    <el-input v-model="form.name">
                        <template #prefix>
                            <el-icon class="el-input__icon"><user /></el-icon>
                        </template>
                    </el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input v-model="form.password" show-password="true">
                        <template #prefix>
                            <el-icon class="el-input__icon"><lock /></el-icon>
                        </template>
                    </el-input>
                </el-form-item>
                <el-form-item prop="confirm">
                    <el-input v-model="form.confirm" show-password="true">
                        <template #prefix>
                            <el-icon class="el-input__icon"><lock /></el-icon>
                        </template>
                    </el-input>
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
        name: "Register",
        components() {
            Lock
            User
        },
        data() {
            return {
                form: {},
                rules: {
                    name: [
                        {required: true, message: '请输入用户名', trigger: 'blur'},
                    ],
                    password: [
                        {required: true, message: '请输入密码', trigger: 'blur'},
                    ],
                    confirm: [
                        {required: true, message: '请确认密码', trigger: 'blur'},
                    ],
                }
            }
        },
        methods: {
            register() {
                if (this.form.password !== this.form.confirm) {
                    this.$message({
                        type: "error",
                        message: "两次密码输入不一致!"
                    })
                    return
                }

                this.$refs.form.validate((valid) =>{
                    if (valid) {
                        request.post("/student/register", this.form).then(res => {
                            if (res.code === '0') {
                                this.$message({
                                    type: "success",
                                    message: "创建成功"
                                })
                                this.$router.push("/login")
                            } else {
                                this.$message({
                                    type: "error",
                                    message: res.msg
                                })
                            }
                        })
                    }
                })
            }
        }
    }
</script>

<style scoped>

</style>
