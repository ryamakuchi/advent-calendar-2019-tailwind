<template>
  <div>
    <ul class="flex justify-between items-center bg-teal-500 p-6">
      <li class="flex items-center">
        <logo class="mr-2" />
        <a
          class="text-white text-teal-200 hover:text-white text-2xl"
          href="https://qiita.com/advent-calendar/2019/nuxt-js"
          target="_blank"
          rel="noopener noreferrer"
        >
          Nuxt.js Advent Calendar 2019 for Qiita
        </a>
      </li>

      <li>
        <a
          href="https://tailwindcss.com/"
          target="_blank"
          rel="noopener noreferrer"
          class="inline-block px-4 py-2 leading-none border rounded text-white border-white hover:border-transparent hover:text-teal-500 hover:bg-white mt-4 lg:mt-0"
        >
          Made with Taiwind CSS
        </a>
      </li>
    </ul>

    <nuxt />
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  }
}
</script>
