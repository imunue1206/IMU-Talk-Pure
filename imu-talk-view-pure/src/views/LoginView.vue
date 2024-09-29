<template>
    <div class="login-box box">
        <ImuInput v-model="email" title="邮箱" type="info" modelValue="email" :validator="emailCheck" />
        <ImuInput v-model="password" title="密码" type="info" modelValue="password" />
        <button class="btn" @click="handleLogin">登陆</button>
        <a class="black" href="/register">没有账号？ 前往注册</a>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { message } from 'ant-design-vue'
import request from '@/api/BaseRequest.js'
import ImuInput from '@/components/ImuInput.vue';

const email = ref('');
const password = ref('');
const router = useRouter();

const emailCheck = (value) => {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/; 
    if (emailRegex.test(value)) {
        return ''; // 邮箱格式正确时返回空字符串
    } else {
        return '邮箱格式错误'; // 邮箱格式不正确时返回错误提示
    }
}

const handleLogin = async () => {
    if (!email.value || !password.value) {
        message.error('请输入邮箱和密码');
        return;
    }

    try {
        const response = request.post('/login', {
            email: email.value,
            password: password.value
        });

        if (response.success) {
            message.success('登录成功');
            router.push('/home');
        } else {
            message.error(response.message || '登录失败');
        }
    } catch (error) {
        console.error('Login error:', error);
        message.error('请求失败，请重试');
    }
};
</script>

<style scoped>

.login-box {
}
</style>