<template>
    <main class="h-screen w-screen bg-black">
        <div id="tiles" ref="wrapper" class="h-full w-full grid ">
            <div class="tile" v-for="i in numberOfTiles"
            @click="handleTileClick(i)"
            :key="i"></div>
        </div>
    </main>
</template>

<script setup lang="ts">
import { animate, stagger } from 'animejs'
import { computed, onMounted, ref } from 'vue'

const colors = ['#fff456', '#45ff56', '#56f4ff', '#566cff']

const columns = ref(0)
const rows = ref(0)
const numberOfTiles = computed(() => columns.value * rows.value);
const wrapper = ref<HTMLDivElement | null>(null)

const count = ref(-1);

const setColumnsAndRows = () => {
    columns.value = Math.floor(document.body.clientWidth / 10)
    rows.value = Math.floor(document.body.clientHeight / 10)
}

const handleTileClick = (index: number) => {
    count.value++;
    const backgroundColor = colors[count.value % colors.length] ?? '#efab34'
    animate('.tile', {
        backgroundColor,
        delay: stagger(20, { grid: [columns.value, rows.value], from: index }),
    })
}

onMounted(() => {
    setColumnsAndRows()
    wrapper.value?.style.setProperty('--columns', columns.value.toString())
    wrapper.value?.style.setProperty('--rows', rows.value.toString())
    window.onresize = () => {
        setColumnsAndRows()
        wrapper.value?.style.setProperty('--columns', columns.value.toString())
        wrapper.value?.style.setProperty('--rows', rows.value.toString())
    }
})
</script>
<style scoped>
#tiles {
    grid-template-columns: repeat(var(--columns), 1fr);
    grid-template-rows: repeat(var(--rows), 1fr);
}
</style>
