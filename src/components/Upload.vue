<template>
    <b-col cols="6" class="d-flex flex-column border p-3">
        <img class="rounded p-3 image-fluid" v-if="url" :src="url">
        <b-form-file @change="onFileChange"
        v-model="file"
        :state="Boolean(file)"
        placeholder="Choose a file or drop it here..."
        drop-placeholder="Drop file here..."
        ></b-form-file>
    </b-col>
</template>

<script>
export default {
    name: 'Upload',
    data(){
        return {
            url: null,
            file: null
        }
    },
    methods: {
        onFileChange(e){
            const file = e.target.files[0]
            // console.log(e)
            this.url = URL.createObjectURL(file)
            this.$emit('imageSelected', file)
        }
    },
    props: [
        'deleteImage'
    ],
    watch:{
        deleteImage(newVal, oldVal){
                this.url = null
                this.file = null
        }
    }
}
</script>

<style scoped>
    img {
        align-self: center; 
        background-color: rgba(0, 0, 0, 0.5);
        height: 30vh;
        border-radius: 15px;
        padding: 2px;
        overflow: hidden;
    }
</style>

