<template>
    <article class=" min-h-screen w-full  bg-black text-white">
        <header class="px-4 flex flex-col justify-center items-center
         gap-4 py-4 lg:flex-row lg:justify-start lg:gap-10 mx-auto max-w-7xl">
            <span class=" text-white text-2xl font-bold">Creative Neverland</span>
            <nav>
                <ul class="text-sm flex gap-4 items-center">
                    <li><NuxtLink to="/" class=" text-slate-200
                         hover:text-white duration-700 transition-all">Home</NuxtLink></li>
                    <li><NuxtLink to="/" class=" text-slate-200
                         hover:text-white duration-700 transition-all">About me</NuxtLink></li>
                </ul>
            </nav>
        </header>
        <section class="px-4 w-full h-100 flex justify-center items-center">
            <div class="w-full h-full flex flex-col justify-center items-center gap-4">
                <h1 class="text-4xl font-bold">Creative Neverland</h1>
                <p class=" text-slate-200">
                    Here is my creative space, a place where i share my thoughts, ideas, and projects.
                </p>
            </div>
        </section>
        <main class="px-4 max-w-7xl mx-auto">
            <section class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4">
                <nuxt-link to="/infinite-parallax" class="flex flex-col items-start gap-4">
                    <img src="https://via.placeholder.com/150" alt="Project 1" class="w-full h-full object-cover">
                    <h2 class="text-2xl font-bold">Infinite Parallax</h2>
                    <p class=" text-slate-200">
                        A collection of infinite parallax effects created with HTML, CSS, and JavaScript.
                    </p>
                </nuxt-link>
            </section>
        </main>
    </article>
</template>
<script setup lang="ts">

</script>