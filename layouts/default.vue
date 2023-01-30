<template>
    <div class="md:max-w-3xl w-full mx-auto">
        <main class="p-2 mb-16">
            <RouterView />
        </main>
        <div class="fixed bottom-0 p-4 bg-white w-full md:max-w-3xl mx-auto">
            <div class="w-full grid grid-cols-4 items-center">
                <RouterLink to="/" class="text-gray-300 hover:text-green-600 flex flex-col items-center justify-center"
                    :class="{ 'text-green-600': $route.path === '/' }">
                    <svg class="w-6 h-6" viewBox="0 0 18 20" fill="currentColor"
                        xmlns="http://www.w3.org/2000/svg">
                        <path fill-rule="evenodd" clip-rule="evenodd"
                            d="M7.8 1.02503C8.51111 0.491697 9.48889 0.491697 10.2 1.02503L17.2 6.27503C17.7036 6.65274 18 7.24552 18 7.87503V17.375C18 18.4796 17.1046 19.375 16 19.375H2C0.89543 19.375 0 18.4796 0 17.375V7.87503C0 7.24552 0.296388 6.65274 0.8 6.27503L7.8 1.02503Z"
                            fill="currentColor" />
                    </svg>
                    <p class="text-md mt-1">Home</p>
                </RouterLink>

                <RouterLink to="/manga"
                    class="text-gray-300 hover:text-green-600 flex flex-col items-center justify-center"
                    :class="{ 'text-green-600': $route.path === '/manga' }">
                    <svg class="w-6 h-6" viewBox="0 0 24 24" fill="currentColor"
                        xmlns="http://www.w3.org/2000/svg">
                        <path fill-rule="evenodd" clip-rule="evenodd"
                            d="M4 5.47095C4 3.81409 5.34315 2.47095 7 2.47095H18C19.1046 2.47095 20 3.36638 20 4.47095V17.4609C20 17.6285 19.9623 17.7825 19.8872 17.9326L19.3416 19.0237C19.2009 19.3053 19.2009 19.6366 19.3416 19.9182L19.8854 21.0057C19.9586 21.1447 20 21.303 20 21.4709C20 22.0232 19.5523 22.4709 19 22.4709H7C5.34315 22.4709 4 21.1278 4 19.4709V5.47095ZM7 18.4709H17.4076C17.1789 19.1177 17.1789 19.8242 17.4076 20.4709H7C6.44772 20.4709 6 20.0232 6 19.4709C6 18.9187 6.44772 18.4709 7 18.4709ZM10 7.47095C9.44772 7.47095 9 7.91866 9 8.47095C9 9.02323 9.44772 9.47095 10 9.47095H14C14.5523 9.47095 15 9.02323 15 8.47095C15 7.91866 14.5523 7.47095 14 7.47095H10Z"
                            fill="currentColor" />
                    </svg>
                    <p class="text-md mt-1">Manga</p>
                </RouterLink>

                <RouterLink to="/search" class="text-gray-300 hover:text-green-600 flex flex-col items-center justify-center"
                    :class="{ 'text-green-600': $route.path === '/search' }"
                >
                    <svg class="w-5 h-5" viewBox="0 0 20 20" fill="currentColor"
                        xmlns="http://www.w3.org/2000/svg">
                        <path fill-rule="evenodd" clip-rule="evenodd"
                            d="M0.833332 8.75001C0.833332 4.37777 4.37776 0.833344 8.75 0.833344C13.1222 0.833344 16.6667 4.37777 16.6667 8.75001C16.6667 10.636 16.0072 12.3679 14.9063 13.7278L17.9672 16.7887C18.2927 17.1141 18.2927 17.6418 17.9672 17.9672C17.6418 18.2927 17.1141 18.2927 16.7887 17.9672L13.7278 14.9063C12.3679 16.0072 10.6359 16.6667 8.75 16.6667C4.37776 16.6667 0.833332 13.1222 0.833332 8.75001ZM8.75 2.50001C5.29824 2.50001 2.5 5.29825 2.5 8.75001C2.5 12.2018 5.29824 15 8.75 15C12.2018 15 15 12.2018 15 8.75001C15 5.29825 12.2018 2.50001 8.75 2.50001Z"
                            fill="currentColor" />
                    </svg>
                    <p class="text-md mt-1">Search</p>
                </RouterLink>

                <RouterLink to="/history" class="text-gray-300 hover:text-green-600 flex flex-col items-center justify-center"
                    :class="{ 'text-green-600': $route.path === '/history' }"
                >
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                        stroke="currentColor" class="w-6 h-6">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="M12 6v6h4.5m4.5 0a9 9 0 11-18 0 9 9 0 0118 0z" />
                    </svg>
                    <p class="text-md mt-1">History</p>
                </RouterLink>
            </div>
        </div>
    </div>
</template>