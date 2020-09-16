<template>
    <div class="login_container">
        <div class="login_box">
            <!-- 头像区域 -->
            <div class="avatar_box">
                <img src="../assets/logo.png" alt="" />
            </div>
            <!-- 登录表单区域 -->
            <el-form
                ref="loginFormRef"
                :rules="loginFormRules"
                class="login_form"
                :model="loginForm"
            >
                <!-- 用户名 -->
                <el-form-item prop="username">
                    <el-input
                        v-model="loginForm.username"
                        prefix-icon="iconfont icon-user"
                        type="text"
                        placeholder="请输入用户名"
                    ></el-input>
                </el-form-item>
                <!-- 密码 -->
                <el-form-item prop="password">
                    <el-input
                        v-model="loginForm.password"
                        prefix-icon="iconfont icon-3702mima"
                        type="password"
                        placeholder="请输入密码"
                    ></el-input>
                </el-form-item>
                <!-- 按钮区域 -->
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="resetLoginForm"
                        >重置</el-button
                    >
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            // 登录表单的数据绑定对象
            loginForm: {
                username: "admin",
                password: "123456",
            },
            // 表单验证规则
            loginFormRules: {
                // 验证用户名是否合法
                username: [
                    {
                        required: true,
                        message: "请输入登录名称",
                        trigger: "blur",
                    },
                    {
                        min: 3,
                        max: 10,
                        message: "长度在3到10个字符",
                        trigger: "blur",
                    },
                ],
                // 验证密码是否合法
                password: [
                    {
                        required: true,
                        message: "请输入登录密码",
                        trigger: "blur",
                    },
                    {
                        min: 6,
                        max: 15,
                        message: "长度在6到15个字符",
                        trigger: "blur",
                    },
                ],
            },
        };
    },
    methods: {
        resetLoginForm() {
            // console.log(this);
            this.$refs.loginFormRef.resetFields();
        },
        login() {
            this.$refs.loginFormRef.validate(async (valid) => {
                // 布尔值 表示表单本地验证合法
                if (!valid) return;
                // 发送登录请求 解构响应数据
                const { data: res } = await this.$http.post(
                    "login",
                    this.loginForm
                );
                // 判断响应结果
                if (res.meta.status !== 200)
                    return this.$message.error("登录失败");
                this.$message.success("登录成功");
                // console.log(res.data.token);
                
                // localStorage or sessionStorage
                // 前者持久化储存，后者一次性页面内储存，对于登录操作应保证安全，选后者
                window.sessionStorage.setItem("token", res.data.token);
                // 登录成功后跳转到后台主页
                this.$router.push("/home");
            });
        },
    },
};
</script>

<style lang="less" scoped>
.login_container {
    height: 100%;
    background-color: #2b4b6b;
}

.login_box {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);

    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;

    .avatar_box {
        position: absolute;
        left: 50%;
        transform: translate(-50%, -50%);
        height: 130px;
        width: 130px;
        border: 1px solid #eee;
        border-radius: 50%;
        padding: 10px;
        box-shadow: 0 0 10px #ddd;
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
