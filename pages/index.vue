<template>
  <div>
    <p>sounds</p>
    <p @click="pauseSound">pause</p>
    <div>
      <div v-for="(sound, i) in audios" :key="i" @click="playSound(sound.src)">
        <p>{{ sound.title }}</p>
      </div>
    </div>
    <!-- <audio v-for="(val, prop) in audiosController" :key="prop" controls loop>
      <source :src="val" type="audio/mp3" @click="play(val)" />
      Your browser does not support the audio element.
    </audio> -->

    <div class="anime__container">
      <lottie
        :options="defaultOptions"
        :height="400"
        :width="400"
        @animCreated="handleAnimation"
      />
      <div>
        <p>Speed: x{{ animationSpeed }}</p>
        <input
          v-model="animationSpeed"
          type="range"
          value="1"
          min="0"
          max="3"
          step="0.5"
          @change="onSpeedChange"
        />
      </div>
      <button @click="stop">stop</button>
      <button @click="pause">pause</button>
      <button @click="play">play</button>
    </div>
  </div>
</template>

<script>
import animations from '~/assets/animations.js'
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
      defaultOptions: { animationData: animations.animetion1 },
      animationSpeed: 1,
    }
  },
  beforeDestroy() {
    this.currentSound.pause()
  },

  methods: {
    async playSound(sound) {
      try {
        this.currentSound = sound
        this.currentSound.setAttribute('loop', 'true')
        await this.currentSound.play()
      } catch (error) {
        console.log(error)
      }
    },
    pauseSound() {
      this.currentSound.pause()
    },

    /* animation handlers */
    handleAnimation(anim) {
      this.anim = anim
    },

    stop() {
      this.anim.stop()
    },

    play() {
      this.anim.play()
    },

    pause() {
      this.anim.pause()
    },

    onSpeedChange() {
      this.anim.setSpeed(this.animationSpeed)
    },
  },
}
</script>
