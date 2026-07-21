<template>
    <div class="h-screen w-screen bg-indigo-950 flex items-center justify-center">
        <div class="relative w-60 h-60">
            <div class="absolute inset-0 m-auto w-30 h-30 bg-yellow-500 rounded-full"></div>

            <div class="orbit absolute inset-0">
                <div class="bubble absolute top-30 left-0 w-2 h-2 -mt-1 bg-white rounded-full"></div>
            </div>
            <div class="orbit absolute inset-0">
                <div class="bubble absolute top-30 left-4 w-2 h-2 -mt-1 bg-white rounded-full"></div>
            </div>
        </div>
    </div>
</template>
<script setup lang="ts">
import { animate } from 'animejs';

onMounted(() => {
    animate('.orbit', {
        rotate: '1turn',
        duration: (_, i) =>  4000 / ((i ?? 0) + 1),
        ease: 'linear',
        loop: true,
    })
})
</script>
