<template>
  <main class="l-timer-page">
    <div class="memo">
      <v-text-field
        label="Временные заметки"
        textarea
        rows="4"
        auto-grow
        counter=4000
        v-model="memo"
      ></v-text-field>
    </div>
    <div class="l-timer">
      <h2 class="red--text">
        {{ alert }}
      </h2>

      <div>
        work is on: <h3>{{ timer() }}</h3>
      </div>
      <div v-show="isOn === false">
        <span class="grey--text">work is off</span>: <h4>{{ idleTimer() }}</h4>
      </div>

      <v-text-field v-model="timerText"></v-text-field>
      <v-btn
        block
        :color="isOn ? 'light-blue lighten-1' : 'green lighten-1'"
        @click.prevent="toggleTimer()"
      >
        {{ isOn ? "Стоп" : "Старт" }}
      </v-btn>

      <ul
        v-for="elapsed in elapsedTimers"
        :key="elapsed.key"
      >
        <li>
          <v-btn @click.prevent="deleteElapsed(elapsed)">
            X
          </v-btn>

          {{ elapsed.timer }}
          <span class="grey--text">{{ elapsed.end }}</span>
          <span class="success--text">{{ elapsed.text }}</span>
        </li>
      </ul>
    </div>
  </main>
</template>

<script>
  export default {
    name: 'Timer',
    data () {
      return {
        memo: '',
        idleSeconds: 0,
        alert: '',
        timerText: '',
        seconds: 0,
        isOn: false,
        elapsedTimers: []
      }
    },
    created () {
      const key = this.getKey()
      const value = localStorage.getItem(key)
      this.elapsedTimers = JSON.parse(value)
      if (this.elapsedTimers === null) {
        this.elapsedTimers = []
      }

      this.memo = localStorage.getItem('app-notes-memo')
      this.$watch('memo', (val) => {
        localStorage.setItem('app-notes-memo', val)
      })

      setInterval(() => {
        if (this.isOn) {
          this.seconds++
          if (this.seconds > 1150) {
            this.alert = 'Делай паузу, уже почти 20 минут работы'
          }
        } else {
          this.idleSeconds++
        }
      }, 1000)
    },
    methods: {
      getKey () {
        let key = new Date().toDateString()
        key = String(key)
        key = key.split(' ').join('-')
        return key
      },

      convertSecondsToTimeString (seconds) {
        const min = Math.floor(seconds / 60)
        const sec = seconds - min * 60

        const m = min > 9 ? String(min) : '0' + String(min)
        const s = sec > 9 ? String(sec) : '0' + String(sec)
        return [m, ':', s].join('')
      },

      timer () {
        return this.convertSecondsToTimeString(this.seconds)
      },

      idleTimer () {
        return this.convertSecondsToTimeString(this.idleSeconds)
      },

      stopTimer () {
        this.elapsedTimers.push({
          key: new Date().getTime() / 1000,
          end: new Date().toLocaleTimeString(),
          timer: this.timer(),
          text: this.timerText
        })
        this.isOn = false
        this.alert = ''
        this.seconds = 0

        const key = this.getKey()
        localStorage.setItem(
          key,
          JSON.stringify(this.elapsedTimers)
        )
      },
      startTimer () {
        this.isOn = true
        this.idleSeconds = 0
      },
      toggleTimer () {
        if (this.isOn) {
          this.stopTimer()
        } else {
          this.startTimer()
        }
      },

      deleteElapsed (elapsed) {
        const ind = this.elapsedTimers.indexOf(elapsed)
        if (ind > -1) {
          this.elapsedTimers.splice(ind, 1)
        }
      }
    }
  }
</script>

<style scoped>

</style>
