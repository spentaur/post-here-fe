<template>
  <div
    id="container"
    class="bg-white py-16 px-4 overflow-hidden sm:px-6 lg:px-8"
  >
    <div class="relative max-w-2xl mx-auto">
      <div class="text-center">
        <h2
          class="text-3xl leading-9 font-extrabold tracking-tight text-gray-900 sm:text-4xl sm:leading-10"
        >
          About
        </h2>
      </div>
      <div class="mt-4">
        Post Here uses a DistilBERT classification model and ~1,000,000 posts
        mined from ~5,000 subreddits. The front end uses Nuxt and tailwind ui
        and is deployed as a static site to Cloudflare using Cloudflare workers.
        The model was converted to onnx and uses Azure Functions and onnxruntime
        to run inferences. Please refer to the following repos and blog posts
        below for more information, and code examples.
      </div>
      <div class="mt-6">
        <span class="font-bold text-lg">Repos</span>
        <ul class="mt-1 ml-4">
          <li>
            <a
              class="text-blue-700 hover:underline"
              href="https://github.com/spentaur/post-here-fe"
              >Front End</a
            >
          </li>
          <li>
            <a
              class="text-blue-700 hover:underline"
              href="https://github.com/spentaur/post-here-function"
              >Model Deployment</a
            >
          </li>
        </ul>
      </div>
      <div class="mt-12">
        <div>Blog 1</div>
        <div>Blog 2</div>
        <div>Blog 3</div>
        <div>Blog 4</div>
      </div>
    </div>
  </div>
</template>
<script>
export default {}
</script>
