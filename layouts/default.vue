<template>
  <div>
    <nav class="">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex items-center justify-between h-16">
          <div></div>
          <div class="">
            <div class="ml-10 flex items-baseline space-x-4">
              <transition name="in-out-translate-fade" mode="out-in">
                <div v-if="$route.name === 'index'" key="about">
                  <nuxt-link
                    to="/about"
                    class="px-3 py-2 rounded-md text-sm text-gray-500 font-bold hover:text-gray-900 focus:outline-none"
                    >About</nuxt-link
                  >
                </div>
                <div v-else key="home">
                  <nuxt-link
                    to="/"
                    class="px-3 py-2 rounded-md text-sm text-gray-500 font-bold hover:text-gray-900 focus:outline-none"
                    >Home</nuxt-link
                  >
                </div>
              </transition>
            </div>
          </div>
        </div>
      </div>
    </nav>
    <Nuxt />
  </div>
</template>

<style>
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}
</style>
