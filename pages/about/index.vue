<template>
  <div id="container" class="bg-white pt-3 pb-16 px-4 overflow-hidden sm:px-6">
    <div class="relative max-w-2xl mx-auto">
      <div class="text-center">
        <h2
          class="text-3xl leading-9 font-extrabold tracking-tight text-gray-900 sm:text-4xl sm:leading-10"
        >
          About
        </h2>
      </div>
      <div class="p-4 text-lg sm:text-center text-left text-gray-500">
        Post-Here.com uses a BERT classification model and ~4,000,000 posts
        mined from ~5,000 subreddits. The front end uses Nuxt and tailwind UI
        and is deployed as a static site to Cloudflare using Cloudflare workers.
        The model was converted to ONNX and uses Azure Functions and onnxruntime
        to run inferences. Please refer to the following repos and blog posts
        below for more information, and code examples.
      </div>
      <div class="text-lg sm:text-center text-gray-500">
        By
        <a
          href="https://spentaur.com"
          class="text-blue-400 mt-2 font-bold hover:underline hover:text-blue-600"
          >Spencer Adams</a
        >
      </div>
      <div class="mt-6 leading-relaxed border-t-2 border-gray-100 pt-6">
        <span class="font-bold text-xl">Repos</span>
        <ul class="ml-4">
          <li>
            <a
              class="text-blue-400 text-lg font-bold hover:text-blue-600 hover:underline"
              href="https://github.com/spentaur/post-here-fe"
              >Front End</a
            >
          </li>
          <li>
            <a
              class="text-blue-400 text-lg font-bold hover:text-blue-600 hover:underline"
              href="https://github.com/spentaur/post-here-function"
              >Model Deployment</a
            >
          </li>
        </ul>
      </div>
      <div>
        <div
          v-for="article in articles"
          :key="article.title"
          class="border-t-2 border-gray-100 mt-6 pt-6"
        >
          <div class="text-xl font-bold">
            <nuxt-link class="hover:underline" :to="`/about${article.path}`">{{
              article.title
            }}</nuxt-link>
          </div>
          <div class="mt-4 ml-4 text-lg text-gray-500">
            {{ article.description }}
          </div>
          <div class="mt-4 ml-4">
            <nuxt-link
              class="text-blue-400 text-sm font-bold hover:underline hover:text-blue-600"
              :to="`/about${article.path}`"
              >READ MORE</nuxt-link
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  async asyncData({ $content, params, error }) {
    const articles = await $content()
      .only(['title', 'description', 'path'])
      .sortBy('id', 'asc')
      .limit(6)
      .fetch()
      .catch(() => {
        error({ statusCode: 404, message: 'Page not found' })
      })
    return {
      articles,
    }
  },
}
</script>
