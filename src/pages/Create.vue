<template>
  <div class="albums">
    <h1>Create albums</h1>
    <div>
      <input type="text" placeholder="Enter album name" v-model="albumName" />
      <button @click="createAlbumHandler">Create album</button>
    </div>
    <q-list class="flex" bordered padding>
      <q-separator spaced />

      <template v-if="albums.length > 0">
        <q-item v-ripple v-for="album in albums" :key="album.albumId">
          <q-item-section side top>
            <button @click="deleteAlbumHandler(album.albumId)">Delete</button>
          </q-item-section>

          <q-item-section>
            <q-item-label>{{ album.name }}</q-item-label>
          </q-item-section>
        </q-item></template
      >
      <q-item v-else>No albums found.</q-item>

      <q-separator spaced />
    </q-list>
    <q-separator spaced />
    <list-posters
      :checkedIds="
        selectedPosters.length > 0
          ? selectedPosters.map(poster => poster.posterId)
          : []
      "
      v-on:posterChecked="posterCheckedHandler"
    ></list-posters>
  </div>
</template>

<script>
import ListPosters from "../components/ListPosters";
export default {
  name: "Create",
  components: {
    ListPosters
  },
  data() {
    return {
      albums: [],
      albumName: "",
      selectedPosters: []
    };
  },
  methods: {
    posterCheckedHandler(checkedPosters) {
      this.selectedPosters = checkedPosters;
      console.log(checkedPosters);
    },
    createAlbumHandler() {
      if (this.selectedPosters.length === 0) {
        alert("Please select posters first!");
        return;
      }
      // set unique album id
      let albumId = 0;
      let albums = JSON.parse(localStorage.getItem("albums"))
        ? JSON.parse(localStorage.getItem("albums"))
        : [];
      if (albums && albums.length > 0) {
        albumId = Number(albums[albums.length - 1].albumId) + 1;
      }

      // create new album
      const album = {
        albumId,
        name: this.albumName,
        posters: this.selectedPosters
      };

      this.albums.push(album);

      // update in localstorage
      localStorage.removeItem("albums");
      localStorage.setItem("albums", JSON.stringify(this.albums));

      // reset data
      this.selectedPosters = [];
      this.albumName = "";
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
      this.albums = albums;
    }
  }
};
</script>

<style lang="scss" scoped>
.albums {
  display: block;
  margin: 0 auto;
  text-align: center;
}
</style>
