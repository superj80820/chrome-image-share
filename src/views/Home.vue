<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <ShareImage />
  </div>
</template>

<script>
// @ is an alias to /src
import ShareImage from "@/components/ShareImage.vue";

export default {
  name: "home",
  components: {
    ShareImage
  }
};
</script>
