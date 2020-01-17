<template>
  <b-row class="text-center justify-content-md-center">
    <b-col cols="4" id="loginPart" class="align-self-center">
      <b-row class="text-center mb-3" id="loginForm">
        <b-col sm="4" align-self="center">
            <label :for="`email-${email}`">Email:</label>
        </b-col>
        <b-col sm="8">
            <b-form-input :id="`email-${email}`" type="email" v-model="email"></b-form-input>
        </b-col>
      </b-row>
      <b-row class="text-center mb-3">
          <b-col sm="4" align-self="center">
            <label :for="`password-${password}`">Password:</label>
        </b-col>
        <b-col sm="8">
            <b-form-input :id="`password${password}`" type="password" v-model="password"></b-form-input>
        </b-col>
      </b-row>
      <b-row class="text-center justify-content-md-center p-3">
        <b-button variant="success" class="mx-2" v-on:click="login">Login</b-button>
        <b-button variant="primary" class="mx-2" v-on:click="register">Register</b-button>
      </b-row>
    </b-col>
  </b-row>
</template>

<script>
import axios from "axios";
import Swal from 'sweetalert2'

export default {
  name: "LoginRegister",
  data() {
    return {
      email: "",
      password: ""
    };
  },
  methods: {
    register() {
      axios({
        method: "post",
        url: "https://trippy-server.prograami.com/register",
        data: {
          email: this.email,
          password: this.password
        }
      })
      .then(({ data })=>{
          localStorage.setItem('token', data.token)
          this.$emit('logged', true)
      })
      .catch(err=>{
          Swal.fire({
              icon: 'error',
              title: 'Error',
              text: err.response.data.message
          })
        //   console.log(err.response.data.message)
      })
    },
    login(){
        axios({
            method:'post',
            url: "https://trippy-server.prograami.com/login",
            data:{
                email: this.email,
                password: this.password
            }
        })
        .then(({ data })=>{
            localStorage.setItem('token', data.token)
            this.$emit('logged', true)
        })
        .catch(err=>{
            Swal.fire({
              icon: 'error',
              title: 'Error',
              text: err.response.data.message
          })
        })
    }
  }
};
</script>

<style scoped>
#loginForm {
  margin-top: 8%;
}
#loginPart {
  background-color: rgba(0, 0, 0, 0.5);
  height: 30vh;
  border-radius: 15px;
  color: white;
}
label{
    font-size: 23px;
}
</style>