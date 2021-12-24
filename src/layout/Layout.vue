<template>
    <div>
        <!--头部-->
        <Header/>
        <!--主体-->
        <div style="display: flex">
            <!--侧边栏-->
            <Aside/>
            <!--内容区域-->
            <router-view style="flex: 1;"></router-view>
        </div>
    </div>
</template>

<script>

    import Aside from "../components/Aside";
    import Header from "../components/header"

    export default {
        name: "Layout",
        components: {
            Header,
            Aside
        }
    }
</script>

<style scoped>

</style>
