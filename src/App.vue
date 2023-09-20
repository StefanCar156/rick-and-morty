<template>
  <div id="app" class="container mx-auto py-8">
    <h1 class="text-2xl font-semibold mb-4 ml-4">
      Rick and Morty Character Gallery
    </h1>
    <CharacterGallery />
  </div>
</template>

<script>
import CharacterGallery from "./components/CharacterGallery.vue"

export default {
  components: {
    CharacterGallery,
  },
}
</script>
