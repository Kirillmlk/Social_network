<template>
    <div class="w-96 mx-auto">
        <div class="mb-4">
            <div>
                <input v-model="title" type="text" placeholder="title"
                       class="mb-3 w-96 rounded-3xl border p-2 border-slate-300">
            </div>
            <div>
                <textarea v-model="content" placeholder="content"
                          class="w-96 mb-3 rounded-3xl border p-2 border-slate-300"></textarea>
            </div>
            <div class="flex mb-3 items-center">
                <div>
                    <input @change="storeImage" ref="file" type="file" class="hidden">
                    <a href="#" class="block p-2 w-16 text-center text-sm rounded-3xl bg-sky-500 text-white"
                       @click.prevent="selectFile()">Image</a>
                </div>
                <div>
                    <a @click.prevent="image = null" href="#" class="ml-2">Cancel</a>
                </div>
            </div>
            <div v-if="image">
                <img :src="image.url" alt="preview">
            </div>
            <div>
                <a @click.prevent="store" href="#"
                   class="block p-2 w-32 bg-green-600 rounded-3xl text-center text-white hover:bg-white hover:border
                   hover:border-green-600 hover:text-green-600 ml-auto">Publish</a>
            </div>
        </div>

        <div v-if="posts">
            <h1 class="mb-4 pb-8 text-center">Posts</h1>
            <Post v-for="post in posts" :post="post"></Post>
        </div>
    </div>
</template>

<script>
import Post from "../../components/Post.vue";
// import Stat from "../../components/Stat.vue";
export default {
    name: "Personal",

    data() {
        return {
            title: '',
            content: '',
            image: null,
            posts: [],
            errors: [],
            stats: []
        }
    },

    components: {
        Post
    },

    mounted() {
        this.getPosts()
        // this.getStats()
    },

    methods: {

        // getStats() {
        //     axios.post('/api/users/stats', { user_id: null})
        //     .then( res => {
        //         this.stats = res.data.data
        //     })
        // },

        getPosts() {
            axios.get('/api/posts')
                .then(res => {
                    this.posts = res.data.data
                })
        },

        store() {
            const id = this.image ? this.image.id : null
            axios.post('/api/posts', {title: this.title, content: this.content, image_id: id})
                .then(res => {
                    this.title = ''
                    this.content = ''
                    this.image = null
                    this.posts.unshift(res.data.data)
                })
                .catch(e => {
                    this.errors = e.response.data.errors
                })
        },
        selectFile() {
            this.fileInput = this.$refs.file;
            this.fileInput.click();
        },

        storeImage(e) {
            const file = e.target.files[0]
            const formData = new FormData()
            formData.append('image', file)

            axios.post('/api/post_images', formData)
                .then(res => {
                    this.image = res.data.data
                })
        }

    }
}
</script>

<style scoped>

</style>
