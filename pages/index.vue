<template>
  <div id="container" class="bg-white pt-3 pb-16 px-4 overflow-hidden sm:px-6">
    <div class="relative max-w-2xl mx-auto">
      <div class="text-center">
        <h2
          class="text-3xl leading-9 font-extrabold tracking-tight text-gray-900 sm:text-4xl sm:leading-10"
        >
          Post Here
        </h2>
        <p class="mt-4 text-lg leading-6 text-gray-500">
          Enter in a piece of text to find out which subreddit to post it in.
        </p>
      </div>
      <div class="mt-12">
        <div class="relative">
          <transition name="fade">
            <div
              v-show="!modelLoaded"
              class="bg-gray-200 absolute rounded shadow-2xl h-full w-full z-10 justify-center items-center flex flex-col"
            >
              <button
                :disabled="loadingModel"
                type="button"
                class="w-48 inline-flex items-center justify-center px-6 py-3 border border-transparent text-base leading-6 font-medium rounded-lg text-white bg-indigo-600 hover:bg-indigo-500 focus:outline-none focus:border-indigo-600 focus:shadow-outline-indigo active:bg-indigo-600 transition ease-in-out duration-150"
                @click.prevent="loadModel"
              >
                <loading v-if="loadingModel" />
                <template v-else> Load Model </template>
              </button>
              <div class="mt-4 text-gray-500 font-medium text-sm">
                {{ modelError || 'May take ~1 minute' }}
              </div>
            </div>
          </transition>
          <div>
            <form
              class="grid grid-cols-1 gap-y-8 sm:grid-cols-2 sm:gap-x-8"
              @submit.prevent="getPreds"
            >
              <div class="sm:col-span-2">
                <label
                  for="text"
                  class="block text-sm font-medium leading-5 text-gray-700"
                  >Text</label
                >
                <div
                  :class="{ 'shadow-lg': modelLoaded }"
                  class="mt-1 relative rounded-lg transition-shadow duration-500"
                >
                  <textarea
                    id="text"
                    v-model="text"
                    rows="8"
                    class="form-textarea py-3 px-4 block w-full transition border-gray-200 ease-in-out duration-150"
                  ></textarea>
                </div>
              </div>
              <div class="sm:col-span-2">
                <div class="flex items-start">
                  <div class="flex-shrink-0">
                    <span
                      role="checkbox"
                      :aria-checked="nsfw"
                      tabindex="0"
                      :class="nsfw ? 'bg-orange-500' : 'bg-gray-200'"
                      class="relative inline-block flex-shrink-0 h-6 w-11 border-2 border-transparent rounded-full cursor-pointer transition-colors ease-in-out duration-200 focus:outline-none"
                      @click="nsfw = !nsfw"
                    >
                      <span
                        aria-hidden="true"
                        :class="nsfw ? 'translate-x-5' : 'translate-x-0'"
                        class="inline-block h-5 w-5 rounded-full bg-white shadow transform transition ease-in-out duration-200"
                      ></span>
                    </span>
                  </div>
                  <div class="ml-3">
                    <p class="text-base leading-6 text-gray-500">Show NSFW</p>
                  </div>
                </div>
              </div>
              <div class="sm:col-span-2">
                <span
                  :class="{ 'shadow-lg': modelLoaded }"
                  class="w-full h-12 inline-flex rounded-lg transition-shadow duration-500"
                >
                  <button
                    :disabled="loading"
                    type="submit"
                    class="w-full inline-flex items-center justify-center px-6 py-3 border border-transparent text-base leading-6 font-medium rounded-lg text-white bg-orange-500 hover:bg-orange-400 focus:outline-none focus:border-orange-600 focus:shadow-outline-indigo active:bg-orange-600 transition ease-in-out duration-150"
                  >
                    <loading v-if="loading" />
                    <template v-else> Submit </template>
                  </button>
                </span>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div
      v-show="preds.length > 0"
      class="relative mt-12 max-w-2xl lg:max-w-5xl mx-auto"
    >
      <ul id="preds" class="grid grid-cols-1 gap-6 lg:grid-cols-2">
        <li
          v-for="pred in preds"
          :key="pred.data.data.display_name"
          class="col-span-1 bg-white rounded-lg shadow-lg flex flex-col"
        >
          <div
            v-if="!pred.data.data.over18 || nsfw"
            class="w-full flex-grow flex justify-between p-6 space-x-6"
          >
            <div class="flex-1">
              <div class="flex-col items-center">
                <h3 class="text-lg font-medium text-gray-900">
                  <a
                    :href="`https://reddit.com/${pred.data.data.display_name_prefixed}`"
                    >{{ pred.data.data.display_name }}</a
                  >
                </h3>
                <p class="text-sm text-gray-500">
                  <a
                    :href="`https://reddit.com/${pred.data.data.display_name_prefixed}`"
                  >
                    {{ pred.data.data.display_name_prefixed }}
                  </a>
                </p>
              </div>
              <p class="mt-1 text-gray-900 text-sm leading-5">
                {{ he.decode(pred.data.data.public_description) }}
              </p>
            </div>
            <img
              v-if="pred.data.data.community_icon || pred.data.data.icon_img"
              class="w-10 h-10 bg-gray-300 rounded-full flex-shrink-0"
              :src="
                fixURL(pred.data.data.community_icon || pred.data.data.icon_img)
              "
              alt=""
            />
          </div>
          <div
            v-else
            class="w-full flex-grow flex items-center justify-center rounded-t-lg bg-gray-100 p-6 space-x-6"
          >
            <h3 class="text-4xl font-bold text-gray-300">NSFW</h3>
          </div>
          <div class="border-t border-gray-200">
            <div class="-mt-px flex">
              <div class="w-0 flex-1 flex border-r border-gray-200">
                <span
                  class="relative -mr-px w-0 flex-1 inline-flex items-center justify-center py-4 text-sm leading-5 text-gray-700 font-medium border border-transparent rounded-bl-lg hover:text-gray-500 focus:outline-none focus:shadow-outline-blue focus:border-blue-300 focus:z-10 transition ease-in-out duration-150"
                >
                  <span class="font-bold">
                    {{ pred.data.data.subscribers.toLocaleString() }}
                  </span>
                  &nbsp;subscribers
                </span>
              </div>
              <div class="-ml-px w-0 flex-1 flex">
                <span
                  class="relative -mr-px w-0 flex-1 inline-flex items-center justify-center py-4 text-sm leading-5 text-gray-700 font-medium border border-transparent rounded-bl-lg hover:text-gray-500 focus:outline-none focus:shadow-outline-blue focus:border-blue-300 focus:z-10 transition ease-in-out duration-150"
                >
                  <span class="font-bold">
                    {{ pred.data.data.accounts_active.toLocaleString() }}
                  </span>
                  &nbsp;active
                </span>
              </div>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
const he = require('he')
export default {
  data() {
    return {
      text: '',
      preds: [],
      nsfw: false,
      loading: false,
      he,
      loadingModel: false,
      modelError: '',
      modelLoaded: false,
      timesLoaded: 0,
      inverval: false,
    }
  },
  methods: {
    prime() {
      return this.$axios.get(
        'https://post-here.azurewebsites.net/api/post-here'
      )
    },
    async loadModel() {
      this.loadingModel = true
      const res = await this.$axios.get(
        'https://post-here.azurewebsites.net/api/post-here'
      )
      if (res.data === 'success') {
        this.loadingModel = false
        this.modelLoaded = true
      } else {
        this.modelError = 'Error Loading Model'
      }

      this.interval = setInterval(this.loadModelInBackground, 300000)
    },
    async loadModelInBackground() {
      await this.$axios.get('https://post-here.azurewebsites.net/api/post-here')
      this.timesLoaded += 1
      if (this.timesLoaded >= 5) {
        clearInterval(this.interval)
      }
    },
    async getPreds() {
      const options = {
        easing: 'ease-in',
        offset: -10,
        force: true,
        x: false,
        y: true,
      }
      this.loading = true
      this.preds = []
      const preds = await this.$axios.$post(
        'https://post-here.azurewebsites.net/api/post-here',
        {
          text: this.text.split(' ').slice(0, 512).join(' '),
          k: 16,
        }
      )
      const predsUrls = preds[0].map((pred) =>
        this.$axios
          .get(`https://post-here.com/r/${pred}`)
          .then((res) => res)
          .catch(() => {})
      )
      Promise.all(predsUrls)
        .then((res) => {
          this.preds = res.filter(Boolean)
          this.loading = false
          setTimeout(() => this.$scrollTo('#preds', 200, options), 10)
        })
        .catch(() => {})
    },
    fixURL(url) {
      return url.replace('&amp;', '&')
    },
  },
}
</script>
