<template>
    <div class="w-25">
        <div ref="dropzone" class="p-5 text-light text-center bg-dark">
            Upload
        </div>
    </div>
</template>

<script>
import Dropzone from 'dropzone'
export default {
    name: "Index",
    data(){
        return {
            dropzone: null,
        }
    },
    mounted(){
        this.dropzone = new Dropzone(this.$refs.dropzone,{
            url:'test'
        })
    }
}
</script>

<style scoped>

</style>
