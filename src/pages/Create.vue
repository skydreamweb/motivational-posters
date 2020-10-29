<template>
  <div class="albums">
    <h1>Create albums</h1>
    <div>
      <input type="text" v-model="albumName" />
      <button @click="createAlbum">Create album</button>
    </div>
    <div>
      <div class="row inline">
        <button @click="deleteAlbums">Delete album</button>
      </div>
      <div>
        <button @click="addPoster">Add poster to album</button>
      </div>
    </div>
    <q-list class="flex" bordered padding>
      <q-separator spaced />

      <q-item v-ripple v-for="album in albums" :key="album.id">
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
    <list-posters v-on:postersChecked="hello"></list-posters>
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
      selectedPosters: null
    };
  },
  methods: {
    hello(postersCheck) {
      this.selectedPosters = postersCheck;
      console.log(postersCheck);
    },
    addPoster() {
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
    createAlbum() {
      console.log(this.postersChecked);
      let id = 0;
      let allAlbums = JSON.parse(localStorage.getItem("albums"));

      // random number for id
      if (allAlbums && allAlbums.length > 0) {
        id = Number(allAlbums[allAlbums.length - 1].id) + 1;
      }
      let name = this.albumName;
      let checked = this.checked;
      this.albums.push({ id, name, checked });
      console.log(this.albums);
      localStorage.setItem("albums", JSON.stringify(this.albums));
    },

    deleteAlbums() {
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
  created() {
    let albums = [];
    albums = JSON.parse(localStorage.getItem("albums"))
      ? JSON.parse(localStorage.getItem("albums"))
      : [];

    if (albums.length > 0) {
      albums.forEach(album => {
        this.albums.push(album);
      });
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
