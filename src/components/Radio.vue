<script setup lang="ts">
import { computed } from 'vue';


const props = defineProps<{volume: number}>()

const emit = defineEmits<{(e: 'volumeChange', volume: number)}>()

const volume = computed({
    get(){
        return props.volume
    }, 
    set(value){
        emit("volumeChange", value)
    }
})

const selectedValue = computed(()=>{
    return Math.round(props.volume * 100)
})

</script>

<template>
    <div class="flex gap-10 items-center justify-center flex-col flex-wrap">
        <p>Selecione o volume de sua preferência:</p>
        <div class="flex gap-3 items-center justify-center flex-wrap">
            <label v-for="i in 101" :key="i - 1" class="flex gap-3 items-center justify-center">
            {{ i - 1 }}
            <input 
            type="radio" 
            @change="emit('volumeChange', (i - 1) /100)"
            :checked="i - 1 === selectedValue"
            :value="selectedValue">
        </label>
        </div>
    </div>
</template>