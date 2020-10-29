<template>
  <div class="bg-white pt-3 pb-16 px-4 overflow-hidden sm:px-6">
    <div class="relative px-1 sm:px-6 lg:px-8">
      <div class="text-lg max-w-prose mx-auto mb-6">
        <h2
          class="text-3xl text-center font-extrabold tracking-tight text-gray-900 sm:text-4xl sm:leading-10 mt-2 mb-8 leading-8"
        >
          {{ page.title }}
        </h2>
        <p class="text-gray-500">
          {{ page.description }}
        </p>
      </div>
      <div class="prose prose-lg text-gray-500 mx-auto">
        <nuxt-content :document="page" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    const slug = params.slug || 'problem-statement'
    const page = await $content(slug)
      .fetch()
      .catch(() => {
        error({ statusCode: 404, message: 'Page not found' })
      })

    return {
      page,
    }
  },
  head() {
    return {
      title: this.page.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.page.description,
        },
        // Open Graph
        { hid: 'og:title', property: 'og:title', content: this.page.title },
        {
          hid: 'og:description',
          property: 'og:description',
          content: this.page.description,
        },
        // Twitter Card
        {
          hid: 'twitter:title',
          name: 'twitter:title',
          content: this.page.title,
        },
        {
          hid: 'twitter:description',
          name: 'twitter:description',
          content: this.page.description,
        },
      ],
    }
  },
}
</script>
