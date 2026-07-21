<template>
    <div class=" h-screen w-full">
        <div class=" w-full max-w-7xl mx-auto py-4 ">
            <div class="w-full h-50 border border-slate-200 rounded-xl p-4 flex items-center justify-center">
                <div class="grid grid-cols-2 gap-6 w-full">
                    <div class="text-4xl font-bold text-center border border-slate-200 rounded-xl p-4">
                        {{ referenceValue }}
                    </div>
                    <div class="text-4xl font-bold text-center border border-slate-200 rounded-xl p-4
                    current_time
                    " ref="currentTimeRef">
                        0
                    </div>
                </div>
            </div>
            <div class="w-full grid grid-cols-3 gap-4 mt-4">
                <button class="bg-blue-500 hover:bg-blue-600 transition-all
                 duration-300 text-white rounded-xl px-4 py-2" @click="initReferenceValue">Init</button>
                <button class="bg-green-500 hover:bg-green-600 transition-all duration-300
                 text-white rounded-xl px-4 py-2 cursor-pointer " @click="startTimer">Start</button>
                <button class="bg-red-500 hover:bg-red-600 
                transition-all duration-300 text-white
                 rounded-xl px-4 py-2" @click="stopTimer">Stop</button>
            </div>
        </div>
    </div>
</template>
<script setup lang="ts">
import { Timer, createTimer, utils } from 'animejs';

const referenceValue = ref(0);
const timer = ref<Timer | null>(null);
const currentTimeRef = ref<HTMLElement | null>(null);
const valueFounded = ref(0);

const initReferenceValue = () => {
    referenceValue.value = Math.floor(Math.random() * 1000);
};

const startTimer = () => {
    timer.value?.play();
};

const stopTimer = () => {
    timer.value?.pause();
};

onMounted(() => {
    if (currentTimeRef.value) {
        timer.value = createTimer({
            autoplay: false,
            duration: 10000,
            frameRate: 1000,
            onUpdate: self => {
                if (currentTimeRef.value) {
                    currentTimeRef.value.innerHTML = self.currentTime?.toString() ?? '';
                }
            },
            onPause: (self) => {
                valueFounded.value = self.currentTime ?? 0;
            }
        });
    }
});
</script>