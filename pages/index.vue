<template>
  <div class="bg-white py-16 px-4 overflow-hidden sm:px-6 lg:px-8 lg:py-24">
    <div class="relative max-w-2xl mx-auto">
      <div class="text-center">
        <h2
          class="text-3xl leading-9 font-extrabold tracking-tight text-gray-900 sm:text-4xl sm:leading-10"
        >
          Post Here
        </h2>
        <p class="mt-4 text-lg leading-6 text-gray-500">
          Enter in a peice of text to find out which subreddit to post it in.
        </p>
      </div>
      <div class="mt-12">
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
            <div class="mt-1 relative rounded-lg shadow-lg">
              <textarea
                id="text"
                v-model="text"
                rows="8"
                class="form-textarea py-3 px-4 block w-full transition ease-in-out duration-150"
              ></textarea>
            </div>
          </div>
          <div class="sm:col-span-2">
            <div class="flex items-start">
              <div class="flex-shrink-0">
                <!--
                Simple toggle

                On: "bg-indigo-600", Off: "bg-gray-200"
              -->
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
            <span class="w-full h-12 inline-flex rounded-lg shadow-lg">
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
    <div
      v-if="preds.length > 0"
      class="relative mt-12 max-w-2xl lg:max-w-5xl mx-auto"
    >
      <ul class="grid grid-cols-1 gap-6 lg:grid-cols-2">
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
                {{ pred.data.data.public_description }}
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
            class="w-full flex-grow flex items-center justify-center bg-gray-100 p-6 space-x-6"
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
                    {{
                      new Intl.NumberFormat('en-IN', {
                        maximumSignificantDigits: 3,
                      }).format(pred.data.data.subscribers)
                    }}
                  </span>
                  &nbsp;subscribers
                </span>
              </div>
              <div class="-ml-px w-0 flex-1 flex">
                <span
                  class="relative -mr-px w-0 flex-1 inline-flex items-center justify-center py-4 text-sm leading-5 text-gray-700 font-medium border border-transparent rounded-bl-lg hover:text-gray-500 focus:outline-none focus:shadow-outline-blue focus:border-blue-300 focus:z-10 transition ease-in-out duration-150"
                >
                  <span class="font-bold">
                    {{
                      new Intl.NumberFormat('en-IN', {
                        maximumSignificantDigits: 3,
                      }).format(pred.data.data.accounts_active)
                    }}
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
export default {
  data() {
    return {
      text: '',
      preds: [],
      nsfw: false,
      loading: false,
    }
  },
  methods: {
    async getPreds() {
      this.loading = true
      this.preds = []
      const preds = await this.$axios.$post(
        'https://post-here.azurewebsites.net/api/post-here',
        {
          text: this.text.split(' ').slice(0, 512).join(' '),
          k: 30,
        }
      )
      const predsUrls = preds[0].map((pred) =>
        this.$axios.get(`https://www.reddit.com/r/${pred}/about.json`)
      )
      this.preds = await Promise.all(predsUrls)
      this.loading = false
    },
    fixURL(url) {
      return url.replace('&amp;', '&')
    },
  },
}
</script>
