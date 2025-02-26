<template>
    <nav class="bg-raisin-black fixed w-full z-20 top-0 start-0 border-b border-purple-800">
        <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
            <router-link to="/" class="flex items-center space-x-3 rtl:space-x-reverse" @click="closeMenu">
                <img src="/Lift-Ed.svg" class="h-8" alt="Lift-Ed Logo">
                <span class="self-center text-2xl font-semibold whitespace-nowrap text-white">Lift-Ed</span>
            </router-link>

            <div class="flex md:order-2 space-x-3 md:space-x-0 rtl:space-x-reverse">
                <router-link
                    to="/services"
                    class="text-white bg-purple-700 hover:bg-purple-800 focus:ring-4 focus:outline-none focus:ring-purple-300 font-medium rounded-lg text-sm px-4 py-2 text-center"
                    @click="closeMenu"
                >
                    Get Started
                </router-link>
                <button
                    type="button"
                    class="inline-flex items-center p-2 w-10 h-10 justify-center text-sm text-gray-400 rounded-lg md:hidden hover:bg-granite-gray focus:outline-none focus:ring-2 focus:ring-purple-600"
                    @click="toggleMenu"
                    aria-label="Toggle menu"
                >
                    <svg
                        class="w-5 h-5"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 17 14"
                    >
                        <path
                            stroke="currentColor"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            stroke-width="2"
                            d="M1 1h15M1 7h15M1 13h15"
                        />
                    </svg>
                </button>
            </div>

            <div
                class="w-full md:flex md:w-auto md:order-1 mt-4"
                :class="{ 'hidden': !isMenuOpen }"
            >
                <ul class="flex flex-col p-4 md:p-0 font-medium border border-granite-gray bg-raisin-black md:space-x-8 rtl:space-x-reverse md:flex-row md:mt-0 md:border-0 md:bg-raisin-black">
                    <li v-for="item in navItems" :key="item.path">
                        <router-link
                            :to="item.path"
                            class="block py-2 px-3 text-white rounded md:hover:bg-transparent md:hover:text-purple-500 md:p-0"
                            :class="{
                                'text-purple-500': $route.path === item.path,
                                'hover:bg-granite-gray hover:text-white md:hover:bg-transparent md:hover:text-purple-500': $route.path !== item.path
                            }"
                            @click="closeMenu"
                        >
                            {{ item.name }}
                        </router-link>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<script setup>
import {ref, watch} from 'vue';
import {useRoute} from 'vue-router';

const isMenuOpen = ref(false);
const route = useRoute();

const toggleMenu = () => {
    isMenuOpen.value = !isMenuOpen.value;
};

const closeMenu = () => {
    isMenuOpen.value = false;
};

// Close menu when route changes
watch(() => route.path, () => {
    closeMenu();
});

const navItems = [
    {name: 'Home', path: '/'},
    {name: 'Services', path: '/services'},
    {name: 'About', path: '/about'},
    {name: 'Contact', path: '/contact'}
];

// Close menu when clicking outside
const handleClickOutside = (event) => {
    const nav = document.querySelector('nav');
    if (isMenuOpen.value && nav && !nav.contains(event.target)) {
        closeMenu();
    }
};

// Add click outside listener
if (typeof window !== 'undefined') {
    window.addEventListener('click', handleClickOutside);
}
</script>

<style scoped>
/* Optional: Add smooth transition for menu */
div[class*="w-full"] {
    transition: all 0.3s ease-in-out;
}

/* Optional: Add overlay for mobile menu */
@media (max-width: 768px) {
    div[class*="w-full"]:not(.hidden) {
        background-color: rgba(0, 0, 0, 0.5);
    }
}
</style>
