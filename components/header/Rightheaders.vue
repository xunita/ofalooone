<template>
  <div
    class="flex align-center"
    :class="{
      'space-x-5': size > 1100,
    }"
  >
    <div>
      <div v-show="size > 1100" class="flex align-center space-x-5 w-fit">
        <div class="flex align-center space-x-5 w-fit">
          <propertydrophome :color="'rg'" :title="'Acheter'"></propertydrophome>
          <propertydrophome :color="'rg'" :title="'Louer'"></propertydrophome>
        </div>
        <a
          href="/devenir-agent"
          class="button btn-008489 border-none text-white dezz size-13 rounded font-semibold px-4 pb-1.5"
          >Devenir agent</a
        >
      </div>
    </div>
    <div
      class="flex align-center"
      :class="{
        'space-x-8': size > 1100,
        'space-x-6': size <= 699,
      }"
    >
      <!-- <div v-show="size > 889">
        <currency :color="'rfef'"></currency>
      </div> -->
      <div v-show="size <= 699" class="z-10">
        <button
          v-show="!minsearch"
          class="button dezz border-none rounded-full"
          @click="showminsearch"
        >
          <svg
            class="w-6 h-6 logo-color font-semibold"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
            ></path>
          </svg>
        </button>
      </div>
      <signdrop></signdrop>
    </div>
  </div>
</template>

<script>
// import Currency from '../dropdown/Currency.vue'
import Propertydrophome from '../dropdown/Propertydrophome.vue'
import Signdrop from '../dropdown/Signdrop.vue'
export default {
  components: { Propertydrophome, Signdrop },
  computed: {
    size() {
      return this.$store.state.size
    },
    minsearch() {
      return this.$store.state.minsearch === true
    },
  },
  methods: {
    showminsearch() {
      this.$store.commit('set_MinSearch', true)
    },
  },
}
</script>
