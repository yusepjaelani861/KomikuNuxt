<template>
    <div>
        <div v-if="data" class="min-h-screen">
            <div class="relative z-10">
                <div class="absolute top-0 left-0 z-20 w-full py-2 bg-gray-600 bg-opacity-30">
                    <button class="absolute left-0 top-1 px-4 py-2 text-md rounded-lg focus:outline-none"
                        @click="() => $router.back()">
                        <svg class="w-6 h-6" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <path d="M15.6 4.79999L8.40002 12L15.6 19.2" stroke="#F4F3FD" stroke-width="2"
                                stroke-linecap="round" stroke-linejoin="round" />
                        </svg>
                    </button>
                    <div class="flex items-center justify-center">
                        <h1 class="text-2xl font-bold text-white text-center line-clamp-1 w-[60%]">{{ data.title }}</h1>
                    </div>
                </div>
                <img :src="data.thumbnail" class="w-full max-h-96 object-contain rounded-lg" />
                <button
                    class="absolute bottom-2 font-semibold left-2 px-6 py-2 bg-[#61BFAD] text-white rounded-full flex items-center justify-center">
                    Manga
                </button>
            </div>

            <div class="mt-4">
                <h1 class="text-2xl font-bold">{{ data.title }}</h1>
                <div class="flex items-center">
                    <p class="text-sm text-gray-500">{{ data.views }} Views</p>
                    <p class="text-sm text-gray-500 ml-2">•</p>
                    <p class="text-sm text-gray-500 ml-2">{{ data.chapters.length }} Chapters</p>
                </div>

                <div class="mt-4">
                    <h1 class="text-xl font-bold">Sinopsis</h1>
                    <p class="text-sm text-gray-500">
                        {{ data.description }}
                    </p>
                </div>

                <div class="mt-4">
                    <div class="grid grid-cols-3 md:grid-cols-5 items-center">
                        <button v-for="genre in data.genres" :key="genre"
                            class="mr-2 flex-shrink-0 px-4 mb-2 py-1 text-sm text-white bg-[#61BFAD] rounded-full focus:outline-none">
                            {{ genre }}
                        </button>
                    </div>
                </div>

                <div class="mt-4 px-4">
                    <CardChapter v-for="item in data.chapters" :key="item.id" :data="item" :open="openModal"
                        :setRead="setRead" />
                    <div v-if="data.chapters.length === 0" class="flex items-center justify-center">
                        <p class="text-xl font-bold">No Chapters</p>
                    </div>
                </div>
            </div>
        </div>

        <div v-else class="min-h-screen">
            <div class="flex items-center justify-center h-screen">
                <div class="flex flex-col items-center">
                    <div class="w-20 h-20 border-4 border-[#61BFAD] rounded-full animate-spin"></div>
                    <p class="text-xl font-bold mt-4">Loading...</p>
                </div>
            </div>
        </div>

        <ModalReader v-if="modal" :open="openModal" :read="currentRead" :chapters="data.chapters" :slug="slug" />
    </div>
</template>

<script>
export default {
    name: "NuxtTutorial",
    data() {
        return {
            data: null,
            modal: false,
            currentRead: null,
            nextRead: null,
        };
    },
    asyncData({ isDev, route, store, env, params, query, req, res, redirect, error }) {
        return {
            slug: params.slug
        };
    },
    async mounted() {
        const res = await this.$axios.get(`/api/v1/komiku/${this.slug}`);
        this.data = res.data.data;
        console.log(this.data);

        // set localstorage bookmarks
        if (localStorage.getItem("bookmarks") === null) {
            localStorage.setItem("bookmarks", JSON.stringify([]));
        }

        this.data.url = this.slug;

        const bookmarks = JSON.parse(localStorage.getItem("bookmarks"));
        const isBookmarks = bookmarks.find((item) => item.url === this.slug);
        if (!isBookmarks) {
            bookmarks.push(this.data);
            localStorage.setItem("bookmarks", JSON.stringify(bookmarks));
        } else {
            const index = bookmarks.findIndex((item) => item.url === this.slug);
            bookmarks[index] = this.data;
            localStorage.setItem("bookmarks", JSON.stringify(bookmarks));
        }

        // set localstorage history
        if (localStorage.getItem("history") === null) {
            localStorage.setItem("history", JSON.stringify([]));
        }

        const history = JSON.parse(localStorage.getItem("history"));
        const isHistory = history.find((item) => item.url === this.slug);
        if (!isHistory) {
            history.push(this.data);
            localStorage.setItem("history", JSON.stringify(history));
        }

    },
    methods: {
        openModal() {
            this.modal = !this.modal;
        },
        setRead(url) {
            this.currentRead = url;
            if (!this.modal) {
                this.openModal();
            }
        }
    }
}
</script>