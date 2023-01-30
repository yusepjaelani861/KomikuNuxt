<template>
    <div class="min-h-screen p-2">
        <div class="p-2 flex items-center mb-4">
            <img src="https://api.multiavatar.com/yusep.svg" class="w-14 h-14 rounded-full mr-4" />
            <div>
                <p class="text-sm">Selamat datang,</p>
                <p class="text-lg font-bold">Guest!</p>
            </div>
        </div>
        <select class="w-full text-[#B8B8D2] bg-[#F4F3FD] h-12 text-md px-5 pr-16 rounded-lg focus:outline-none">
            <option value="1">Semua Manga</option>
        </select>

        <div class="mt-4 w-full">
            <CardPost v-if="data.length > 0" v-for="komik in data" :key="komik.id" :data="komik" />
            <LoaderCardPost v-else v-for="i in 5" :key="i" />
        </div>
    </div>
</template>

<script>
export default {
    name: 'MangaPage',
    data() {
        return {
            data: []
        }
    },
    mounted() {
        const bookmarks = JSON.parse(localStorage.getItem('bookmarks'))
        this.data = bookmarks
        this.data.reverse()
    }
}
</script>