<template>
  <div class="min-h-screen p-2">
    <div class="p-2 flex items-center mb-4">
      <img src="https://api.multiavatar.com/yusep.svg" class="w-14 h-14 rounded-full mr-4" />
      <div>
        <p class="text-sm">Selamat datang,</p>
        <p class="text-lg font-bold">Guest!</p>
      </div>
    </div>
    <input type="text" placeholder="Search" v-model="search" @keydown.enter="searchManga"
      class="w-full text-[#B8B8D2] bg-[#F4F3FD] h-12 text-md px-5 pr-16 rounded-lg focus:outline-none" />

    <!-- <div class="flex justify-between items-center mt-4">
      <p class="text-lg font-bold">Trending Manga</p>
      <p class="text-sm text-[#B8B8D2]">See all</p>
    </div> -->

    <!-- <div class="py-2 w-full flex flex-nowrap items-center overflow-x-auto">
      <CardManga v-if="feature.length > 0" v-for="item in feature" :key="feature.id" :data="item" />
      <LoaderCardManga v-for="i in 5" />
    </div> -->

    <div class="flex items-center w-full mt-4">
      <button class="px-4 py-2 text-md mr-2 text-white bg-[#61BFAD] rounded-lg focus:outline-none">
        Semua
      </button>
      <button class="px-4 py-2 text-md mr-2 text-gray-500 rounded-lg focus:outline-none">
        Populer
      </button>
      <button class="px-4 py-2 text-md mr-2 text-gray-500 rounded-lg focus:outline-none">
        Baru
      </button>
    </div>

    <div class="mt-4 w-full">
      <CardPost v-if="data.length > 0" v-for="(item, index) in data" :key="index" :data="item" />
      <LoaderCardPost v-if="loading" v-for="i in 5" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      data: [],
      page: this.$route.query.page || 1,
      loading: true,
      search: '',
    }
  },
  async mounted() {
    const res = await this.$axios.get('/api/v1/komiku/search?search=' + (this.page > 1 ? '&page=' + this.page : ''))
    const data = res.data.data
    this.data = data
    if (this.data.length > 0) {
      this.loading = false
    }
    this.page = 1

    window.addEventListener('scroll', this.handleScroll)
  },
  methods: {
    async handleScroll() {
      const { scrollTop, clientHeight, scrollHeight } = document.documentElement
      if (scrollTop + clientHeight >= scrollHeight) {
        if (!this.loading && this.$route.path === '/') {
          this.loading = true
          setTimeout(async () => {
            const res = await this.$axios.get(`/api/v1/komiku/search?search=&page=${this.page + 1}`)
            const data = res.data.data
            this.data = [...this.data, ...data]
            this.page += 1
            this.loading = false
          }, 1000)
        }
      }
    },
    searchManga() {
      this.$router.push({ path: '/search', query: { search: this.search } })
    }
  }
}
</script>
