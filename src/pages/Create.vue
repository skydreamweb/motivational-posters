<template>
  <div class="albums">
    <h1>Create albums</h1>
    <div>
      <input type="text" v-model="albumName" />
      <button @click="createAlbumHandler">Create album</button>
    </div>
    <div>
      <div class="row inline">
        <button @click="deleteAlbumHandler">Delete album</button>
      </div>
      <div>
        <button @click="addPostersHandler">Add posters to album</button>
      </div>
    </div>
    <q-list class="flex" bordered padding>
      <q-separator spaced />

      <q-item v-ripple v-for="album in albums" :key="album.albumId">
        <q-item-section side top>
          <q-checkbox
            indeterminate-value="false"
            v-model="check"
            :val="album"
            :check="album"
            :checked="checked"
          />
        </q-item-section>

        <q-item-section>
          <q-item-label>{{ album.name }}</q-item-label>
        </q-item-section>
      </q-item>

      <q-separator spaced />
    </q-list>
    <q-separator spaced />
    <list-posters v-on:posterChecked="posterCheckedHandler"></list-posters>
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
      id: "",
      val: "",
      albums: [],
      albumName: "",
      check: [],
      checked: false,
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
        alert("Please select posters!");
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
      this.selectedPosters = []
      
    },
    addPostersHandler() {
      console.log(this.selectedPosters);
    },
    posterCheck(img) {
      let vm = this;
      console.log("this is img", img);
      let filterCh = this.postersChecked;
      let compare = img;
      filterCh.forEach(image => {
        console.log("this is image ", image);
        if (image == compare) {
          return;
        } else {
          console.log("Nema ga");
          console.log(img);
          vm.postersChecked.push(img);
        }
      });

      console.log(this.postersChecked);
    },

    deleteAlbumHandler() {
      // colect "albums" key from localstorage
      let albumsToDelete = JSON.parse(localStorage.getItem("albums"));
      let toDelete = this.check;

      // filter to arrays of objects
      const results = albumsToDelete.filter(
        ({ id: id1 }) => !toDelete.some(({ id: id2 }) => id2 === id1)
      );
      // reset checked albums
      this.check = [];
      this.albums = results;

      localStorage.setItem("albums", JSON.stringify(results));
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
