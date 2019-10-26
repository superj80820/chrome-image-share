<template>
  <v-container>
    <v-layout text-center wrap>
      <input type="file" @change="handleImage" accept="image/*" ref="image" />
      <v-col align="center" justify="center">
        <v-overlay :value="overlay">
          <v-progress-circular
            indeterminate
            color="primary"
          ></v-progress-circular>
        </v-overlay>
        <img :src="imgSrc" style="max-width: 100%; max-height: 750px" />
        <v-row align="center" justify="center">
          <v-btn color="primary" @click="_clickInputBtn">拍照或上傳圖檔</v-btn>
        </v-row>
        <v-row v-if="!switchInputOrCopy" align="center" justify="center">
          <v-btn color="success" v-clipboard:copy="imgSrc">複製 URL!</v-btn>
          <v-btn color="success" v-clipboard:copy="markdownMsg"
            >複製 markdown!</v-btn
          >
        </v-row>
      </v-col>
    </v-layout>
  </v-container>
</template>

<script>
const axios = require("axios").default;
const FormData = require("form-data");

export default {
  data: () => ({
    imgSrc: "",
    overlay: false
  }),
  computed: {
    switchInputOrCopy: function() {
      return this.imgSrc === "";
    },
    markdownMsg: function() {
      return `![](${this.imgSrc})`;
    }
  },
  mounted() {
    if (this.$route.query.imgurId !== undefined) {
      this.imgSrc = `https://imgur.com/${this.$route.query.imgurId}.jpg`;
    }
  },
  methods: {
    handleImage() {
      let form = new FormData();
      form.append("image", this.$refs.image.files[0]);
      this.overlay = true;
      this._uploadToImgur(form).then(response => {
        this.imgSrc = `https://imgur.com/${response.data.data.id}.jpg`;
        this.$router.push({
          path: "/",
          query: { imgurId: response.data.data.id }
        });
        this.overlay = false;
      });
    },
    _uploadToImgur(form) {
      return axios({
        method: "post",
        url: "https://api.imgur.com/3/image",
        data: form,
        headers: {
          Authorization: "Client-ID e74a559c959567b",
          "Content-Type": "multipart/form-data"
        }
      })
        .then(function(response) {
          return Promise.resolve(response);
        })
        .catch(function(err) {
          console.error(err);
        });
    },
    _clickInputBtn() {
      this.$refs.image.click();
    }
  }
};
</script>

<style>
input[type="file"] {
  display: none;
}
.v-btn {
  margin: 5px;
}
</style>
