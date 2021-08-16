<template>
  <div>
    <p>sounds</p>
    <p @click="pause">pause</p>
    <div>
      <div v-for="(sound, i) in audios" :key="i" @click="play(sound.src)">
        <p>{{ sound.title }}</p>
      </div>
    </div>
    <!-- <audio v-for="(val, prop) in audiosController" :key="prop" controls loop>
      <source :src="val" type="audio/mp3" @click="play(val)" />
      Your browser does not support the audio element.
    </audio> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentSound: null,
      isPlaying: null,
      // audiosController: {
      //   ocean: require('@/assets/audio/ocean.mp3').default,
      // },
      audios: [
        {
          title: 'Heavy Ocean Waves',
          src: new Audio(require('@/assets/audio/ocean.mp3').default),
        },
        {
          title: 'Ocean Waves With Rain',
          src: new Audio(
            require('@/assets/audio/ocean-waves-with-rain.mp3').default
          ),
        },
        {
          title: 'Small Ocean Waves',
          src: new Audio(
            require('@/assets/audio/small-ocean-waves.mp3').default
          ),
        },
      ],
    }
  },
  beforeDestroy() {
    this.currentSound.pause()
  },

  methods: {
    async play(sound) {
      try {
        this.currentSound = sound
        this.currentSound.setAttribute('loop', 'true')
        await this.currentSound.play()
      } catch (error) {
        console.log(error)
      }
    },
    pause() {
      this.currentSound.pause()
    },
  },
}
</script>
