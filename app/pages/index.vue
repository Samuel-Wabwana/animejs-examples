<template>
    <div class=" h-screen w-full">
        <div class=" grid grid-cols-2 gap-4 md:grid-cols-3 lg:grid-cols-4 w-full max-w-7xl mx-auto py-4 ">
            <NuxtLink to="/games/timer" class="flex flex-col items-center justify-center border
             border-slate-200 hover:text-green-500 hover:border-green-500 transition-all duration-300
              rounded-lg p-4 w-full h-full hover:bg-green-50">
                <h2 class=" text-2xl font-bold ">Timer</h2>
            </NuxtLink>
            <NuxtLink to="/games/dancing-bubble" class="flex flex-col items-center justify-center border
             border-slate-200 hover:text-green-500 hover:border-green-500 transition-all duration-300
              rounded-lg p-4 w-full h-full hover:bg-green-50">
                <h2 class=" text-2xl font-bold ">Dancing Bubble</h2>
            </NuxtLink>
        </div>
    </div>
</template>
<script setup lang="ts">

</script>