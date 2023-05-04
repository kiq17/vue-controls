<script setup lang="ts">
import { useMediaControls } from "@vueuse/core";
import { ref } from "vue"
import Location from "./components/Location.vue";
import Slider from "./components/Slider.vue";
import Random from "./components/Random.vue";
import Radio from "./components/Radio.vue";
import Spin from "./components/Spin.vue";


const video = ref<HTMLVideoElement | null>(null)

const { volume } = useMediaControls(video)

const handleVolumeChange = (value: number)=>{
  volume.value = value
}

</script>

<template>
  <section class="flex justify-between gap-5 items-center p-16">
    <div class="lg:w-1/3 flex-1 self-start">
      <video class="w-full" src="./assets/videoTest.mp4" ref="video" controls muted></video>
      <p>Volume: {{ Math.round(volume * 100) }}</p>
    </div>
    <div class="flex flex-col gap-40 justify-center flex-1">
      <Location @volume-change="handleVolumeChange"/>
      <Slider :volume="volume" @volume-change="handleVolumeChange"/>
      <Random @volume-change="handleVolumeChange"/>
      <Radio :volume="volume" @volume-change="handleVolumeChange"/>
      <Spin @volume-change="handleVolumeChange"/>
    </div>
  </section>
</template>