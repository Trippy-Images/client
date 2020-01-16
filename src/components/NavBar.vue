<template>
    <b-container fluid>
        <b-row id="parent">
            <b-col>Logo</b-col>
            <b-col>Nama Web</b-col>
            <b-col>User</b-col>
        </b-row>
    </b-container>
</template>

<script>
export default {
    name: 'NavBar'
}
</script>

<style scoped>
    #parent {
        border: 2px solid black
    }
</style>