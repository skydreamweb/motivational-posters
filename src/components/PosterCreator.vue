<template>
  <div>
    <div class="box">
      <img :src="imagePoster.src" alt="" />
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
    </div>
  </div>
</template>

<script>
export default {
  name: "PosterCreator",
  data() {
    return {
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
      console.log(img);
        let motivation = {
            title: this.title,
            subtitle: this.subtitle
        }
        const returnedTarget = Object.assign(img, motivation);
        // push image in state to array
        this.images.push(returnedTarget);

        // place array in localstorage
        localStorage.setItem("posters", JSON.stringify(this.images));
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
