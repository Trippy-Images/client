<template>
    <div id="Content">
    <NavBar></NavBar>
    <b-container fluid>
    <b-row class="text-center justify-content-md-center">
        <b-col cols="8" id="loginPart" v-if="!isLogin">
            <b-row class="text-center justify-content-md-center">
                <label for="email">email:</label>
                <input type="text" v-model="email">
            </b-row>
            <b-row class="text-center justify-content-md-center">
                <label for="password">password:</label>
                <input type="password" v-model="password">
            </b-row>
            <b-row class="text-center justify-content-md-center">
                <button>Login</button>
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
    <b-row class="text-center justify-content-md-center" v-if="isLogin">
        <Upload v-on:imageSelected="addImage($event, 0)"></Upload>
        <Upload v-on:imageSelected="addImage($event, 1)"></Upload>
    </b-row>
    <b-row class="text-center justify-content-md-center">
        <b-col>
            <button v-on:click="ResultButton" v-if="isLogin">Result</button>
        </b-col>
    </b-row>
</b-container>
    </div>
</template>

<script>
import NavBar from './NavBar'
import Upload from './Upload'
import axios from 'axios'

export default {
    name: 'parent',
    components: {
        NavBar,
        Upload
    },
    created(){
        if(localStorage.getItem('token')){
            this.isLogin = true
        }else{
            this.isLogin = true
        }
    },
    data(){
        return {
            resultPart : null,
            images: [],
            style: null,
            content: null,
            isLogin: false,
            email: '',
            password:''
        }
    },
    methods:{
        ResultButton(){
            axios({
                method: 'post',
                url: 'http://localhost:3000',
                data: {
                    images: this.images
                },
                headers:{
                    token: localStorage.getItem('token')
                }
            })
            .then(({ data })=>{
                this.resultPart = data.result
                this.style = data.style
                this.content = data.content
            })
        },
        againButton(){
            this.resultPart = null
            this.images = []

        },
        addImage(file, index){
            this.images[index] = file
        },
        signIn(){
            axios({
                method: 'post',
                url: 'http://localhost:3000',
                data:{
                    email: this.email,
                    password: this.password
                }
            })
            .then(({ data })=>{
                this.isLogin = true
            })
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