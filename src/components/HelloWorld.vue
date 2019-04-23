<template>
  <div class="container d-flex justify-content-center">

    <form class="border p-25 w-75 rounded" v-if="view=='doYouWantToPlay'" >
      <div class="form-group">
        <h2 class="mb-5">¿Quieres jugar a adivinar el numero random?</h2>
      </div>
      <div class="form-group form-check ">
        <button type="submit" class="btn btn-primary w-50" v-on:click="changeView('insertNameUser')">Iniciar Juego</button>
      </div>
    </form>

  <div class="atom-spinner" v-if="showSpinner">
  <div class="spinner-inner">
    <div class="spinner-line"></div>
    <div class="spinner-line"></div>
    <div class="spinner-line"></div>
    <div class="spinner-circle">
      &#9679;
    </div>
  </div>
</div>

    <form class="border p-5 w-50 rounded" v-if="view == 'insertNameUser'" >
      <div class="form-group">
        <h5 class="mb-5">¿Cúal es tu nombre?</h5>
        <input type="text" v-model="playerName" class="form-control" placeholder="Ingresa tu nombre" required>
      </div>
      <div class="form-group form-check ">
        <button @click="changeView('guessTheNumber')" class="btn btn-success w-25 mr-5">Aceptar</button>
        <button @click="changeView('doYouWantToPlay')" type="submit" class="btn btn-danger w-25">Cancelar</button>
      </div>
    </form>

    <form class="border p-5 w-50 rounded" v-if="view == 'guessTheNumber'" >
      <div class="form-group">
        <h5 class="mb-5">¿Adivina qué número estoy pensando?</h5>
        <input type="number"  v-model="numberEnteredByTheUser" class="form-control" placeholder="Ingresa número" required>
      </div>
      <div class="form-group form-check">
        <button type="submit" @click="guessTheNumber" v-if="!endOfTheGame" class="btn btn-primary">Adivinar</button>
        <button type="submit" @click="restartTheGame" v-if="endOfTheGame" class="btn btn-info">Reiniciar Juego</button>

      </div>
    </form>

  </div>

</template>

<script>
export default {
  el: 'guessTheNumber',
  data () {
    return {
      view: 'doYouWantToPlay',
      showSpinner: false,
      playerName: '',
      numberEnteredByTheUser: 0,
      numberRandom: 0,
      numberOfAttemps: 0,
      endOfTheGame: false
    }
  },
  methods: {
    getNumberRandom: function () {
      this.numberRandom = Math.floor(Math.random() * 20)
      setTimeout(() => (this.showSpinner = false), 500)
    },
    changeView: function (view) {
      if (view === 'insertNameUser') {
        this.showSpinner = true
        this.getNumberRandom()
      }
      this.view = view
    },
    guessTheNumber: function () {
      if (this.numberRandom < Number(this.numberEnteredByTheUser)) {
        this.numberOfAttemps++
        this.$swal.fire({
          type: 'error',
          title: 'Cerca',
          text: `Estoy pensando un número más bajo. ${this.playerName}`
        })
        this.numberOfAttemps++
      } else if (this.numberRandom > Number(this.numberEnteredByTheUser)) {
        this.$swal.fire({
          type: 'error',
          title: 'Cerca',
          text: `Estoy pensando un número más alto ${this.playerName}.`
        })
        this.numberOfAttemps++
      } else if (this.numberRandom === Number(this.numberEnteredByTheUser)) {
        this.$swal(`¡Felicidades ${this.playerName}!`, `Lo has conseguido en ${this.numberOfAttemps} intentos`, `success`)
        this.endOfTheGame = true
      }
    },
    restartTheGame: function () {
      this.getNumberRandom()
      this.playerName = ''
      this.view = 'doYouWantToPlay'
      this.numberOfAttemps = 0
      this.numberEnteredByTheUser = 0
      this.endOfTheGame = false
    }
  }
}
</script>

<style scoped>

.atom-spinner, .atom-spinner * {
      box-sizing: border-box;
      position: relative;
      left: 27%;
      margin-top: 8%;
    }

    .atom-spinner {
      height: 60px;
      width: 60px;
      overflow: hidden;
    }

    .atom-spinner .spinner-inner {
      position: relative;
      display: block;
      height: 100%;
      width: 100%;
    }

    .atom-spinner .spinner-circle {
      display: block;
      position: absolute;
      color: #ff1d5e;
      font-size: calc(60px * 0.24);
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }

    .atom-spinner .spinner-line {
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      animation-duration: 1s;
      border-left-width: calc(60px / 25);
      border-top-width: calc(60px / 25);
      border-left-color: #ff1d5e;
      border-left-style: solid;
      border-top-style: solid;
      border-top-color: transparent;
    }

    .atom-spinner .spinner-line:nth-child(1) {
      animation: atom-spinner-animation-1 1s linear infinite;
      transform: rotateZ(120deg) rotateX(66deg) rotateZ(0deg);
    }

    .atom-spinner .spinner-line:nth-child(2) {
      animation: atom-spinner-animation-2 1s linear infinite;
      transform: rotateZ(240deg) rotateX(66deg) rotateZ(0deg);
    }

    .atom-spinner .spinner-line:nth-child(3) {
      animation: atom-spinner-animation-3 1s linear infinite;
      transform: rotateZ(360deg) rotateX(66deg) rotateZ(0deg);
    }

    @keyframes atom-spinner-animation-1 {
      100% {
        transform: rotateZ(120deg) rotateX(66deg) rotateZ(360deg);
      }
    }

    @keyframes atom-spinner-animation-2 {
      100% {
        transform: rotateZ(240deg) rotateX(66deg) rotateZ(360deg);
      }
    }

    @keyframes atom-spinner-animation-3 {
      100% {
        transform: rotateZ(360deg) rotateX(66deg) rotateZ(360deg);
      }
    }
</style>
