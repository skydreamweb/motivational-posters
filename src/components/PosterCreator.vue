<template>
  <div>
    <h1>Create poster</h1>
    <div class="box">
      <img :src="imagePoster ? imagePoster.src : ''" alt="" />
    </div>
    <div class="lable">
      <label for="checkbox">{{ title }}</label>
      <label for="checkbox">{{ subtitle }}</label>
    </div>

    <div class="text">
      <input type="text" v-model="title" placeholder="Please enter title" />
      <input
        type="text"
        name="subtitle"
        placeholder="Please enter subtitle"
        v-model="subtitle"
      />
      <button @click="savePoster(imagePoster)">Save poster</button>
      <q-dialog v-model="noActive">
        <q-card>
          <q-card-section>
            <div class="text-h6">Done</div>
          </q-card-section>

          <q-card-section class="q-pt-none">
            Poster has been created!
          </q-card-section>

          <q-card-actions align="right">
            <q-btn flat label="OK" color="primary" v-close-popup />
          </q-card-actions>
        </q-card>
      </q-dialog>
    </div>
  </div>
</template>

<script>
export default {
  name: "PosterCreator",
  data() {
    return {
      noActive: false,
      title: "",
      subtitle: "",
      images: [],
      src: ""
    };
  },
  props: {
    imagePoster: {
      type: Object,
      default: () => {
        return {};
      }
    }
  },
  methods: {
    savePoster(img) {
      if (!img) {
        return alert("Please select image first!");
      }
      // set unique poster id
      let posterId = 0;
      let posters = JSON.parse(localStorage.getItem("posters"))
        ? JSON.parse(localStorage.getItem("posters"))
        : [];

      if (posters && posters.length > 0) {
        posterId = Number(posters[posters.length - 1].posterId) + 1;
      }
      console.log(img);
      const poster = {
        posterId,
        title: this.title,
        subtitle: this.subtitle,
        imageId: img.imageId,
        imgSrc: img.src
      };

      posters.push(poster);

      // save posters in localStorage
      localStorage.setItem("posters", JSON.stringify(posters));

      this.noActive = true;

      // reset data
      this.title = "";
      this.subtitle = "";
      this.images = [];

      // go to create album page
      localStorage.setItem(router.push("/create"));
    }
  }
};
</script>

<style lang="scss" scoped>
.text {
  margin-top: 7rem;
}
.box img {
  width: 596px;
  height: 400px;
}
.lable {
  width: 100%;
  height: 100%;
  position: absolute;
  border: 2px solid tomato;
  background-color: tomato;
  width: 600px;
  height: 100px;
  margin-top: -100px;
  display: grid;
  margin: 0 auto;
  text-align: center;
  font-size: 1.5rem;
}
.box {
  width: 600px;
  height: 400px;
  border: 2px solid black;
}
.lable {
  position: absolute;
}
</style>
