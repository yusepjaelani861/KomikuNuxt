<template>
    <div class="min-h-screen p-2">
        <div class="p-2 flex items-center mb-4">
            <img src="https://api.multiavatar.com/yusep.svg" class="w-14 h-14 rounded-full mr-4" />
            <div>
                <p class="text-sm">Selamat datang,</p>
                <p class="text-lg font-bold">Guest!</p>
            </div>
        </div>
        <input type="text" placeholder="Search" v-model="search" @keydown.enter="searching"
            class="w-full text-[#B8B8D2] bg-[#F4F3FD] h-12 text-md px-5 pr-16 rounded-lg focus:outline-none" />

        <div class="mt-4 w-full">
            <CardPost v-for="komik in data" :key="komik.id" :data="komik" />
            <!-- Tidak ditemukan -->
            <div class="flex flex-col items-center justify-center">
                <p class="text-lg font-bold mt-4">Tidak ada history</p>
                <p class="text-sm text-[#B8B8D2]">Anda belum membaca satupun Manga</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            data: [],
            search: ''
        }
    },
    mounted() {
        const history = JSON.parse(localStorage.getItem('history'))
        this.data = history
        this.data.reverse()
    },
    methods: {
        searching() {
            const history = JSON.parse(localStorage.getItem('history'))
            const data = history.filter((item) => item.title.toLowerCase().includes(this.search.toLowerCase()))
            this.data = data
        }
    }
}
</script>