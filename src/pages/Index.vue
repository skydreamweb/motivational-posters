<template>
  <q-page class="flex">
    <div class="poster">
      <poster-creator :image-poster="singleImage"></poster-creator>
    </div>
    <div class="upload">
      <h3>Please upload your image</h3>

      <input type="file" @change="fileUpload" />
    </div>

    <div class="container row">
      <div v-for="img in images" :key="img.imageId" class="images col-3">
        <img :src="img.src" @click="selectImageHandler(img)" />
        <div class="button">
          <q-btn
            class="q-mx-xs"
            @click="deleteImageHandler(img.imageId)"
            separator
            rounded
            color="red"
            icon-right="delete"
            label="Delete"
          />
          <q-btn
            class="q-mx-xs"
            color="secondary"
            icon-right="add"
            separator
            rounded
            label="Add"
            @click="addImage(img)"
          />
        </div>
      </div>
    </div>
    <q-separator></q-separator>
  </q-page>
</template>

<script>
// comment
import PosterCreator from "../components/PosterCreator";
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
        // Create unique name and id
        let imageId = 0;
        let images = JSON.parse(this.$q.localStorage.getItem("images"));

        // random number for id
        if (images && images.length > 0) {
          imgageId = Number(images[images.length - 1].imageId) + 1;
        }

        // push image in state to array
        this.images.push({ imageId, src: reader.result });

        // place array in localstorage
        this.$q.localStorage.set("images", JSON.stringify(this.images));
      });
      reader.readAsDataURL(event.target.files[0]);
    },
    addImage(img) {
      console.log(img);
      this.singleImage = img;
    },
    // get info about image on click
    selectImageHandler(img) {
      console.log(img);
    },

    // delete image
    deleteImageHandler(imageId) {
      if (confirm("Are you sure that you want to delete this image?")) {
        // update images
        this.images = this.images.filter(image => image.imageId !== imageId);

        // update localstorage
        this.$q.localStorage.remove("images");
        this.$q.localStorage.set("images", JSON.stringify(this.images));
      }
    }
  },
  mounted() {
    // load data from localstorage after reload component
    let images = JSON.parse(localStorage.getItem("images"))
      ? JSON.parse(localStorage.getItem("images"))
      : [];
    if (images.length > 0) {
      this.images = images;
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
  display: flex;
  height: auto;
  align-items: center;
  border-radius: 40px;
  border: 1px solid black;
  padding: 15px;
  position: relative;
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
  left: 25px;
  bottom: 10px;
}
</style>
