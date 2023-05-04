<script setup lang="ts">
import { useGeolocation } from "@vueuse/core";
import { ref, watch } from "vue"

const { coords } = useGeolocation()

    
const emit = defineEmits<{ (e: 'volumeChange', volume: number) }>()

const volume = ref(0)

watch(() => coords.value, () => {
    volume.value = Math.abs(Math.round(coords.value.latitude)) / 100
    emit('volumeChange', volume.value)
})

</script>

<template>
    <div class="flex flex-col items-center gap-3 justify-center">
        <p>O volume inicial é baseado em sua localização</p>
        <p>Volume: {{ Math.round(volume * 100) }}</p>
        <p>Latitude: {{ Math.abs(Math.round(coords.latitude)) }}</p>
    </div>
</template>