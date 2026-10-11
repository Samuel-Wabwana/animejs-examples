<template>
    <article class=" min-h-screen w-full  bg-black text-white">
        <header class="px-4 flex flex-col justify-center items-center
         gap-4 py-4 lg:flex-row lg:justify-start lg:gap-10 mx-auto max-w-7xl">
            <span class=" text-white text-2xl font-bold"><span class="text-violet-500">Creative</span> Neverland</span>
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
        <main class="px-4 pb-10 max-w-7xl mx-auto">
            <section class="grid grid-cols-1 gap-x-6 gap-y-10 md:grid-cols-2 lg:grid-cols-4">
                <nuxt-link
                    to="/infinite-parallax"
                    class="group relative isolate -m-2 flex flex-col items-start gap-3 rounded-2xl p-2
                        outline-none focus-visible:ring-2 focus-visible:ring-white/70"
                >
                    <span
                        aria-hidden="true"
                        class="pointer-events-none absolute top-2 right-2 bottom-2 left-2 z-0 rounded-xl bg-white
                            opacity-0 transition-[top,right,bottom,left,border-radius,opacity]
                            duration-350 ease-[cubic-bezier(0.22,1,0.36,1)]
                            group-hover:top-0 group-hover:right-0 group-hover:bottom-0 group-hover:left-0
                            group-hover:rounded-2xl group-hover:opacity-100
                            group-focus-visible:top-0 group-focus-visible:right-0 group-focus-visible:bottom-0
                            group-focus-visible:left-0 group-focus-visible:rounded-2xl
                            group-focus-visible:opacity-100
                            motion-reduce:transition-none"
                    />
                    <figure class="relative z-10 aspect-video w-full overflow-hidden rounded-xl p-0">
                        <img
                            src="/png/screen-parallax.png"
                            alt="Infinite Parallax"
                            class="h-full w-full object-cover"
                        >
                    </figure>
                    <h2
                        class="relative z-10 text-base font-medium leading-snug text-white transition-colors
                            duration-350 ease-[cubic-bezier(0.22,1,0.36,1)]
                            group-hover:text-[#0f0f0f] group-focus-visible:text-[#0f0f0f]
                            motion-reduce:transition-none"
                    >
                        Infinite Parallax
                    </h2>
                    <p
                        class="relative z-10 text-sm leading-snug text-slate-300 transition-colors
                            duration-350 ease-[cubic-bezier(0.22,1,0.36,1)]
                            group-hover:text-[#606060] group-focus-visible:text-[#606060]
                            motion-reduce:transition-none"
                    >
                        A collection of infinite parallax effects created with HTML, CSS, and JavaScript.
                    </p>
                </nuxt-link>
                <nuxt-link
                    to="/click-reveal"
                    class="group relative isolate -m-2 flex flex-col items-start gap-3 rounded-2xl p-2
                        outline-none focus-visible:ring-2 focus-visible:ring-white/70"
                >
                    <span
                        aria-hidden="true"
                        class="pointer-events-none absolute top-2 right-2 bottom-2 left-2 z-0 rounded-xl bg-white
                            opacity-0 transition-[top,right,bottom,left,border-radius,opacity]
                            duration-350 ease-[cubic-bezier(0.22,1,0.36,1)]
                            group-hover:top-0 group-hover:right-0 group-hover:bottom-0 group-hover:left-0
                            group-hover:rounded-2xl group-hover:opacity-100
                            group-focus-visible:top-0 group-focus-visible:right-0 group-focus-visible:bottom-0
                            group-focus-visible:left-0 group-focus-visible:rounded-2xl
                            group-focus-visible:opacity-100
                            motion-reduce:transition-none"
                    />
                    <figure class="relative z-10 aspect-video w-full overflow-hidden rounded-xl p-0">
                        <img
                            src="/png/hero-reveal.png"
                            alt="Header Click reveal"
                            class="h-full w-full object-cover"
                        >
                    </figure>
                    <h2
                        class="relative z-10 text-base font-medium leading-snug text-white transition-colors
                            duration-350 ease-[cubic-bezier(0.22,1,0.36,1)]
                            group-hover:text-[#0f0f0f] group-focus-visible:text-[#0f0f0f]
                            motion-reduce:transition-none"
                    >
                       Hero Click Reveal
                    </h2>
                    <p
                        class="relative z-10 text-sm leading-snug text-slate-300 transition-colors
                            duration-350 ease-[cubic-bezier(0.22,1,0.36,1)]
                            group-hover:text-[#606060] group-focus-visible:text-[#606060]
                            motion-reduce:transition-none"
                    >
                        A click reveal effect for the header.
                    </p>
                </nuxt-link>
            </section>
        </main>
    </article>
</template>
<script setup lang="ts">

</script>