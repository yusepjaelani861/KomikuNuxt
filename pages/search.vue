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

        <div class="mt-4 w-full">
            <CardPost v-if="data.length > 0" v-for="item in data" :key="item.id" :data="item" />
            <!-- Tidak ditemukan -->
            <div v-else class="flex flex-col items-center justify-center">
                <p class="text-lg font-bold mt-4">Tidak ditemukan</p>
                <p class="text-sm text-[#B8B8D2]">Manga yang anda cari tidak ditemukan</p>
            </div>
            <LoaderCardPost v-if="loading" v-for="i in 5" :key="i" />
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            data: [],
            page: this.$route.query.page || 1,
            search: '',
            loading: true,
        }
    },
    asyncData({ isDev, route, store, env, params, query, req, res, redirect, error }) {
        return {
            search: query.search || ''
        }
    },
    async mounted() {
        const res = await this.$axios.get('/api/v1/komiku/search?search=' + this.search + (this.page > 1 ? '&page=' + this.page : ''))
        this.data = res.data.data
        if (this.data.length > 0) {
            this.loading = false
        }

        window.addEventListener('scroll', this.handleScroll)
    },
    methods: {
        async searchManga() {
            this.data = []
            this.loading = true
            this.$router.push({ path: '/search', query: { search: this.search } })
            const res = await this.$axios.get('/api/v1/komiku/search?search=' + this.search + (this.page > 1 ? '&page=' + this.page : ''))
            this.data = res.data.data
            if (this.data.length > 0) {
                this.loading = false
            }

            this.page = 1
        },
        handleScroll() {
            if (window.scrollY + window.innerHeight >= document.body.offsetHeight) {
                this.loadMore()
            }
        },
        async loadMore() {
            if (!this.loading && this.$route.path == '/search') {
                this.loading = true
                setTimeout(async () => {
                    const res = await this.$axios.get('/api/v1/komiku/search?search=' + this.search + '&page=' + (this.page + 1))
                    this.data = this.data.concat(res.data.data)
                    if (res.data.data.length > 0) {
                        this.page++
                    }
                    this.loading = false
                }, 1000)
            }
        }
    }
}
</script>