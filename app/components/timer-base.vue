<template>
    <div class="w-full max-w-100 aspect-video bg-green-600/40 rounded-2xl">
        <div class="w-full h-full flex items-center justify-center">
            <div class=" rounded-lg bg-green-600/70 p-4 w-25 time">
                dssfsdf
            </div>
        </div>
    </div>
</template>
<script setup lang="ts">
import { $, Timer, createTimer } from 'animejs';

let timer: Timer;
onMounted(() => {
    const [$time] = $('.time')
    timer = createTimer({
        duration: 2000,
        alternate: true,
        loop: true,
        onUpdate: self => {
            if ($time) {
                return $time.innerHTML = self.iterationCurrentTime.toString();
            }
        }
    })
});
</script>