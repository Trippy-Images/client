<template>
    <div id="Content">
        <div id="HeroImage"></div>
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
          <b-dropdown-item href="#" v-on:click='logout'>Sign Out</b-dropdown-item>
        </b-nav-item-dropdown>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</div>
    <b-container fluid id="main">

    <LoginRegister v-if="!isLogin" v-on:logged="loggedIn"></LoginRegister>
    <div class="loader" v-if="isLoading">

        <div class="text-center mb-3 d-flex justify-content-between">
      <b-spinner
        variant='primary'
      ></b-spinner>
    </div>
    </div>
    
    <b-row class="text-center justify-content-md-center" >
        <b-col cols="8" id="resultImage" v-if="resultPart">
            <b-img id="resultFinal" v-bind:src="resultPart" fluid-grow alt="Fluid-grow image"></b-img>
            </b-col>
    </b-row>
    <b-row class="text-center justify-content-md-center my-4" v-if="resultPart">
        <b-col cols="4"></b-col>
        <b-col>
            <b-button pill variant="dark" v-on:click="againButton" size="lg">Again</b-button>
        </b-col>
        <b-col>
            <FacebookShareButton v-bind:facebookShare="resultPart"></FaceBookShareButton>
        </b-col>
        <b-col cols="4"></b-col>
    </b-row>
    <b-row class="text-center justify-content-md-center" v-if="isLogin" id="uploadForm">
        <Upload v-on:imageSelected="addImage($event, 0)" v-bind:deleteImage='clearImage'></Upload>
        <Upload v-on:imageSelected="addImage($event, 1)" v-bind:deleteImage='clearImage'></Upload>
    </b-row>
    <b-row class="text-center justify-content-md-center">
        <b-col>
            <b-button class="mt-4" pill variant="dark" v-on:click="ResultButton" v-if="isLogin" size="lg">Result</b-button>
        </b-col>
    </b-row>
</b-container>
    </div>
</template>

<script>
import Upload from './Upload'
import axios from 'axios'
import VueUploadMultipleImage from 'vue-upload-multiple-image'
import LoginRegister from './LoginRegist'
import Swal from 'sweetalert2'

import FacebookShareButton from './FacebookShareButton'

export default {
    name: 'parent',
    components: {
        Upload,
        FacebookShareButton,
        LoginRegister,
    },
    created(){
        if(localStorage.getItem('token')){
            this.isLogin = true
        }else{
            this.isLogin = false
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
            password:'',
            isLoading: false,
            clearImage: false
        }
    },
    methods:{
        ResultButton(){
            this.isLoading = true
            let imageDatas = new FormData()
            imageDatas.append('image1', this.images[0])
            imageDatas.append('image2', this.images[1])

            axios({
                method: 'post',
                url: 'http://localhost:3000/trippy',
                data: imageDatas,
                headers:{
                    token: localStorage.getItem('token'),
                    "Content-Type": "multipart/form-data"
                }
            })
            .then(({ data })=>{
                // console.log(data);
                this.isLoading = false
                this.resultPart = data.result
                this.style = data.style
                this.content = data.content
                this.clearImage = false
            })
            .catch(err=>{
                Swal.fire({
              icon: 'error',
              title: 'Error',
              text: 'All image cannot be empty'
          })
                this.isLoading = false
            })
        },
        againButton(){
            this.resultPart = null
            this.images = []
            this.clearImage = true

        },
        addImage(file, index){
            // console.log(file)
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
            .catch(err=>{
                Swal.fire({
              icon: 'error',
              title: 'Error',
              text: err.response.data.message
          })
            })
        },
        loggedIn(){
            this.isLogin = true
        },
        logout(){
            localStorage.clear()
            this.isLogin =false
            this.resultPart = null
        }
    }
}

</script>

<style>
    .loader {
        position: fixed;
        z-index: 2;
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100vh;
        width: 100vw;
        background-color: rgba(0, 0, 0, 0.5);
        top: 0;
        left: 0;
    }
    #resultImage {
        background-color: rgba(0, 0, 0, 0.5);
        height: 60vh;
        border-radius: 15px;
        padding: 2px;
        overflow: hidden;
    }
    #Content {
        height: 100vh;
    }
    #HeroImage {
background-image: url('../../media/Background.jpg');
        position: fixed; 
    top: 0; 
    left: 0; 
    min-width: 100%;
    min-height: 100%;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    z-index: -100;
    }
    #main {
        margin-top: 15px;
    }
    
    #uploadForm {
        background-color: rgba(0, 0, 0, 0.5);
    }

</style>