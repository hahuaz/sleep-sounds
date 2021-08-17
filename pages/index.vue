<template>
  <div>
    <p>sounds</p>
    <section class="bg-blue-50">
      <div class="max-w-screen-lg mx-auto">
        <div class="container mx-auto">
          <div class="grid grid-flow-col gap-4">
            <div
              v-for="(sound, i) in sounds"
              :key="i"
              class="cursor-pointer bg-red-50 relative"
              @click="onSoundClick(sound)"
            >
              <div
                :ref="sound.animation.ref"
                class="animation-place w-32 h-32 absolute"
              ></div>

              <div class="w-32 h-32">
                <transition name="fade">
                  <img
                    v-if="sound !== currentSound"
                    class="w-32 h-32"
                    src="~/assets/sound-image.png"
                    alt="sound-image"
                  />
                </transition>
              </div>
              <p class="font-semibold">{{ sound.title }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import lottie from 'lottie-web'

import animations from '~/assets/animations.js'
import ocean from '~/assets/audio/ocean.mp3'
import oceanWavesWithRain from '~/assets/audio/ocean-waves-with-rain.mp3'
import smallOceanWaves from '~/assets/audio/small-ocean-waves.mp3'

export default {
  data() {
    return {
      anim: null,
      currentSound: null,

      sounds: [
        {
          animation: {
            ref: 'heavyOceanWaves',
            name: 'ocean',
          },
          title: 'Heavy Ocean Waves',
          audio: new Audio(ocean),
        },
        {
          animation: {
            ref: 'oceanWavesWithRain',
            name: 'ocean',
          },
          title: 'Ocean Waves With Rain',
          audio: new Audio(oceanWavesWithRain),
        },
        {
          animation: {
            ref: 'smallOceanWaves',
            name: 'ocean',
          },
          title: 'Small Ocean Waves',
          audio: new Audio(smallOceanWaves),
        },
      ],
    }
  },

  methods: {
    playSound(audio) {
      audio.setAttribute('loop', 'true')
      audio.play()
    },

    addAnimation(animation) {
      /* add settime out for transition */
      setTimeout(() => {
        lottie.loadAnimation({
          container: this.$refs[animation.ref][0],
          renderer: 'svg',
          // name: ref,
          loop: true,
          autoplay: true,
          animationData: animations[animation.name],
        })
      }, 500)
    },

    removeAnimation() {
      /* you can pass first attribute as name to destroy specific animation  */
      lottie.destroy()
    },

    onSoundClick(sound) {
      /* TODO when playing sound clicked stop it */
      if (this.currentSound) {
        /* pause previous sound */
        this.currentSound.audio.pause()
        /* destroy previous animation */
        this.removeAnimation()
      }

      this.currentSound = sound
      this.playSound(sound.audio)
      this.addAnimation(sound.animation)
    },
  },
}
</script>
