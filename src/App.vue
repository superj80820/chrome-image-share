<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>簡單傳圖</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        text
        href="https://github.com/superj80820/chrome-image-share"
        target="_blank"
      >
        <span class="mr-2">使用方法</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <ShareImage />
    </v-content>
  </v-app>
</template>

<script>
import ShareImage from "./components/ShareImage";

export default {
  name: "App",
  components: {
    ShareImage
  },
  data: () => ({
    //
  })
};
</script>
