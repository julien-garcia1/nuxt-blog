<template>
  <div>
    <h1>Author: {{ articles[0].name }}</h1>
    <p>Bio: {{ articles[0].bio }}</p>
    <h3>Here are a list of articles by {{ articles[0].name }}:</h3>
    <ul>
      <li v-for="article in articles" :key="article.slug">
        <NuxtLink :to="{ name: 'blog-slug', params: { slug: article.slug } }">
          <img :src="article.img" :alt="article.alt" />
          <div>
            <h2>{{ article.title }}</h2>
            <p>{{ article.description }}</p>
            <p>{{ formatDate(article.updatedAt) }}</p>
          </div>
        </NuxtLink>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  async asyncData({ $content, params} ) {
    const articles = await $content('articles', params.slug)

      .without('body')
      .sortBy('createdAt', 'asc')
      .fetch()

    return { articles }
  },

  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('fr', options)
    }
  }
}
</script>