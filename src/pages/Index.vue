<template>
  <q-page class="flex">
    <div class="poster">
    <poster-creator :image-poster="this.singleImage"></poster-creator>
    </div>
    <div class="upload">
    <h3>Please upload your image</h3>
    <input type="file" @change="fileUpload" />
    </div>
    <div class="container row">
      <div v-for="img in images" :key="img.id" class="images col-3">
        <img :src="img.src" @click="selectImageHandler(img)" />
        <div class="button">
        <button @click="deleteImage(img)" class="" separator >Delete</button>
        <button @click="addImage(img)">Create Poster</button>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import PosterCreator from '../components/PosterCreator'
export default {
  name: "PageIndex",
  data() {
    return {
      images: [],
      singleImage: null
    };
  },
  components: {
    PosterCreator
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
    addImage(img){
      console.log(img);
       this.singleImage = img;
    },
    // get info about image on click
    selectImageHandler(img) {
      console.log(img);
    },

    // delete image
    deleteImage(img) {
      // colect "images" key from localstorage
      let imagesToDelete = JSON.parse(localStorage.getItem("images"));

      // filter array & remove clicked
      var filteredImages = imagesToDelete.filter(
        element => element.id != img.id
      );

      // place new filtered array to state
      this.images = filteredImages;

      // place filtered array to localstorage
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

.buttons {
  display: inline;
}
.poster {
  margin: 0 auto;
}
.container {
  justify-content: center;
}
.upload {
  margin: 3rem auto;
}
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
