<template>
  <div>
    <button @click.prevent="shareToFacebook">Share To Facebook</button>
  </div>
</template>

<script>
export default {
  methods: {
    shareToFacebook() {
      FB.ui(
        {
          method: "share",
          href:
            "http://advice.uk.match.com/quizzes/which-european-are-you-destined-date"
        },
        function(response) {}
      );
    }
  }
};
</script>

<style scoped>
</style>