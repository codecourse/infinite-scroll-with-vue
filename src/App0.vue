<template>
  <div id="app" class="container mx-auto pt-12">
    <div
      class="bg-gray-200 shadow-xl p-8 rounded-lg mb-12"
      v-for="article in articles" :key="article.id"
    >
      <h1 class="text-gray-800 text-lg font-semibold mb-2">
        {{ article.title }} (#{{ article.id }})
      </h1>
      <p class="text-gray-600 text-md">
        {{ article.description }}
      </p>
    </div>

    <div v-if="articles.length" v-observe-visibility="handleScrolledToBottom"></div>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    data () {
      return {
        articles: [],
        page: 1,
        lastPage: 1
      }
    },

    methods: {
      async fetch () {
        let articles = await axios.get(`http://localhost:8000/api/articles?page=${this.page}`)

        this.articles.push(...articles.data.data)
        this.lastPage = articles.data.meta.last_page
      },

      handleScrolledToBottom (isVisible) {
        if (!isVisible) { return }
        if (this.page >= this.lastPage) { return }

        this.page++

        this.fetch()
      }
    },

    mounted () {
      this.fetch()
    }
  }
</script>

<style src="@/assets/app.css"></style>
