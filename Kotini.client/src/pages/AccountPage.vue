<template>
  <div v-if="account.name" class="container">
    <div class="row text-center my-4">
      <div class="col-12 player-header">
        {{ account.name.toUpperCase() }}
      </div>
    </div>
    <div class="row my-4">
      <div class="col-md-3 text-center">
        <img class="rounded-circle picture-glow" :src="account.picture" alt="" />
      </div>
      <div class="col-md-5 align-items-center mb-5 mt-2 pt-lg-5">
        <div class="about">
          <div>BEST TIME: {{ account.fastestScore }}ms</div>
          <div>GAMES WON: {{ account.gamesWon }}</div>
          <div>GAMES PLAYED: {{ account.gamesPlayed }}</div>
        </div>
      </div>
      <div class="col-md-3 ml-auto d-flex justify-content-end align-items-end mt-5 pt-5">
        <button class="edit-button" data-toggle="modal" data-target="#editModal">
          EDIT PROFILE
        </button>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-10 mx-auto picture-glow box">
        <div class="row">
          <Achievements />
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col my-4 player-header text-center">
        <p>Win History</p>
      </div>
    </div>
    <div class="row">
      <GameCard v-for="win in wins" :key="win.id" :win="win" />
    </div>
  </div>
  <EditProfileModal v-if="account.id" />
</template>

<script>
import { computed, onMounted, reactive, ref } from 'vue'
import { AppState } from '../AppState'
import { AuthService } from '../services/AuthService'
import { accountService } from '../services/AccountService'
import { gameService } from '../services/GameService'

export default {
  name: 'Account',
  setup() {
    onMounted(async() => {
      try {
        await gameService.getWins()
        await accountService.clearSession(AppState.account.id)
        AppState.currentGame = {}
      } catch (error) {
        Notification.toast(error)
      }
    })
    const state = reactive({
      newInfo: { name: AppState.account.name, picture: AppState.account.picture }
    })
    const showEditForm = ref(false)
    return {
      wins: computed(() => AppState.wins),
      state,
      showEditForm,

      account: computed(() => AppState.account),
      async logout() {
        AuthService.logout({ returnTo: window.location.origin })
      },
      async editProfile(id) {
        await accountService.editProfile(id, state.newInfo)
        state.newInfo = { name: AppState.account.name, picture: AppState.account.picture }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.edit-button {
  font-family: 'Nunito', sans-serif;
  color: #ff9e00;
  text-shadow:
    0 0 .125em hsla(0, 0%, 100%, 0.3),
    0 0 .45em #ff9e00;
  font-size: 1.25em;
  background: none;
  border: none;
}

.player-header {
  font-size: 4vh;
  color: #ff9e00;
  text-shadow:
    0 0 .125em hsla(0, 0%, 100%, 0.3),
    0 0 .45em #ff9e00;
}

.picture-glow{
  border: 2px solid #ff9e00;
    box-shadow:
    0 0 .125em hsla(0, 0%, 100%, 0.3),
    0 0 .45em #ff9e00;
}

.about {
font-size: 2.5vh;
   font-family: 'Nunito', sans-serif;
   color: #ff9e00;
   text-shadow:
    0 0 .125em hsla(0, 0%, 100%, 0.3),
    0 0 .45em #ff9e00;
  }

.name {
    font-family: 'Nunito', sans-serif;
  font-family: 'Nunito', sans-serif;

  color: #ff9e00;
  font-size: 20px;
}

img {
  height: 200px;
  max-width: 200px;
}

@media only screen and (max-width: 900px) {
  .about {
text-align: center;
  }
}

.box{
  height: 50%;
}

</style>
