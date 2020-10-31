<template>
  <div>
    <q-list class="flex" bordered padding>
      <q-item v-ripple v-if="posters.length > 0">
        <div class="container row">
          <div
            v-for="poster in posters"
            :key="poster.posterId"
            class="images col-3 poster-list"
          >
            <q-checkbox
              indeterminate-value="false"
              v-model="checkedPosterIds"
              :val="poster.posterId"
            />
            <div class="inner">
              <img :src="poster.imgSrc" />
              <div class="lable">
                <label>{{ poster.title }}</label>
                <label>{{ poster.subtitle }}</label>
              </div>
            </div>
            <div class="text">
              <div class="button">
                <button class="" separator>
                  Delete
                </button>
              </div>
            </div>
          </div>
        </div>
      </q-item>
      <q-item v-else>No posters found</q-item>

      <q-separator spaced />
    </q-list>
  </div>
</template>

<script>
export default {
  name: "Albums",
  data() {
    return {
      posters: [],
      checkedPosterIds: []
    };
  },
  mounted() {
    let posters = JSON.parse(localStorage.getItem("posters"))
      ? JSON.parse(localStorage.getItem("posters"))
      : [];
    if (posters.length > 0) {
      posters.forEach(poster => {
        this.posters = posters;
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
