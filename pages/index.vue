<template>
  <div>
    <add-search-input />
    <ul v-for="article in articles" :key="article.slug">
      <li>
        <nuxt-link :to="{ name: 'blog-slug', params: { slug: article.slug} }">{{ article.title}}</nuxt-link>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const articles = await $content('articles')
      .only(['title', 'description', 'img', 'slug', 'author'])
      .sortBy('createdAt', 'asc')
      .fetch()

    return {articles}
  }
}
</script>
