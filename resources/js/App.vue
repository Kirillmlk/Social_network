<script>
export default {
    name: "App"
}
</script>

<template>
    <div>
        <h1 class="text-3xl font-bold ">
            Hello world!
        </h1>
        <router-link to="index">Index component</router-link>
        <router-link to="page">Page component</router-link>
        <router-view></router-view>
    </div>
</template>

<style scoped>

</style>
