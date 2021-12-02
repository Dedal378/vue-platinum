<template>
  <div class="container">
    <button v-show="!game" @click="play" class="btn-play">
      <svg viewBox="0 0 1000 1000" xmlns="http://www.w3.org/2000/svg">
        <path d="M144.6 960.5V39.6c0-11.8 9.4-22.5 23.7-27.2 14.4-4.7 31-2.4 42.3 5.8l633.2 460.3c15.3 11.1 15.3 31.5 0 42.7l-630.1 458c-7.1 6.5-18 10.8-30.1 10.8-21.5 0-39-13.2-39-29.5z" />
      </svg>
    </button>

    <transition name="fade">
      <div v-show="game" class="game-container">
        <img alt="close" class="icon-close" src="@/assets/no.png">
        <div class="header">Выбери правильный бак для сортировки мусора!</div>
        <main>

        </main>
      </div>
    </transition>

  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      game: false,
    }
  },
  methods: {
    play() {
      this.game = !this.game
    }
  },
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  color: #2c3e50;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: 14px;
  text-align: center;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

button {
  display: flex;
  background: white;
  border: none;
  outline: none;
  cursor: pointer;
  place-content: center;
}

.container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  place-content: center;


  .game-container {
    position: relative;
    display: flex;
    flex-direction: column;
    min-width: 640px;
    min-height: 500px;
    border-radius: 20px;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.5);
    place-self: center;

    .icon-close {
      position: absolute;
      right: 15px;
      top: 15px;
      width: 17px;
      filter: brightness(80%);
      cursor: pointer;

      &:hover {
        transform: scale(1.5);
        transition: all 0.5s ease;
      }
    }

    .header {
      max-width: 60%;
      padding: 10px;
      font-size: 20px;
      font-weight: bolder;
      place-self: center;
    }
  }

  .btn-play {
    min-width: 80px;
    min-height: 50px;
    border-radius: 5px;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.25);
    place-self: center;

    &:hover {
      transition: all 0.5s ease;
      transform: scale(1.05);
      box-shadow: 0 2px 12px rgba(0, 0, 0, 0.35);

      svg {
        transition: all 1s ease;
        fill: #888;
      }
    }

    svg {
      width: 20px;
      height: 100%;
      fill: #696969;
    }
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

</style>
