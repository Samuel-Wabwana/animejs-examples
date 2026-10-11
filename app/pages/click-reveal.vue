<template>
    <main class="h-screen w-screen relative overflow-hidden">
        <div
            ref="clickHint"
            class="absolute inset-0 z-20 pointer-events-none select-none
         text-white font-bold text-4xl flex justify-center items-center"
        >
            <span class="  font-smooch-sans">Click to reveal</span>
        </div>
        <div
            id="tiles"
            ref="wrapper"
            class="h-full w-full grid absolute inset-0 z-10"
            :class="{ 'pointer-events-none': revealed }"
        >
            <div class="tile relative before:content-[''] before:absolute before:inset-0 before:bg-black" v-for="i in numberOfTiles"
            @click="handleTileClick(i)"
            :key="i"></div>
        </div>
        <div class="absolute inset-0 z-0 overflow-y-auto overflow-x-hidden bg-[#f2f4f2] text-[#1a1a1a] pointer-events-none select-none">
            <div class="mx-auto flex min-h-full max-w-350 flex-col px-5 pb-10 pt-5 md:px-10 lg:px-14">
                <header class="border-b border-[#1a1a1a]/15 pb-4">
                    <div class="grid grid-cols-[1fr_auto_1fr] items-center gap-4 text-[10px] font-smooch-sans-medium uppercase tracking-[0.2em] md:text-[11px]">
                        <nuxt-link to="/" type="button" class="flex items-center gap-2 justify-self-start pointer-events-auto" tabindex="-1">
                            Home
                        </nuxt-link>
                        <span class="font-playwrite text-2xl normal-case tracking-normal md:text-3xl">Creative Neverland</span>
                        <span class="flex items-center justify-end gap-2">
                            Contact
                            <svg class="size-3.5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true">
                                <rect x="3" y="5" width="18" height="14" rx="1" />
                                <path d="M3 7l9 6 9-6" />
                            </svg>
                        </span>
                    </div>
                </header>

                <section class="mx-auto mt-8 max-w-3xl text-center md:mt-10">
                    <h1 class="font-playwrite text-4xl leading-tight md:text-5xl lg:text-[3.25rem]">
                        Natur Tec Specialty
                    </h1>
                    <p class="mx-auto mt-4 max-w-xl text-[11px] leading-relaxed font-smooch-sans-light md:text-xs">
                        Bonneville Homes presents the Natur Series which includes several models of new
                        manufactured homes and new manufactured chalets!
                    </p>
                </section>

                <section class="mt-10 flex flex-1 flex-col items-center justify-center pb-10 md:mt-12">
                    <div class="relative shrink-0" :style="{ width: `${mosaicW}px`, height: `${mosaicH}px` }">
                        <div
                            v-for="(piece, index) in mosaicPieces"
                            :key="index"
                            class="absolute overflow-hidden rounded-2xl bg-[#f2f4f2]"
                            :style="pieceBoxStyle(piece)"
                        >
                            <img
                                :src="heroImage"
                                alt=""
                                class="absolute max-w-none object-cover"
                                :style="pieceImageStyle(piece)"
                                draggable="false"
                            >
                        </div>
                    </div>
                </section>
            </div>
        </div>
    </main>
</template>

<script setup lang="ts">
import { animate, stagger } from 'animejs'

const heroImage = '/jpg/image3.jpg'

type MosaicPiece = { col: number; row: number; colSpan: number; rowSpan: number }

const mosaicW = 400
const mosaicH = 520
const mosaicCols = 10
const mosaicRows = 14
const mosaicGap = 9

const mosaicPieces: MosaicPiece[] = [
    { col: 0, row: 0, colSpan: 2, rowSpan: 5 },
    { col: 2, row: 0, colSpan: 4, rowSpan: 3 },
    { col: 6, row: 0, colSpan: 2, rowSpan: 4 },
    { col: 8, row: 0, colSpan: 2, rowSpan: 2 },
    { col: 2, row: 3, colSpan: 2, rowSpan: 4 },
    { col: 4, row: 3, colSpan: 3, rowSpan: 5 },
    { col: 7, row: 4, colSpan: 3, rowSpan: 3 },
    { col: 0, row: 5, colSpan: 2, rowSpan: 4 },
    { col: 2, row: 7, colSpan: 2, rowSpan: 3 },
    { col: 4, row: 8, colSpan: 4, rowSpan: 4 },
    { col: 8, row: 7, colSpan: 2, rowSpan: 5 },
    { col: 0, row: 9, colSpan: 2, rowSpan: 3 },
    { col: 2, row: 10, colSpan: 2, rowSpan: 4 },
]

const cellWidth = (mosaicW - (mosaicCols - 1) * mosaicGap) / mosaicCols
const cellHeight = (mosaicH - (mosaicRows - 1) * mosaicGap) / mosaicRows

function pieceMetrics(piece: MosaicPiece) {
    const left = piece.col * (cellWidth + mosaicGap)
    const top = piece.row * (cellHeight + mosaicGap)
    const width = piece.colSpan * cellWidth + (piece.colSpan - 1) * mosaicGap
    const height = piece.rowSpan * cellHeight + (piece.rowSpan - 1) * mosaicGap
    return { left, top, width, height }
}

function pieceBoxStyle(piece: MosaicPiece) {
    const { left, top, width, height } = pieceMetrics(piece)
    return {
        left: `${left}px`,
        top: `${top}px`,
        width: `${width}px`,
        height: `${height}px`,
    }
}

function pieceImageStyle(piece: MosaicPiece) {
    const { left, top } = pieceMetrics(piece)
    return {
        width: `${mosaicW}px`,
        height: `${mosaicH}px`,
        left: `${-left}px`,
        top: `${-top}px`,
    }
}

const columns = ref(0)
const rows = ref(0)
const numberOfTiles = computed(() => columns.value * rows.value);
const wrapper = ref<HTMLDivElement | null>(null)
const clickHint = ref<HTMLDivElement | null>(null)

const revealed = ref(false)
const revealStarted = ref(false)

const STAGGER_MS = 20
const TILE_REVEAL_DURATION = 1000

function revealPropagationDuration(clickIndex: number) {
    const grid: [number, number] = [columns.value, rows.value]
    const targets = Array.from({ length: numberOfTiles.value }, () => ({}))
    const delayFn = stagger(STAGGER_MS, { grid, from: clickIndex })
    const maxDelay = Math.max(
        ...targets.map((target, i) => delayFn(target, i, targets) as number),
    )
    return maxDelay + TILE_REVEAL_DURATION
}

const setColumnsAndRows = () => {
    columns.value = Math.floor(document.body.clientWidth / 20)
    rows.value = Math.floor(document.body.clientHeight / 20)
}

const handleTileClick = (index: number) => {
    if (revealStarted.value) return
    revealStarted.value = true

    const grid: [number, number] = [columns.value, rows.value]
    const staggerOptions = { grid, from: index }

    if (clickHint.value) {
        animate(clickHint.value, {
            opacity: 0,
            duration: revealPropagationDuration(index) * 0.9,
            ease: 'linear',
        })
    }

    animate('.tile', {
        opacity: 0,
        duration: TILE_REVEAL_DURATION,
        delay: stagger(STAGGER_MS, staggerOptions),
        onComplete: () => {
            revealed.value = true
        },
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
