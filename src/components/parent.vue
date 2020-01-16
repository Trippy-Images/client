<template>
    <div id="Content">
    <NavBar></NavBar>
    <b-container fluid>
    <b-row class="text-center justify-content-md-center">
        <b-col cols="8" id="loginPart" >
            <b-row>

            </b-row>
        </b-col>
    </b-row>
    <b-row class="text-center justify-content-md-center" v-if="resultPart">
        <b-col cols="8" id="resultImage"><img v-bind:src="resultPart"/></b-col> 
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
            this.resultPart = 'https://i.kym-cdn.com/photos/images/original/001/582/190/6d2.png'
            // ini nanti get ke server
        },
        againButton(){
            this.resultPart = null
        }
    }
}

</script>

<style>
    #resultImage {
        background-color: rgba(0, 0, 0, 0.5);
        height: 60vh;
        border-radius: 15px
    }
    #Content {
        background-image: url('../../media/Background.jpg');
        height: 100vh;
    }
    #loginPart {
        background-color: rgba(0, 0, 0, 0.5);
        height: 30vh;
        border-radius: 15px
    }
</style>