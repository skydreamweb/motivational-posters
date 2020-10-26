<template>
  <q-page class="flex flex-center">
  
    <input type="file" @change="fileUpload">
    <img
      v-for="img in images"
      :key="img.id"
      :src="img.src"
      @click="selectImageHandler(img)"
    >
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      model: null,
      images: []
    };
  },
methods: {
    fileUpload(event) {
      const reader = new FileReader()
      reader.addEventListener('load', () => {
        // create unique name
        let id = 0
        let images = JSON.parse(localStorage.getItem('images'))
        if (images && images.length > 0) {
          id = Number(images[images.length - 1].id) + 1
        }
        let name = 'img-' + id

        // push image in array
        this.images.push({ id, name, src: reader.result })

        // put array in localstorage
        localStorage.setItem('images', JSON.stringify(this.images))
      })
      reader.readAsDataURL(event.target.files[0])
    },
    // get info about image on click
    selectImageHandler(img) {
      console.log(img)
    },
  },
  created() {
    // load data from localstorage after reload component
    let images = []
    images = JSON.parse(localStorage.getItem('images'))
      ? JSON.parse(localStorage.getItem('images'))
      : []

    if (images.length > 0) {
      images.forEach(image => {
        this.images.push(image)
      })
    }
  },
};
</script>
