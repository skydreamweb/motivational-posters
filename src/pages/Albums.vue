<template>
  <div>
    <q-list class="flex" bordered padding>
      <q-item v-ripple v-if="albums.length > 0">
        <div class="container row">
          <div
            v-for="album in albums"
            :key="album.albumId"
            class="albums col-3 albums-list"
          >
            
            <div class="inner">
              <div>
                <h3>Album name: {{ album.name }}</h3>
              </div>
            </div>

            <!-- Posters -->
            <h3>Posters list</h3>
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
                      @click.native="checkHandler()"
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
                        <button
                          @click="deletePosterHandler(poster.posterId)"
                          class=""
                          separator
                        >
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
            <!-- End posters -->

            <div class="text" >
              <div class="button">
                <button class="" separator @click="deleteAlbumHandler(album.albumId)">
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
      albums: [],
      posters: [],
      checkedPosterIds: []
    };
  },
  methods: {
    checkHandler() {
      console.log(this.checkedPosterIds);
      let selectedImages = this.posters.filter(poster =>
        this.checkedPosterIds.includes(poster.posterId)
      );
      // send all checked posters
      this.$emit("posterChecked", selectedImages);
    },
    deletePosterHandler(posterId) {
      // remove poster
      this.posters = this.posters.filter(
        poster => poster.posterId !== posterId
      );
      // save change to local storage
      localStorage.removeItem("posters");
      localStorage.setItem("posters", JSON.stringify(this.posters));
    },
        deleteAlbumHandler(albumId) {
      if (confirm("Are you sure that you want to delete selected album/s?")) {
        // remove selected albums
        this.albums = this.albums.filter(album => album.albumId !== albumId);

        // update in localstorage
        localStorage.removeItem("albums");
        localStorage.setItem("albums", JSON.stringify(this.albums));
      }
    }
  },
  mounted() {
    let albums = JSON.parse(localStorage.getItem("albums"))
      ? JSON.parse(localStorage.getItem("albums"))
      : [];
    if (albums.length > 0) {
      albums.forEach(album => {
        this.albums = albums;
      });
    }
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
.albums {
  width: 100%;
  height: auto;
  border: 1px solid black;
}
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
