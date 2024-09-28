<template>
    <div class="input-container">
        <!-- 主题输入框 -->
        <input class="input-box" v-model="internalValue" @input="handleInput" @blur="validateInput" />

        <!-- 左上角嵌入式文字提示 -->
        <div v-if="props.title!==''" class="input-float-text">{{ title }}</div>

        <!-- 文字清空按钮 -->
        <div v-if="internalValue!==''" class="input-clear-container" @click="clearInputValue">
            <CloseCircleOutlined />
        </div>

        <!-- 右上角状态标识图标 -->
        <div v-if="showHint" class="input-hint-container">
            <div v-if="hint!==''">
                <a-tooltip placement="topLeft">
                    <template #title>{{ hint }}</template>
                    <ExclamationCircleOutlined />
                </a-tooltip>
            </div>
            <div v-else>
                <CheckCircleOutlined/>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, watch, defineProps, defineEmits, onMounted } from 'vue';
import { ExclamationCircleOutlined, CloseCircleOutlined, CheckCircleOutlined } from '@ant-design/icons-vue';

// 定义接收的属性
const props = defineProps({
    title: {
        type: String,
        default: '',
    },
    modelValue: {
        type: String,
        default: '',
    },
    type: { // info warning
        type: String,
        default: ''
    },
    validateMethod: {
        type: Function,
    },
});



// 事件触发，用于与父组件通信
const emit = defineEmits(['update:modelValue']);

// 组件内部的 value，保持与父组件同步
const internalValue = ref(props.modelValue);

// 监听外部值，同步到内部
watch(
    () => props.modelValue,
    (fatherValue) => {
        internalValue.value = fatherValue;
    }
);

// 监听内部值，同步到外部
watch(
    () => internalValue.value,
    (childValue) => {
        emit('update:modelValue', childValue)
    }
);

const hint = ref('此处不能为空');
const showHint = ref(false);

// 根据type控制hint模块显示与否
onMounted(() => {
    if (newValue === 'info' || 'warning') {
        showHint.value = true;
    } else {
        showHint.value = false;
    }
})

// 清除输入框的值
function clearInputValue() {
    internalValue.value = '';
    hint.value = '此处必须填写';
}

// 验证输入内容
function validateInput() {

    if (type === 'info' || type === 'warning') {

        let isEmpty = false;

        if (_.isEmpty(internalValue.value)) {

            isEmpty = false;
            hint.value = '此处必须填写';
            // 对应css变化
        } else if (!isEmpty && !_.isEmpty(props.validateMethod)) {

            let res = props.validateMethod(internalValue.value)

            if (res === '') {
                //成功对应操作
                //主要是颜色的更改已经图标的更改
            } else {
                //失败对应操作
                //hint的更新，以及颜色更改，
                hint.value = res;
            }
        }
    }
}
</script>

<style scoped>
.input-container {
    box-sizing: border-box;
    display: flex;
    align-items: end;
    width: 300px;
    height: 60px;
}

.input-box {
    position: relative;
    width: 300px;
    height: 50px;
    box-sizing: border-box;
    padding-left: 10px;
    padding-right: 30px;
    padding-top: 10px;
    padding-bottom: 10px;
    border-radius: 14px;
    border: 1.5px solid black;
    outline: none;
    transition: ease-in-out 0.3s;
}

/* 聚焦时缩放效果 */
.input-box:focus {
    transform: scale(1.02);
}

/* 失去焦点时恢复效果 */
.input-box:not(:focus) {
    transform: scale(1);
}

.input-float-text {
    position: absolute;
    margin-bottom: 35px;
    margin-left: 20px;
    height: 20px;
    max-width: 100px;
    font-size: 12px;
    padding-left: 5px;
    padding-right: 5px;
    background-color: white;
}

.input-clear-container {
    position: absolute;
    width: 30px;
    height: 30px;
    margin-bottom: 10px;
    margin-left: 270px;
    font-size: 17px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
}

.input-hint-container {
    position: absolute;
    width: 20px;
    height: 25px;
    margin-bottom: 35px;
    margin-left: 300px;
    display: flex;
    align-items: center;
    justify-content: end;
}
</style>