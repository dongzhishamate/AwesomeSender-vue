<template>
    <el-row class="tac">
        <el-col :span="12">
            <el-menu
                    style="width: 200px; min-height: calc(100vh - 50px)"
                    :default-active="$route.path"
                    router
            >
                <el-sub-menu>
                    <template #title>
                        <el-icon><location /></el-icon>
                        <span>系统管理</span>
                    </template>
                    <el-menu-item index="home">首    页</el-menu-item>
                    <el-menu-item index="user">用户管理</el-menu-item>
                    <el-menu-item index="book">
                        <el-icon><icon-menu /></el-icon>
                        <span>书籍管理</span>
                    </el-menu-item>
                </el-sub-menu>
            </el-menu>
        </el-col>
    </el-row>
</template>

<script>

    import { defineComponent } from 'vue'
    import {
        Location,
        Document,
        Menu as IconMenu,
        Setting,
    } from '@element-plus/icons'

    export default defineComponent({
        name: "Aside",
        components: {
            Location,
            Document,
            Setting,
            IconMenu,
        },
        setup() {
            return {

            }
        },
    })
</script>

<style scoped>

</style>
