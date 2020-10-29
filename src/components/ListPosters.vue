<template>
  <div>
    <q-list class="flex" bordered padding>
      <q-separator spaced />

      <q-item v-ripple v-for="album in albums" :key="album.id">
        <q-item-section side top>
          <img :src="imagePoster.src" alt="" />
        </q-item-section>

        <q-item-section>
          <q-item-label>{{ album.name }}</q-item-label>
        </q-item-section>
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
      src: ""
    };
  },
};
</script>

<style lang="scss" scoped></style>
