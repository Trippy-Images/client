<template>
    <b-col cols="6" class="d-flex flex-column border p-3">
        <img class="rounded p-3" v-if="url" :src="url" style="align-self: center; height: 300px">
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
            file: null
        }
    },
    methods: {
        onFileChange(e){
            const file = e.target.files[0]
            this.url = URL.createObjectURL(file)
            this.$emit('imageSelected', file)
        }
    }
}
</script>

<style>

</style>