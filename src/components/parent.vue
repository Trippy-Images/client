<template>
    <div id="Content">
        <FacebookShareButton></FacebookShareButton>
    <div>
  <b-navbar toggleable="lg" type="dark" variant="info">
    <b-navbar-brand href="#">Trippy Image</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        <b-nav-item-dropdown right v-if="isLogin">
          <!-- Using 'button-content' slot -->
          <template v-slot:button-content>
            <em>User</em>
          </template>
          <b-dropdown-item href="#">Profile</b-dropdown-item>
          <b-dropdown-item href="#">Sign Out</b-dropdown-item>
        </b-nav-item-dropdown>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</div>
    <b-container fluid id="main">
    <b-row class="text-center justify-content-md-center">
        <b-col cols="8" id="loginPart" v-if="!isLogin" class="align-self-center">
            <b-row class="text-center justify-content-md-center" id="loginForm">
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
    <b-row class="text-center justify-content-md-center" v-if="isLogin" id="uploadForm">
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
import Upload from './Upload'
import axios from 'axios'

import FacebookShareButton from './FacebookShareButton'

export default {
    name: 'parent',
    components: {
        Upload,
        FacebookShareButton
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
    #main {
        margin-top: 15px;
    }
    #loginForm {
        margin-top: 8%;
    }
    #uploadForm {
        background-color: rgba(0, 0, 0, 0.5);
    }
</style>