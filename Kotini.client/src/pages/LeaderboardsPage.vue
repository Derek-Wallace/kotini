<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-10 mx-lg-auto my-5 text-center mx-0">
        <div class="navbar-item">
          LEADERBOARDS
        </div>
      </div>
    </div>
    <div class="row d-flex justify-content-around">
      <div class="col-lg-3 mx-3 my-2 border-oj p-lg-5">
        <div class="row">
          <div class="col-12 mx-auto my-2 orange-words text-center played-header">
            GAMES PLAYED
          </div>
        </div>
        <div class="row" v-for="l in leaders" :key="l.id">
          <div v-if="account.id === l.id" class="col-12 d-flex justify-content-between user-text align-items-center orange-words my-3 border-left-0 border-right-0 border-top-0">
            <h4 class="m-0">
              <img :src="l.picture" height="40" width="40" alt="Pic" class="rounded-circle winner-glow" /> {{ l.name.substring(0,15) }}
            </h4>
            <p class="m-0">
              <b>{{ l.gamesPlayed }}</b>
            </p>
          </div>
          <div v-else class="col-12 d-flex justify-content-between align-items-center orange-words my-3">
            <h4 class="m-0">
              <img :src="l.picture" height="40" width="40" alt="Pic" class="rounded-circle" /> {{ l.name.substring(0,15) }}
            </h4>
            <p class="m-0">
              <b>{{ l.gamesPlayed }}</b>
            </p>
          </div>
        </div>
      </div>
      <div class="col-lg-3 mx-3 my-2 border-oj p-lg-5">
        <div class="row">
          <div class="col-12 mx-auto my-2 orange-words text-center played-header">
            GAMES WON
          </div>
        </div>
        <div class="row" v-for="w in winLeaders" :key="w.id">
          <div v-if="account.id === w.id" class="col-12 d-flex justify-content-between user-text align-items-center orange-words my-3 border-left-0 border-right-0 border-top-0">
            <h4 class="m-0">
              <img :src="w.picture" height="40" width="40" alt="Pic" class="rounded-circle winner-glow" /> {{ w.name.substring(0,15) }}
            </h4>
            <p class="m-0">
              <b>{{ w.gamesWon }}</b>
            </p>
          </div>
          <div v-else class="col-12 d-flex justify-content-between align-items-center orange-words my-3">
            <h4 class="m-0">
              <img :src="w.picture" height="40" width="40" alt="Pic" class="rounded-circle" /> {{ w.name.substring(0,15) }}
            </h4>
            <p class="m-0">
              <b>{{ w.gamesWon }}</b>
            </p>
          </div>
        </div>
      </div>
      <div class="col-lg-3 mx-3 my-2 border-oj p-lg-5">
        <div class="row">
          <div class="col-12 mx-auto my-2 orange-words text-center played-header">
            FASTEST TIME
          </div>
        </div>
        <div class="row" v-for="t in timeLeaders" :key="t.id">
          <div v-if="account.id === t.id" class="col-12 d-flex justify-content-between user-text align-items-center orange-words my-3 border-left-0 border-right-0 border-top-0">
            <h4 class="m-0">
              <img :src="t.picture" height="40" width="40" alt="Pic" class="rounded-circle winner-glow" /> {{ t.name.substring(0,15) }}
            </h4>
            <p class="m-0">
              <b>{{ t.fastestScore }}ms</b>
            </p>
          </div>
          <div v-else class="col-12 d-flex justify-content-between align-items-center orange-words my-3">
            <h4 class="m-0">
              <img :src="t.picture" height="40" width="40" alt="Pic" class="rounded-circle" /> {{ t.name.substring(0,15) }}
            </h4>
            <p class="m-0">
              <b>{{ t.fastestScore }}ms</b>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, onMounted } from '@vue/runtime-core'
import { AppState } from '../AppState'
import Notification from '../utils/Notification'
import { leaderboardService } from '../services/LeaderboardService'
import { accountService } from '../services/AccountService'

export default {
  setup() {
    onMounted(async() => {
      try {
        await leaderboardService.getLeaders()
        await leaderboardService.getTimeLeaders()
        await leaderboardService.getWinLeaders()
        await accountService.clearSession(AppState.account.id)
        AppState.currentGame = {}
      } catch (error) {
        Notification.toast(error, 'error')
      }
    })
    return {
      leaders: computed(() => AppState.leaders),
      timeLeaders: computed(() => AppState.timeLeaders),
      winLeaders: computed(() => AppState.winLeaders),
      account: computed(() => AppState.account)
    }
  }

}
</script>

<style scoped>
.navbar-item {
  font-family: 'Nunito', sans-serif;
  color: #ff9e00;
  text-shadow:
    0 0 .125em hsla(0, 0%, 100%, 0.3),
    0 0 .45em #ff9e00;
    font-size: 5vh;
}

.played-header {

  font-size: 4vh;
}

.orange-words {
  font-family: 'Nunito', sans-serif;
  color: #ff9e00;
  text-shadow:
    0 0 .125em hsla(0, 0%, 100%, 0.3),
    0 0 .45em #ff9e00;
}

.border-oj {
border: #ff9e00 solid 2px;
box-shadow:
    0 0 .125em hsla(0, 0%, 100%, 0.3),
    0 0 .45em #ff9e00;
}

@keyframes border-pulsate {
    0%   { border-color: #ff9e00; }
    50% { border-color: rgba(0, 255, 255, 0); }
    100%   { border-color: #ff9e00; }
}

.border-oj {
    display: block;
    margin: 5px auto;
    border: .5vh solid #ff9e00;
    animation: border-pulsate 5s infinite;
}

/* @keyframes text-pulsate {
    0%   { text-shadow:0 0 1em #9d4edd; }
    50% { text-shadow:0 0 .1em hsla(0, 0%, 100%, 0.39); }
    100%   { text-shadow: 0 0 1em #9d4edd; }
} */

.user-text {
  background-color: #6d23b1;
  border-radius: 25px;
  box-shadow: 0 0 0.125em hsl(0deg 0% 100% / 30%), 0 0 0.45em #6d23b1;
}

.winner-glow {
border: #ff9e00 2px solid;
}

</style>
