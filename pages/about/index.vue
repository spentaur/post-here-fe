<template>
  <div id="container" class="bg-white pt-3 pb-16 px-4 overflow-hidden sm:px-6">
    <div class="relative max-w-2xl mx-auto">
      <div class="text-center">
        <h2
          class="text-3xl leading-9 font-extrabold tracking-tight text-gray-900 sm:text-4xl sm:leading-10"
        >
          About
        </h2>
        <a
          href="https://spentaur.com"
          class="text-blue-400 block mt-2 font-bold hover:underline hover:text-blue-600"
          >Spencer Adams</a
        >
      </div>
      <div class="p-4 sm:text-center text-left">
        Post-Here.com uses a DistilBERT classification model and ~1,000,000
        posts mined from ~5,000 subreddits. The front end uses Nuxt and tailwind
        UI and is deployed as a static site to Cloudflare using Cloudflare
        workers. The model was converted to ONNX and uses Azure Functions and
        onnxruntime to run inferences. Please refer to the following repos and
        blog posts below for more information, and code examples.
      </div>
      <div class="mt-6 leading-relaxed border-t-2 border-gray-100 pt-6">
        <span class="font-bold text-xl">Repos</span>
        <ul class="ml-4">
          <li>
            <a
              class="text-blue-400 font-bold hover:text-blue-600 hover:underline"
              href="https://github.com/spentaur/post-here-fe"
              >Front End</a
            >
          </li>
          <li>
            <a
              class="text-blue-400 font-bold hover:text-blue-600 hover:underline"
              href="https://github.com/spentaur/post-here-function"
              >Model Deployment</a
            >
          </li>
        </ul>
      </div>
      <div>
        <div class="border-t-2 border-gray-100 mt-6 pt-6">
          <div class="text-xl font-bold">
            <nuxt-link class="hover:underline" to="#"
              >Problem Statement & Research</nuxt-link
            >
          </div>
          <div class="mt-4 ml-4">
            Reddit is an online forum where uses can create groups known as
            'subreddits' commonly based on a given topic, however, that is not
            strictly necessary. As of October 27, 2020, there are over 1.5
            million unique subreddits. When users post to Reddit, they must post
            to a specific subreddit. They are allowed to share posts from one
            subreddit to another, a practice known as 'cross-posting'. A useful
            feature might be suggesting relevant subreddits a user might want to
            share a specific post with.
          </div>
          <div class="mt-4 ml-4">
            <nuxt-link
              class="text-blue-400 text-sm font-bold hover:underline hover:text-blue-600"
              to="#"
              >READ MORE</nuxt-link
            >
          </div>
        </div>
        <div class="border-t-2 border-gray-100 mt-6 pt-6">
          <div class="text-xl font-bold">
            <nuxt-link class="hover:underline" to="#">Gathering Data</nuxt-link>
          </div>
          <div class="mt-4 ml-4">
            As with most machine learning tasks, NLP requires quality training
            and evaluation data. There exist several pre-gathered datasets
            consisting of posts from various subreddits. However, these datasets
            can be relatively old, and I would like to use more recent data.
            Reddit also frequently removes subreddits, and the "tone" of a given
            subreddit can change as well. As such, I would like to implement a
            way to easily scrape Reddit and retrain my models at a given time
            interval.
          </div>
          <div class="mt-4 ml-4">
            <nuxt-link
              class="text-blue-400 text-sm font-bold hover:underline hover:text-blue-600"
              to="#"
              >READ MORE</nuxt-link
            >
          </div>
        </div>
        <div class="border-t-2 border-gray-100 mt-6 pt-6">
          <div class="text-xl font-bold">
            <nuxt-link class="hover:underline" to="#"
              >Testing / Training / Evaluation</nuxt-link
            >
          </div>
          <div class="mt-4 ml-4">
            Once we have data, the next step is trying several different
            approaches and deciding which one to use for the final deployment.
            There are so many different models, and parameters, and things to
            consider when testing out machine learning models. We must also
            choose a metric with which to judge the models. Once we've made our
            final decision, we will do one final training with a larger subset
            of the data and evaluate using a validation subset of the data.
          </div>
          <div class="mt-4 ml-4">
            <nuxt-link
              class="text-blue-400 text-sm font-bold hover:underline hover:text-blue-600"
              to="#"
              >READ MORE</nuxt-link
            >
          </div>
        </div>
        <div class="border-t-2 border-gray-100 mt-6 pt-6">
          <div class="text-xl font-bold">
            <nuxt-link class="hover:underline" to="#">Deployment</nuxt-link>
          </div>
          <div class="mt-4 ml-4">
            A model saved on my hard drive doesn't offer much value to many
            people. To allow people to use our model, we must deploy it and
            expose some sort of API route. This is often difficult given the
            requirements of large, modern models. In this particular project,
            there will not be many, predictable, requests to the model, so
            having a constant deployment server would be quite ineffective. As
            such, I will attempt to use Azure Functions, a serverless approach,
            meaning I will only pay per request. Because of my projected number
            of users and my financial constraints, I believe the tradeoffs that
            come from using such an approach will be acceptable.
          </div>
          <div class="mt-4 ml-4">
            <nuxt-link
              class="text-blue-400 text-sm font-bold hover:underline hover:text-blue-600"
              to="#"
              >READ MORE</nuxt-link
            >
          </div>
        </div>
        <div class="border-t-2 border-gray-100 mt-6 pt-6">
          <div class="text-xl font-bold">
            <nuxt-link class="hover:underline" to="#"
              >Automating the Process</nuxt-link
            >
          </div>
          <div class="mt-4 ml-4">
            As mentioned earlier, the data on Reddit is rapidly changing. As
            such, any good model will need to be trained using the most recent
            data possible. To make the process of gathering data, retraining the
            model, and updating the deployment easier, I would like to automate
            the process as much as possible. I will again be using Azure
            Functions to periodically perform this task.
          </div>
          <div class="mt-4 ml-4">
            <nuxt-link
              class="text-blue-400 text-sm font-bold hover:underline hover:text-blue-600"
              to="#"
              >READ MORE</nuxt-link
            >
          </div>
        </div>
        <div class="border-t-2 border-gray-100 mt-6 pt-6">
          <div class="text-xl font-bold">
            <nuxt-link class="hover:underline" to="#">Final Thoughts</nuxt-link>
          </div>
          <div class="mt-4 ml-4">
            As with most projects, many lessons were learned. This is a great
            project to hone your NLP and deployment skills. Moving forward, I
            may choose to change some aspects of this project, such as migrating
            to the more popular AWS, or GCP. I will try to keep the model
            architecture up to date with the current state of the art model
            types.
          </div>
          <div class="mt-4 ml-4">
            <nuxt-link
              class="text-blue-400 text-sm font-bold hover:underline hover:text-blue-600"
              to="#"
              >READ MORE</nuxt-link
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {}
</script>
