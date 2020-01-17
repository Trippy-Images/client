<template>
  <div>
    <b-button pill variant="primary" size="lg" @click.prevent="shareToFacebook">Share To Facebook</b-button>
  </div>
</template>

<script>
export default {
  props: ['facebookShare'],
  methods: {
    shareToFacebook() {
      FB.ui(
        {
          method: "share",
          href: this.facebookShare
        },
        function(response) {}
      );
    }
  }
};
</script>

<style scoped>
</style>