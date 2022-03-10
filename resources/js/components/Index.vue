<template>
    <div class="w-25">
        <input type="text" v-model="title" placeholder="Title" class="mb-3">
        <div ref="dropzone" class="p-5 text-light text-center bg-dark">
            Upload
        </div>
        <input @click.prevent="store" type="submit" value="Add" class="btn btn-primary mt-3">
    </div>
</template>

<script>
import Dropzone from 'dropzone'

export default {
    name: "Index",
    data() {
        return {
            dropzone: null,
            title:'',
        }
    },
    mounted() {
        this.dropzone = new Dropzone(this.$refs.dropzone, {
            url: '/api/posts',
            autoProcessQueue: false,
            addRemoveLinks: true,
        })
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
            this.title = ''
            axios.post('/api/posts', data)
                .then(res => {

                })
        }
    }
}
</script>

<style scoped>

</style>
