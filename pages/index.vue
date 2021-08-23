<template>
  <div>
    <p>sounds</p>
    <section class="bg-gray-50">
      <div class="max-w-screen-lg mx-auto">
        <div class="container mx-auto">
          <div
            class="
              sounds__container
              grid grid-cols-3
              justify-items-center
              gap-4
            "
          >
            <div
              v-for="(sound, i) in sounds"
              :key="i"
              class="cursor-pointer relative grid justify-items-center"
              @click="onSoundClick(sound)"
            >
              <div
                :ref="sound.style.ref"
                class="style-place w-32 h-32 absolute"
              ></div>

              <div class="h-32 w-32 rounded-full" :class="sound.style.name">
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
        <!-- TODO add fire sounds. -->
      </div>
    </section>
  </div>
</template>

<script>
import lottie from 'lottie-web'

import animations from '~/assets/animations.js'

/* mp3 sound files files */
import ocean from '~/assets/audio/ocean.mp3'
import oceanRain from '~/assets/audio/ocean_rain.mp3'
import oceanWaves from '~/assets/audio/ocean_waves.mp3'

import campfire from '~/assets/audio/campfire.mp3'
import campfireCracking from '~/assets/audio/campfire_cracking.mp3'
import campfireFlaming from '~/assets/audio/campfire_flaming.mp3'

export default {
  data() {
    return {
      currentSound: null,

      sounds: [
        {
          style: {
            ref: 'ocean',
            name: 'ocean',
          },
          title: 'Heavy Ocean Waves',
          audio: new Audio(ocean),
        },
        {
          style: {
            ref: 'oceanRain',
            name: 'ocean',
          },
          title: 'Ocean Waves With Rain',
          audio: new Audio(oceanRain),
        },
        {
          style: {
            ref: 'oceanWaves',
            name: 'ocean',
          },
          title: 'Small Ocean Waves',
          audio: new Audio(oceanWaves),
        },
        {
          style: {
            ref: 'campfire',
            name: 'campfire',
          },
          title: 'Campfire',
          audio: new Audio(campfire),
        },
        {
          style: {
            ref: 'campfireCracking',
            name: 'campfire',
          },
          title: 'Cracking Campfire',
          audio: new Audio(campfireCracking),
        },
        {
          style: {
            ref: 'campfireFlaming',
            name: 'campfire',
          },
          title: 'Flaming Campfire',
          audio: new Audio(campfireFlaming),
        },
      ],
    }
  },

  methods: {
    playSound(audio) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          audio.setAttribute('loop', 'true')
          audio.play()
          resolve('sound played')
        }, 500)
      })
    },

    addAnimation(style) {
      /* add settime out for transition */
      setTimeout(() => {
        lottie.loadAnimation({
          container: this.$refs[style.ref][0],
          renderer: 'svg',
          // name: "anim-name",
          loop: true,
          autoplay: true,
          animationData: animations[style.name],
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

        /* if playing sound clicked stop it */
        if (this.currentSound === sound) return (this.currentSound = null)
      }

      this.currentSound = sound
      this.playSound(sound.audio)
      this.addAnimation(sound.style)
    },
  },
}
</script>

<style lang="scss" scoped></style>
