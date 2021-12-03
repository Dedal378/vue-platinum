<template>
  <div class="container">
    <button v-show="false" @click="play" class="btn-play">
      <svg viewBox="0 0 1000 1000" xmlns="http://www.w3.org/2000/svg">
        <path d="M144.6 960.5V39.6c0-11.8 9.4-22.5 23.7-27.2 14.4-4.7 31-2.4 42.3 5.8l633.2 460.3c15.3 11.1 15.3 31.5 0 42.7l-630.1 458c-7.1 6.5-18 10.8-30.1 10.8-21.5 0-39-13.2-39-29.5z" />
      </svg>
    </button>

    <transition name="fade">
      <section v-show="true" class="game-container">
        <img @click="close" alt="close" class="icon-close" src="@/assets/no.png">
        <header class="header">Выбери правильный бак для сортировки мусора!</header>

        <div class="main">
          <div class="counter right">
            <img alt="right" src="@/assets/ok.png">
            <span>0</span>
          </div>

          <div class="trash">
            <span>Начать!</span>
            <svg class="timer-svg" viewBox="-40 -40 250.79 250.79" xmlns="http://www.w3.org/2000/svg">
              <g data-name="Layer 2">
                <g data-name="Layer 1">
                  <circle class="cls-1 js-preload-circle" cx="85.89" cy="85.89" r="84.89"
                          style="stroke-dashoffset: 600;" />
                  <circle class="cls-1" cx="85.89" cy="85.89" r="84.89"
                          stroke-opacity="0.55" />
                </g>
              </g>
            </svg>
          </div>

          <div class="counter wrong">
            <img alt="wrong" src="@/assets/no.png">
            <span>0</span>
          </div>
        </div>

        <div class="boxes">
          <div class="box-item">
            <span class="yellow">Вторсырьё</span>
            <img alt="box" class="box-cap" src="@/assets/bucks/buck_top/wastetop_yellow.min.png">
            <img alt="box" class="box" src="@/assets/bucks/wastebox_yellow.min.png">
          </div>
          <div class="box-item">
            <span class="green">Смешанные</span>
            <img alt="box" class="box-cap" src="@/assets/bucks/buck_top/wastetop_green.min.png">
            <img alt="box" class="box" src="@/assets/bucks/wastebox_green.min.png">
          </div>
          <div class="box-item">
            <span class="blue">Бытовые</span>
            <img alt="box" class="box-cap" src="@/assets/bucks/buck_top/wastetop_blue.min.png">
            <img alt="box" class="box" src="@/assets/bucks/wastebox_blue.min.png">
          </div>
          <div class="box-item">
            <span class="orange">Опасные</span>
            <img alt="box" class="box-cap" src="@/assets/bucks/buck_top/wastetop_orange.min.png">
            <img alt="box" class="box" src="@/assets/bucks/wastebox_orange.min.png">
          </div>
        </div>
      </section>
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
    },
    close() {
      this.game = false
    },
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

img {
  width: 100%;
  height: auto;
}

.container {
  display: flex;
  flex-direction: column;
  max-width: 1400px;
  height: 100vh;
  margin: 0 auto;
  place-content: center;

  .game-container {
    position: relative;
    display: flex;
    flex-direction: column;
    height: 80%;
    padding: 10px 20px;
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.5);
    place-self: center;

    .icon-close {
      position: absolute;
      top: 15px;
      right: 15px;
      width: 17px;
      cursor: pointer;
      filter: brightness(80%);

      &:hover {
        transition: all 0.5s ease;
        transform: scale(1.5);
      }
    }

    .header {
      max-width: 60%;
      padding: 10px;
      font-size: 20px;
      font-weight: bolder;
      place-self: center;
    }

    .main {
      display: flex;
      justify-content: space-between;

      .trash {
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;

        span {
          position: absolute;
          color: #fff;
          font-size: 42px;
          font-weight: bolder;
          cursor: pointer;
        }

        & .timer-svg {
          max-width: 300px;
          fill: #aa5ce1;
        }
      }

      .counter {
        display: flex;
        justify-content: space-between;
        align-items: center;
        width: 60px;
        padding: 7px 14px;
        border-radius: 25px;
        place-self: center;

        img {
          width: 20px;
          height: 100%;
        }

        span {
          color: #fff;
          font-size: 24px;
          font-weight: bolder;
        }

        &.right {
          background: #add171;
        }

        &.wrong {
          background: #e3218b;
        }
      }
    }

    .boxes {
      display: flex;
      justify-content: center;

      .box-item {
        position: relative;
        display: flex;
        flex-direction: column;
        align-items: center;
        width: 120px;

        span {
          position: relative;
          padding: 5px 12px;
          margin-bottom: 10px;
          color: #fff;
          border-radius: 25px;
          z-index: auto;

          &.yellow {
            background: #f7a600;
          }

          &.green {
            background: #68bb00;
          }

          &.blue {
            background: #0059ed;
          }

          &.orange {
            background: #f81d0f;
          }
        }

        .box-cap {
          position: absolute;
          display: none;
          width: 100px;
        }

        .box {
          position: relative;
          z-index: 10;
        }
      }
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

.fade-enter-active {
  transition: all 1.5s ease;
  transform: scale(1);
}

.fade-enter-from,
.fade-leave-to {
  transform: scale(0.8);
  opacity: 0;
}

</style>
