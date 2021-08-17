<template>
  <div>
    <p>sounds</p>
    <p @click="stopSound">stop sound</p>
    <div>
      <div
        v-for="(sound, i) in audios"
        :key="i"
        :ref="sound.title"
        @click="playSound(sound.src)"
      >
        <p>{{ sound.title }}</p>
      </div>
    </div>
    <!-- <audio v-for="(val, prop) in audiosController" :key="prop" controls loop>
      <source :src="val" type="audio/mp3" @click="play(val)" />
      Your browser does not support the audio element.
    </audio> -->

    <div class="anime__container">
      <button @click="stop">stop</button>
      <button @click="pause">pause</button>
      <button @click="play">play</button>
      <button style="width: 100px; height: 100px" @click="addAnimation">
        add animation
      </button>
    </div>
  </div>
</template>

<script>
/* eslint-disable-next-line */
import lottie from 'lottie-web'

import animations from '~/assets/animations.js'
import ocean from '~/assets/audio/ocean.mp3'
import oceanWavesWithRain from '~/assets/audio/ocean-waves-with-rain.mp3'
import smallOceanWaves from '~/assets/audio/small-ocean-waves.mp3'

export default {
  data() {
    return {
      currentSound: null,
      isPlaying: false,
      // audiosController: {
      //   ocean: require('@/assets/audio/ocean.mp3').default,
      // },
      audios: [
        {
          title: 'Heavy Ocean Waves',
          src: new Audio(ocean),
        },
        {
          title: 'Ocean Waves With Rain',
          src: new Audio(oceanWavesWithRain),
        },
        {
          title: 'Small Ocean Waves',
          src: new Audio(smallOceanWaves),
        },
      ],
    }
  },
  beforeDestroy() {
    this.currentSound.pause()
  },
  mounted() {
    console.log(this.$refs)
  },

  methods: {
    async playSound(sound) {
      try {
        /* if there is a sound playing stop it first. */
        if (this.currentSound) {
          this.currentSound.pause()
        }
        this.currentSound = sound
        this.isPlaying = true
        this.currentSound.setAttribute('loop', 'true')
        await this.currentSound.play()
      } catch (error) {
        console.log(error)
      }
    },
    stopSound() {
      this.currentSound.pause()
    },

    /* animation handlers */
    addAnimation(e) {
      this.anim = lottie.loadAnimation({
        container: e.target,
        renderer: 'svg',
        loop: true,
        autoplay: true,
        animationData: animations.playing,
      })
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

    /* add animation programaticly */
  },
}
</script>
