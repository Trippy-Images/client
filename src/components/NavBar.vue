<template>
    <div>
  <b-navbar type="light" id="clientNavbar">
    <b-navbar-brand href="#" id="WebName">
      <img src="https://placekitten.com/g/30/30" class="d-inline-block align-top" alt="Kitten">
      Trippy Image
    </b-navbar-brand>
  </b-navbar>
</div>
</template>

<script>
export default {
    name: 'NavBar'
}
</script>

<style scoped>
    #clientNavbar {
        background: black;
        
    }
    #WebName {
        color: white;
    }
</style>