<template>
    <div class="w-96 mx-auto">
        <!-- Проверяем наличие постов -->
        <div v-if="posts.length > 0">
            <h1 class="mb-8 pb-8 border-b border-gray-400">Posts</h1>
            <!-- Рендерим каждый пост -->
            <Post v-for="post in posts" :key="post.id" :post="post"></Post>
        </div>
        <!-- Сообщение, если постов нет -->
        <div v-else>
            <h1 class="mb-8 pb-8 border-b border-gray-400">No Posts</h1>
        </div>
    </div>
</template>

<script>
import Post from "../../components/Post.vue";

export default {
    name: "Feed",

    data() {
        return {
            posts: [], // Инициализация пустым массивом
        };
    },

    components: {
        Post,
    },

    mounted() {
        this.getPosts(); // Загружаем посты при монтировании
    },

    methods: {
        getPosts() {
            axios
                .get(`/api/users/following_posts`)
                .then((res) => {
                    // Проверяем наличие данных и преобразуем их
                    this.posts = Array.isArray(res.data) ? res.data : [];
                    console.log("Полученные посты:", this.posts);
                })
                .catch((err) => {
                    console.error("Ошибка при загрузке постов:", err);
                    this.posts = []; // Очищаем посты в случае ошибки
                });
        },
    },
};
</script>

<style scoped>
/* Добавьте стиль при необходимости */
</style>
