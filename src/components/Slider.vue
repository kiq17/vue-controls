<script setup lang="ts">
import { computed, ref, watch } from 'vue';


const props = defineProps<{
    volume: number
}>()

const emit = defineEmits<{ (e: 'volumeChange', volume: number) }>()

const volume = computed({
    get() {
        return props.volume
    },

    set(value) {
        emit("volumeChange", value)
    }
})

const rotate = ref(0)
const rotateLimite = 25
let interval: any = null
const gravity = 0.02

let volumeControl = ref<HTMLElement | null>(null)


const handleMouseDown = (event: MouseEvent) => {
    clearInterval(interval);
    const clickX = event.clientX;
    const volumeControlRect = volumeControl?.value?.getBoundingClientRect()
    if (!volumeControlRect) return
    const centerX = volumeControlRect.left + volumeControlRect.width / 2
    if (clickX < centerX) {
        while (rotate.value > -rotateLimite) rotate.value -= 1
    } else {
        while (rotate.value < rotateLimite) rotate.value += 1
    }
}

const handleMouseUp = (event: MouseEvent) => {
    clearInterval(interval)
    rotate.value = 0
}

watch(() => rotate.value, () => {
    if (rotate.value > 0) {
        interval = setInterval(() => {
            if (volume.value < 1) {
                if (volume.value + (gravity * rotate.value) / 100 > 1) {
                    volume.value = 1
                } else {
                    volume.value += (gravity * rotate.value) / 100
                }
            }
        }, 5)
    }
    if (rotate.value < 0) {
        interval = setInterval(() => {
            if (volume.value > 0 && rotate.value < -1) {
                if (volume.value + (gravity * rotate.value) / 100 < 0) {
                    volume.value = 0
                } else {
                    volume.value += (gravity * rotate.value) / 100
                }
            }
        }, 5)
    }

})


</script>

<template>
    <div ref="volumeControl" @mousedown="handleMouseDown" @mouseup="handleMouseUp"
        class=" transition-transform flex gap-3 items-center justify-center" :style="{ transform: `rotate(${rotate}deg)` }">
        <i class="fa-solid fa-volume-xmark"></i>
        <input v-model="volume" disabled type="range" min="0" max="1" step="0.01"
            class="-webkit-appearance-none appearance-none w-full h-6 rounded-[10px] outline-none bg-slate-700 transition duration-150 ease-in-out">
        <i class="fa-solid fa-volume-high"></i>
    </div>
</template>