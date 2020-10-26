<template>
  <q-page class="flex flex-center">
   
      <q-file outlined v-model="model">
        <template v-slot:prepend>
          <q-icon name="attach_file" />
        </template>
      </q-file>

      <q-img
      v-for="img in images"
      :key="img.id"
      :src="img.src"
      @click="selectImageHandler(img)"
      >
      </q-img>

  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data(){
    return {
      model: null
    }
  }
}
</script>
