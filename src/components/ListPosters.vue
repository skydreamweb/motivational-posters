<template>
  <div>
    <q-list class="flex" bordered padding>
      <q-item v-ripple>
        <div class="container row">
          <div
            v-for="img in images"
            :key="img.id"
            class="images col-3 poster-list"
          >
            <q-checkbox
              indeterminate-value="false"
              v-model="checkedIds"
              :checked="checked"
              :val="img.id"
              :check="img"
              @click.native="status()"
            />
            <div class="inner">
              <img :src="img.src" @click="selectImageHandler(img)" />
              <div class="lable">
                <label for="checkbox">{{ img.title }}</label>
                <label for="checkbox">{{ img.subtitle }}</label>
              </div>
            </div>
            <div class="text">
              <div class="button">
                <button @click="deleteImage(img)" class="" separator>
                  Delete
                </button>
                
              </div>
            </div>
          </div>
        </div>
      </q-item>

      <q-separator spaced />
    </q-list>
  </div>
</template>

<script>
export default {
  name: "ListPosters",
  data() {
    return {
      title: "",
      subtitle: "",
      images: [],
      src: "",
      checked: false,
      checkedIds: [],
    };
  },
  methods: {
   status() {
      let selectedImages = this.images.filter(image =>
        this.checkedIds.includes(image.id)
      );
      console.log(selectedImages);
      this.$emit("postersChecked", selectedImages);
    },
    // get info about image on click
    selectImageHandler(img) {
      console.log(img);
    },
    addPoster(img) {
      console.log(img);
      let motivation = {
        title: this.title,
        subtitle: this.subtitle
      };
      const returnedTarget = Object.assign(img, motivation);
      // push image in state to array
      this.images.push(returnedTarget);

      // place array in localstorage
      localStorage.setItem("posters", JSON.stringify(this.images));
    },
    deleteImage(img) {
      // colect "images" key from localstorage
      let imagesToDelete = JSON.parse(localStorage.getItem("posters"));

      // filter array & remove clicked
      var filteredImages = imagesToDelete.filter(
        element => element.id != img.id
      );

      // place new filtered array to state
      this.images = filteredImages;

      // place filtered array to localstorage
      localStorage.setItem("posters", JSON.stringify(filteredImages));
    }
  },
  created() {
    let posters = [];
    posters = JSON.parse(localStorage.getItem("posters"))
      ? JSON.parse(localStorage.getItem("posters"))
      : [];
    if (posters.length > 0) {
      posters.forEach(image => {
        this.images.push(image);
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.poster-list {
  border: 1px solid black;
  padding: 5px;
}
.poster-list img {
  width: 100%;
  height: auto;
}
.lable {
  position: absolute;
  background-color: tomato;
  width: 24%;
  margin-top: -4rem;
  height: 50px;
}
.lable {
  position: absolute;
  display: grid;
}
</style>
