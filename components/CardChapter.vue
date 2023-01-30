<template>
    <div class="flex items-center justify-between mb-4">
        <div class="flex items-center">
            <!-- <p class="text-2xl text-gray-500 font-bold">{{ data.title.replace('Chapter ', '') }}</p> -->
            <div class="ml-4">
                <p class="text-md font-bold mb-1">{{ data.title }}</p>
                <p class="text-sm text-gray-500">{{ data.images ? data.images.length : 0 }} pages</p>
            </div>
        </div>
        <button @click="setRead(data.url)"
            class="px-4 py-2 text-md text-white bg-[#61BFAD] rounded-lg focus:outline-none">
            <svg class="w-6 h-6" viewBox="0 0 20 17" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path
                    d="M10 2.253V15.253M10 2.253C8.832 1.477 7.246 1 5.5 1C3.754 1 2.168 1.477 1 2.253V15.253C2.168 14.477 3.754 14 5.5 14C7.246 14 8.832 14.477 10 15.253M10 2.253C11.168 1.477 12.754 1 14.5 1C16.247 1 17.832 1.477 19 2.253V15.253C17.832 14.477 16.247 14 14.5 14C12.754 14 11.168 14.477 10 15.253"
                    stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
            </svg>
        </button>
    </div>
</template>

<script>
export default {
    props: ['data', 'open', 'setRead']
}
</script>