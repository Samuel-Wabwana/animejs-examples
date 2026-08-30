<template>
    <article class="h-screen w-full overflow-hidden bg-black">
        <section class="relative h-full w-full">
            <div
                v-for="(src, i) in images"
                :key="src"
                ref="layerRefs"
                class="absolute left-0 top-0 h-screen w-full will-change-transform"
            >
                <div class="relative h-full w-full overflow-hidden">
                    <img
                        :src="src"
                        :alt="`Parallax image ${i + 1}`"
                        ref="imgRefs"
                        class="h-[130%] w-full object-cover will-change-transform"
                    >
                    <div class="pointer-events-none absolute inset-0 z-10 flex items-center">
                        <svg
                            class="h-auto w-[160%] max-w-none shrink-0 translate-x-[-18.75%] translate-y-[12%] overflow-visible"
                            viewBox="0 0 1400 640"
                            preserveAspectRatio="xMidYMid meet"
                            aria-hidden="true"
                        >
                            <defs>
                                <path
                                    :id="`title-arc-${i}`"
                                    d="M 0 70 Q 700 620 1400 70"
                                />
                            </defs>
                            <text
                                :fill="titleColors[i].back"
                                font-size="182"
                                font-weight="700"
                                letter-spacing="4"
                            >
                                <textPath
                                    :href="`#title-arc-${i}`"
                                    ref="titleRefs"
                                    startOffset="0"
                                >
                                    <tspan
                                        v-for="n in titleCopies"
                                        :key="`title-${n}`"
                                    >WABS NEVERLAND&nbsp;&nbsp;</tspan>
                                </textPath>
                            </text>
                            <text
                                :fill="titleColors[i].front"
                                font-size="48"
                                font-style="italic"
                                dy="-40"
                            >
                                <textPath
                                    :href="`#title-arc-${i}`"
                                    ref="subtitleRefs"
                                    startOffset="0"
                                >
                                    <tspan
                                        v-for="n in subtitleCopies"
                                        :key="`sub-${n}`"
                                    >Creator&nbsp;&nbsp;&nbsp;</tspan>
                                </textPath>
                            </text>
                        </svg>
                    </div>
                </div>
            </div>
        </section>
    </article>
</template>

<script setup lang="ts">
import { animate, type JSAnimation } from 'animejs'

const images = [
    '/jpg/image1.jpg',
    '/jpg/image2.jpg',
    '/jpg/image3.jpg',
    '/jpg/image4.jpg',
    '/jpg/image5.jpg',
]

const titleColors = [
    { back: '#e11d48', front: '#ffffff' },
    { back: '#ffffff', front: '#000000' },
    { back: '#ffffff', front: '#9ca3af' },
    { back: '#ffffff', front: '#facc15' },
    { back: '#ffffff', front: '#000000' },
]

const titleCopies = 8
const subtitleCopies = 20

const layerRefs = ref<HTMLElement[]>([])
const imgRefs = ref<HTMLElement[]>([])
const titleRefs = ref<SVGTextPathElement[]>([])
const subtitleRefs = ref<SVGTextPathElement[]>([])
const titleAnims: JSAnimation[] = []

const startArcMarquee = (
    el: SVGTextPathElement,
    duration: number,
) => {
    const first = el.querySelector('tspan')
    // Measure one repeated unit so the loop tiles without a visible jump
    const unit = first?.getComputedTextLength() || el.getComputedTextLength()
    if (!unit) return animate(el, { duration: 0 })

    const state = { distance: 0 }

    return animate(state, {
        distance: unit,
        duration,
        ease: 'linear',
        loop: true,
        onUpdate: () => {
            // Wrap before applying so loop reset (unit → 0) looks identical
            const offset = -(state.distance % unit)
            el.setAttribute('startOffset', `${offset}`)
        },
    })
}

const scroll = { y: 0 }
let targetY = 0
let touchStartY = 0
let snapTimer: ReturnType<typeof setTimeout> | null = null

const wrap = (value: number, length: number) => ((value % length) + length) % length

const render = () => {
    const h = window.innerHeight
    const total = h * images.length
    const layers = layerRefs.value
    const imgs = imgRefs.value

    for (let i = 0; i < layers.length; i++) {
        const raw = i * h - scroll.y
        let y = wrap(raw, total)
        if (y > total - h) y -= total

        layers[i].style.transform = `translate3d(0, ${y}px, 0)`

        // Inner parallax: image drifts slower than its panel
        const imgOffset = (y / h) * -h * 0.18
        imgs[i].style.transform = `translate3d(0, ${imgOffset}px, 0)`
    }
}

const animateScroll = () => {
    animate(scroll, {
        y: targetY,
        duration: 900,
        ease: 'out(3)',
        composition: 'blend',
        onUpdate: render,
    })
}

const snapToNearest = () => {
    const h = window.innerHeight
    targetY = Math.round(targetY / h) * h
    animateScroll()
}

const scheduleSnap = () => {
    if (snapTimer) clearTimeout(snapTimer)
    snapTimer = setTimeout(snapToNearest, 120)
}

const onWheel = (e: WheelEvent) => {
    e.preventDefault()

    // Normalize trackpad / mouse wheel deltas (pixels, lines, pages)
    const delta =
        e.deltaMode === 1 ? e.deltaY * 16 :
        e.deltaMode === 2 ? e.deltaY * window.innerHeight :
        e.deltaY

    targetY += delta
    animateScroll()
    scheduleSnap()
}

const onTouchStart = (e: TouchEvent) => {
    touchStartY = e.touches[0]?.clientY ?? 0
}

const onTouchMove = (e: TouchEvent) => {
    e.preventDefault()
    const y = e.touches[0]?.clientY ?? touchStartY
    targetY += (touchStartY - y) * 1.5
    touchStartY = y
    animateScroll()
}

const onTouchEnd = () => {
    scheduleSnap()
}

onMounted(async () => {
    document.documentElement.style.overflow = 'hidden'
    render()

    await nextTick()
    titleRefs.value.forEach((el) => {
        titleAnims.push(startArcMarquee(el, 56000))
    })
    subtitleRefs.value.forEach((el) => {
        titleAnims.push(startArcMarquee(el, 4500))
    })

    window.addEventListener('wheel', onWheel, { passive: false })
    window.addEventListener('touchstart', onTouchStart, { passive: true })
    window.addEventListener('touchmove', onTouchMove, { passive: false })
    window.addEventListener('touchend', onTouchEnd)
})

onUnmounted(() => {
    if (snapTimer) clearTimeout(snapTimer)
    titleAnims.forEach((anim) => anim.revert())
    document.documentElement.style.overflow = ''
    window.removeEventListener('wheel', onWheel)
    window.removeEventListener('touchstart', onTouchStart)
    window.removeEventListener('touchmove', onTouchMove)
    window.removeEventListener('touchend', onTouchEnd)
})
</script>
