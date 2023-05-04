<script setup lang="ts">

const emit = defineEmits<{(e: 'volumeChange', volume: number)}>()

const handleClick = ()=>{
    emit("volumeChange", Math.random())
}

</script>

<template>
    <div class="flex items-center justify-center">
        <button class="border-2 border-white p-3" @click="handleClick">Mudar volume</button>
    </div> 
</template>