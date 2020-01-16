<template>
    <div>
    <NavBar></NavBar>
    <b-container fluid>
    <b-row class="text-center justify-content-md-center" v-if="resultPart">
        <b-col cols="8" id="resultImage">Result Image</b-col> 
    </b-row>
    <b-row class="text-center justify-content-md-center" v-if="resultPart">
        <b-col>
            <button v-on:click="againButton">Again</button>
        </b-col>
    </b-row>
    <b-row class="text-center justify-content-md-center">
        <Kenny></Kenny>
        <Kenny></Kenny>
    </b-row>
    <b-row class="text-center justify-content-md-center">
        <b-col>
            <button v-on:click="ResultButton" v-if="resultPart==null">Result</button>
        </b-col>
    </b-row>
</b-container>
    </div>
</template>

<script>
import NavBar from './NavBar'
import Kenny from './Kenny'

export default {
    name: 'parent',
    components: {
        NavBar,
        Kenny
    },
    data(){
        return {
            resultPart : null
        }
    },
    methods:{
        ResultButton(){
            this.resultPart = 'Result'
        },
        againButton(){
            this.resultPart = null
        }
    }
}

</script>

<style>
    #resultImage {
        border: black 2px solid;
        height: 45vh;
    }
</style>