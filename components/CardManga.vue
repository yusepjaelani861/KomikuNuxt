<template>
    <div class="w-32 md:w-40 flex-none p-2 rounded-lg bg-white">
        <RouterLink :to="`/${data.slug}`">
            <img :src="data.thumbnail"
                class="w-full rounded-lg"
            />
            <div class="p-2 text-center">
                <p class="text-sm font-bold line-clamp-2">{{ data.title }}</p>
                <p class="text-xs text-[#B8B8D2]">Yusep Jaelani</p>
            </div>
        </RouterLink>
    </div>
</template>

<script>
export default {
    props: ['data']
}
</script>