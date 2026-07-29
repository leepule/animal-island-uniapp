<script setup lang="ts">

const PHONE_API = [
    { prop: 'apps', desc: '应用列表（内置默认动森风 App，一般无需传入）', type: 'AppDef[]', defaultVal: '内置' },
];

const code = `<!-- 动森风格手机界面，内置相机 / 地图 / 聊天等 App 图标 -->
<AiPhone />`;
</script>

<template>
    <AppLayout>
    <view class="demo-page">
        <DemoHeader name="phone" />

        <view class="demo-label">动森风格手机界面</view>
        <view class="demo-box" style="display: flex; justify-content: center; padding: 40rpx; overflow-x: auto">
            <AiPhone />
        </view>

        <AiCodeBlock :code="code" />
        <ApiTable :rows="PHONE_API" />
    </view>
    </AppLayout>
</template>
