<template>
    <div class="w-25">
        <input type="text" v-model="title" placeholder="Title" class="mb-3">
        <div ref="dropzone" class="p-5 text-light text-center bg-dark">
            Upload
        </div>
        <div class="mb-3 mt-3">
            <vue-editor  useCustomImageHandler
                         @image-added="handleImageAdded" v-model="content" />
        </div>
        <input @click.prevent="store" type="submit" value="Add" class="btn btn-primary mt-3">

        <div class="mt-5">
            <div v-if="post" >
                <h2>{{post.title}}</h2>
                <div v-for="image in post.images" class="mt-5">
                    <img :src="image.preview_url">
                    <img :src="image.url">
                </div>

                <div class="ql-editor" v-html="post.content"></div>

            </div>
        </div>
    </div>
</template>

<script>
import Dropzone from 'dropzone'
import { VueEditor } from "vue2-editor";

export default {
    name: "Index",
    data() {
        return {
            dropzone: null,
            title:'',
            post:[],
            content:'',
        }
    },
    components:{
        VueEditor
    },
    mounted() {
        this.dropzone = new Dropzone(this.$refs.dropzone, {
            url: '/api/posts',
            autoProcessQueue: false,
            addRemoveLinks: true,
        })

        this.getPost()
    },
    methods: {
        store() {
            const data = new FormData();
            const files = this.dropzone.getAcceptedFiles();
            files.forEach( file => {
                data.append('images[]',file);
                this.dropzone.removeFile(file)
            })
            data.append('title', this.title)
            data.append('content', this.content)

            this.title = ''
            axios.post('/api/posts', data)
                .then(res => {
                    this.getPost()
                })
        },
        getPost(){
            axios.get('/api/posts')
            .then(res => {
                this.post = res.data.data
            })
        },
        handleImageAdded(file, Editor, cursorLocation, resetUploader){
            const formData = new FormData();
            formData.append("image", file);

            axios.post("/api/posts/images", formData)
                .then(result => {
                    const url = result.data.url; // Get url from response
                    Editor.insertEmbed(cursorLocation, "image", url);
                    resetUploader();
                })
                .catch(err => {
                    console.log(err);
                });
        }
    }
}
</script>

<style scoped>

</style>
