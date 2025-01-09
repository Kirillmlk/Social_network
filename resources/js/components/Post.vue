<script>
export default{
    name: "Post",

    props: [
        'post',
    ]
}
</script>

<template>
    <div class="mb-6 pb-6 border-b border-black text-center">
        <h1 class="text-xl">{{ post.title }}</h1>
        <img v-if="post.image_url" class="my-3 mx-auto" :src="post.image_url" :alt="post.title"/>
        <p>{{ post.content }}</p>
        <p class="ml-auto mt-2 text-slate-500 text-right">{{ post.date }}</p>
    </div>
</template>

<style scoped>

</style>
