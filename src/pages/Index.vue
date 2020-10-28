<template>
  <q-page class="flex">
    <h3>Please upload your image</h3>
    <input type="file" @change="fileUpload" />
    <div class="container row">
      <div v-for="img in images" :key="img.id" class="images col-3">
        <img :src="img.src" @click="selectImageHandler(img)" />
        <button @click="deleteImage(img)" class="button">Delete</button>
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      images: []
    };
  },
  methods: {
    fileUpload(event) {
      const reader = new FileReader();
      reader.addEventListener("load", () => {
        // Create unique name
        let id = 0;
        let images = JSON.parse(localStorage.getItem("images"));

        // random number for id
        if (images && images.length > 0) {
          id = Number(images[images.length - 1].id) + 1;
        }
        let name = "img-" + id;

        // push image in state to array
        this.images.push({ id, name, src: reader.result });

        // place array in localstorage
        localStorage.setItem("images", JSON.stringify(this.images));
      });
      reader.readAsDataURL(event.target.files[0]);
    },

    // get info about image on click
    selectImageHandler(img) {
      console.log(img);
    },

    // delete image
    deleteImage(img) {
      let imagesToDelete = JSON.parse(localStorage.getItem("images"));

      var filteredImages = imagesToDelete.filter(e => e.id != img.id);
      this.images = filteredImages;

      localStorage.setItem("images", JSON.stringify(filteredImages));
    }
  },
  created() {
    // load data from localstorage after reload component
    let images = [];
    images = JSON.parse(localStorage.getItem("images"))
      ? JSON.parse(localStorage.getItem("images"))
      : [];
    if (images.length > 0) {
      images.forEach(image => {
        this.images.push(image);
      });
    }
  }
};
</script>
<style scoped>
.images {
  display: block;
  border: 1px solid black;
  padding: 15px;
  position: relative;
  height: auto;
}
.q-page {
  max-width: 1100px;
  margin: 0 auto;
}
img {
  width: 100%;
}
.button {
  position: absolute;
  z-index: 999;
  left: 0;
  bottom: 0;
}
</style>
