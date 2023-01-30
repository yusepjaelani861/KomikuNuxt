<template>
    <div class="fixed top-0 left-0 w-full h-full z-50 flex items-center justify-center">
        <div class="h-screen w-full bg-white">
            <div class="flex relative items-center justify-center border-b w-full px-4 py-2">
                <h1 class="text-lg font-bold line-clamp-1 w-[80vw]">{{ title }}</h1>
                <!-- Button close -->
                <button @click="open" class="absolute right-2 focus:outline-none rounded-full p-1.5 bg-gray-300">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
                        stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>

                <!-- Progress bar -->
                <div class="absolute z-30 bottom-0 left-0 w-full h-1 bg-gray-300">
                    <div class="h-full bg-[#61BFAD]" :style="{ width: progress + '%' }"></div>
                </div>
            </div>
            <div class="mt-2 overflow-auto h-full" id="screen-reader" v-on:scroll="countProgress">
                <img v-for="image in images" :src="image" class="w-full object-contain" />
                <div class="pt-[1rem] pb-[5rem] flex items-center justify-center">
                    <button v-if="next_url" @click="updateChapter(next_url)"
                        class="px-4 py-2 bg-[#61BFAD] text-white rounded-lg focus:outline-none">
                        Next Chapters
                    </button>
                </div>

                <div v-if="loading" class="min-h-[90vh]">
                    <div class="flex items-center justify-center h-screen">
                        <div class="flex flex-col items-center">
                            <div class="w-20 h-20 border-4 border-[#61BFAD] rounded-full animate-spin"></div>
                            <p class="text-xl font-bold mt-4">Loading...</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['open', 'read', 'chapters', 'slug'],
    data() {
        return {
            images: [],
            title: '',
            next_url: null,
            loading: true,
            progress: 0
        }
    },
    async mounted() {
        const data = await this.$axios.$get(`/api/v1/komiku/ch/${this.read}`)
        this.images = data.images
        this.title = data.title
        this.loading = false

        const next_chapter = this.chapters.find((item) => item.url === this.read)
        const index = this.chapters.indexOf(next_chapter)
        const next = this.chapters[index + 1]

        if (next) {
            this.next_url = next.url
        }

        const history = JSON.parse(localStorage.getItem("history"));
        if (history) {
            const find = history.find((item) => item.url === this.slug)
            if (find) {
                const index = history.indexOf(find)
                history[index].last_chapter = {
                    slug: this.read,
                    chapter: this.title
                }
                localStorage.setItem("history", JSON.stringify(history))
            }
        }
    },
    methods: {
        async updateChapter(url) {
            this.progress = 0
            this.images = []
            this.loading = true
            const data = await this.$axios.$get(`/api/v1/komiku/ch/${url}`)
            this.images = data.images
            this.title = data.title

            if (this.images.length > 0) {
                this.loading = false
            }

            const next_chapter = this.chapters.find((item) => item.url === url)
            const index = this.chapters.indexOf(next_chapter)
            const next = this.chapters[index + 1]

            if (next) {
                this.next_url = next.url
            } else {
                this.next_url = null
            }

            const history = JSON.parse(localStorage.getItem("history"));
            if (history) {
                const find = history.find((item) => item.url === this.slug)
                if (find) {
                    const index = history.indexOf(find)
                    history[index].last_chapter = {
                        slug: url,
                        chapter: this.title
                    }
                    localStorage.setItem("history", JSON.stringify(history))
                }
            }
        },
        handleScroll() {
            const scrollHeight = document.documentElement.scrollHeight
            const scrollTop = document.documentElement.scrollTop
            const clientHeight = document.documentElement.clientHeight

            const scrolled = (scrollTop / (scrollHeight - clientHeight)) * 100
            this.progress = scrolled
        },
        countProgress() {
            const content = document.getElementById('screen-reader')
            const { scrollHeight, scrollTop, clientHeight } = content
            const scrolled = (scrollTop / (scrollHeight - clientHeight)) * 100
            this.progress = scrolled
        }
    }
}
</script>