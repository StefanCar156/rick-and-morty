<template>
  <div>
    <div
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4"
    >
      <!-- Cards -->
      <div
        v-for="character in characters"
        :key="character.id"
        class="bg-white shadow rounded-lg p-4"
      >
        <img
          :src="character.image"
          :alt="character.name"
          class="w-full h-auto mb-2"
        />
        <h3 class="text-lg font-semibold">{{ character.name }}</h3>
        <p>Status: {{ character.status }}</p>
        <p>Created: {{ getFormattedDate(character.created) }}</p>
        <p>Location: {{ character.location.name }}</p>
      </div>
    </div>

    <!-- Loading spinner -->
    <div
      class="animate-spin m-auto h-8 w-8 rounded-full border-4 border-solid border-gray-400 border-r-transparent align-[-0.125em] motion-reduce:animate-[spin_1.5s_linear_infinite]"
      role="status"
    ></div>
  </div>
</template>

<script>
import moment from "moment"

export default {
  data() {
    return {
      characters: [],
      page: 1,
      loading: false,
    }
  },
  mounted() {
    this.loadCharacters()
    this.setupInfiniteScroll()
  },
  methods: {
    async loadCharacters() {
      try {
        this.loading = true
        const response = await this.fetchCharacters(this.page)
        this.characters = [...this.characters, ...response.data.results]
        this.page++
      } catch (error) {
        console.error("Error loading characters:", error)
      } finally {
        this.loading = false
      }
    },
    async fetchCharacters(page) {
      const apiUrl = `https://rickandmortyapi.com/api/character/?page=${page}`
      return await this.$axios.get(apiUrl)
    },
    setupInfiniteScroll() {
      const self = this
      window.addEventListener("scroll", function () {
        if (
          window.innerHeight + window.scrollY >=
            document.documentElement.offsetHeight - 100 &&
          !self.loading
        ) {
          self.loadCharacters()
        }
      })
    },
    getFormattedDate(date) {
      return moment(date).format("DD.MM.YYYY. HH:mm")
    },
  },
}
</script>
