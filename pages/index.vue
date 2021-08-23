<template>
  <div>
    <div class="bg-gray-50 min-h-screen py-12">
      <h1 class="text-center text-5xl py-6 font-semibold font-mono">
        Better Sleeps Better Lifes
      </h1>
      <section>
        <div class="max-w-screen-lg mx-auto">
          <div class="container mx-auto py-12">
            <div
              class="
                sounds__container
                grid
                sm:grid-cols-2
                lg:grid-cols-3
                justify-items-center
                gap-12
              "
            >
              <div
                v-for="(sound, i) in sounds"
                :key="i"
                class="
                  cursor-pointer
                  relative
                  grid
                  justify-items-center
                  bg-white
                  px-12
                  py-6
                  rounded-lg
                  shadow-lg
                "
                @click="onSoundClick(sound)"
              >
                <div
                  :ref="sound.style.ref"
                  class="
                    anim__container
                    rounded-full
                    grid
                    justify-items-center
                    align-items-center
                    h-48
                    w-48
                    relative
                  "
                  :class="sound.style.name"
                >
                  <transition name="fade" class="relative">
                    <!-- TODO configure lottie to display anime bigger -->
                    <div
                      v-if="sound === currentSound"
                      class="anim__place"
                    ></div>
                    <img
                      v-else
                      src="~/assets/images/anim_placeholder.png"
                      class="absolute h-32 w-32 top-[32px]"
                      alt="anim__placeholder"
                    />
                  </transition>
                </div>
                <p class="font-semibold mt-4">{{ sound.title }}</p>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import lottie from 'lottie-web'

import animations from '~/assets/animations.js'

import {
  ocean,
  oceanRain,
  oceanWaves,
  campfire,
  campfireCracking,
  campfireFlaming,
} from '~/assets/audio/index'

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
  beforeDestroy() {
    // this.currentSound.audio.stop()
  },

  methods: {
    /* settime out's for fade animetion on  */

    playSound(audio) {
      setTimeout(() => {
        audio.setAttribute('loop', 'true')
        audio.play()
      }, 500)
    },

    addAnimation(style) {
      /* give vue time to update dom cause you're using refs vith v-if  */
      // setTimeout(() => {
      //   lottie.loadAnimation({
      //     container: this.$refs[style.ref][0].querySelector('.anim__place'),
      //     renderer: 'svg',
      //     // name: "anim-name",
      //     loop: true,
      //     autoplay: true,
      //     animationData: animations[style.name],
      //   })
      // }, 600)

      /* TODO use next-tick instead of settimout but you should remove mode="out-in" transition cause app crashes when it's applied */
      this.$nextTick(() => {
        if (this.$refs[style.ref][0].querySelector('.anim__place')) {
          setTimeout(() => {
            lottie.loadAnimation({
              container: this.$refs[style.ref][0].querySelector('.anim__place'),
              renderer: 'svg',
              // name: "anim-name",
              loop: true,
              autoplay: true,
              animationData: animations[style.name],
            })
          }, 500)
        } else {
          this.addAnimation(style)
        }
      })
    },

    removeAnimation() {
      /* you can pass first attribute as name to destroy specific animation  */
      lottie.destroy()
    },

    onSoundClick(sound) {
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
