<template>
  <div class="wrapper">
    <v-toolbar dense dark color="black">
      <v-spacer></v-spacer>
      <v-app-bar-nav-icon @click="dialog = true"></v-app-bar-nav-icon>
    </v-toolbar>
    <div class="home text-center d-flex flex-column align-center justify-center">

      <v-btn
        @click="playSound"
        fab
        outlined
        color="white"
        height="250"
        width="250"
        class="mt-n12 mb-12"
      >
        <span class="capitalize display-3 font-weight-thin">zen</span>
      </v-btn>

      <v-btn
        small
        id="enough"
        class=""
        text
        color="white"
        @click="stopPlaying"
      >
        <span class="text-capitalize font-weight-light">enough</span>
      </v-btn>

        <countdown
          ref="countdown"
          :auto-start="false"
          @progress="handleCountdownProgress"
          class="subtitle-1 font-weight-light mt-5 white--text"
          :time="`${minutes}` * 60 * 1000"
        >
          <template slot-scope="props">{{ props.minutes }} minutes, {{ props.seconds }} seconds.</template>
        </countdown>


    </div>

    <v-dialog
      v-model="dialog"
      :overlay="false"
      max-width="300px"
      transition="dialog-transition"
    >
      <v-card>
        <v-card-title primary-title>
          <span class="font-weight-light">Settings</span>
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col cols="6">
              <span>Meditate</span>
              <v-text-field
                color="black'"
                name="minutes"
                label="Minutes"
                id="id"
                v-model="minutes"
                type="number"
              ></v-text-field>
            </v-col>
            <v-col cols="6">
              <span>Chime interval</span>
              <v-text-field
                color="black'"
                name="minutes"
                label="Seconds"
                id="id"
                v-model="chimeIntervalSeconds"
                type="number"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn outlined @click="dialog = false" color="black">save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>

</template>

<script>

export default {
  name: 'Home',
  data() {
    return {
      dialog: false,
      minutes: 10,
      chimeIntervalSeconds: 60
    }
  },
  methods: {
    playSound () {
        this.$refs.countdown.start();
        let audio = new Audio(require('../assets/zen_chime.wav'))
        audio.play()

        this.playing = setInterval( () => {
          audio.play()
        }, this.chimeSecondsToMilliseconds)
    },
    stopPlaying() {
      let audio = new Audio(require('../assets/gong.wav'))
      audio.play()

      this.$router.push('/about')
      clearInterval(this.playing)
      console.log('stop playing')
    },
    handleCountdownProgress(data) {
      // console.log(data.totalMinutes);
      console.log(data.totalMilliseconds);
      if(data.totalMilliseconds === 1000) {
        this.stopPlaying()
        console.log('times up')
      }
    }
  },
  computed: {
    chimeSecondsToMilliseconds() {
      let minutes = this.chimeIntervalSeconds
      return minutes * 1000
    }
  }
}
</script>

<style scoped>
.wrapper {
  height: 100%;
}
  .home {
    height: 100%;
    background: black;
    position: relative;
  }
  #enough {
    position: absolute;
    bottom: 60px;
  }
</style>
