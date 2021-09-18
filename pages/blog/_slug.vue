<template>
  <article>
    <h1>{{ article.title }}</h1>
    <p>{{ article.description }}</p>
    <img :src="article.img" :alt="article.alt" />
    <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>

    <nav>
      <ul>
        <li v-for="link in article.toc" :key="link.id">
          <nuxt-link
            :class="{ h2Heading : link.depth === 2, h3Heading: link.depth === 3 }"
            :to="`#${link.id}`">{{ link.text }}</nuxt-link>
        </li>
      </ul>
    </nav>

    <!-- <nuxt-content :document="article" /> -->

    <author :author="author" />

    <prev-next :prev="prev" :next="next" />
  </article>
</template>
<script>
  export default {
    async asyncData({ $content, params }) {
      const article = await $content('articles', params.slug).fetch()

      const [prev, next] = await $content('articles')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()

      return { article, prev, next }
    },

    data() {
      return {
        author: {
          name: 'Benjamin',
          bio: 'All about Benjamin',
          image: 'https://source.unsplash.com/random'
        }
      }
    },

    methods: {
      formatDate(date) {
        const options = { year: 'numeric', month: 'long', day: 'numeric' }

        return new Date(date).toLocaleString('fr', options)
      }
    }
  }
</script>

<style>
  .nuxt-content h2 {
    font-weight: bold;
    font-size: 28px;
  }
  .nuxt-content h3 {
    font-weight: bold;
    font-size: 22px;
  }
  .nuxt-content p {
    margin-bottom: 20px;
  }

  .icon.icon-link {
    background-image: url('~assets/icons/icon-hashtag.svg');
    display: inline-block;
    width: 20px;
    height: 20px;
    background-size: 20px 20px;
  }

  .h2Heading {
    padding-left: 5px;
  }

  .h3Heading {
    margin-left: 5px;
    padding-bottom: 5px;
  }
</style>