<template>
  <div id="app" class="container mx-auto pt-12">
    <InfiniteScroll :items="articles" @refetch="fetch">
      <template v-slot:item="{ item }">
        <div class="bg-gray-200 shadow-xl p-8 rounded-lg mb-12">
          <h1 class="text-gray-800 text-lg font-semibold mb-2">
            {{ item.title }} (#{{ item.id }})
          </h1>
          <p class="text-gray-600 text-md">
            {{ item.description }}
          </p>
        </div>
      </template>
    </InfiniteScroll>
  </div>
</template>

<script>
  import axios from 'axios'
  import InfiniteScroll from '@/components/InfiniteScroll'

  export default {
    components: {
      InfiniteScroll
    },

    data () {
      return {
        articles: [],
        lastPage: 1
      }
    },

    methods: {
      async fetch (page) {
        if (page > this.lastPage) { return }

        let articles = await axios.get(`http://localhost:8000/api/articles?page=${page}`)

        this.articles.push(...articles.data.data)
        this.lastPage = articles.data.meta.last_page
      },
    },

    mounted () {
      this.fetch(1)
    }
  }
</script>

<style src="@/assets/app.css"></style>
