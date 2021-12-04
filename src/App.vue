<template>
  <div class="container">
    <button v-show="!playGame" @click="playGame = true" class="btn-play">
      <svg viewBox="0 0 1000 1000" xmlns="http://www.w3.org/2000/svg">
        <path d="M144.6 960.5V39.6c0-11.8 9.4-22.5 23.7-27.2 14.4-4.7 31-2.4 42.3 5.8l633.2 460.3c15.3 11.1 15.3 31.5 0 42.7l-630.1 458c-7.1 6.5-18 10.8-30.1 10.8-21.5 0-39-13.2-39-29.5z" />
      </svg>
    </button>

    <transition name="fade">
      <section v-show="playGame" class="game-container">
        <div class="header-container">
          <img @click="close" alt="close window" class="icon-close" src="@/assets/no.png">
          <header class="header">{{ title }}</header>
        </div>

        <div class="main">
          <div v-if="!isPlaying" class="trash">
            <button @click="startGame" class="btn-start">Начать!</button>
            <svg class="timer-svg" id="timer" viewBox="-40 -40 250.79 250.79" xmlns="http://www.w3.org/2000/svg">
              <circle cx="85.89" cy="85.89" r="84.89" />
            </svg>
          </div>

          <div v-else class="trash ">
            <div class="counter right">
              <img alt="right" src="@/assets/ok.png">
              <span>{{ counterRight }}</span>
            </div>

            <div v-if="!isEndGame" class="trash">
              <div class="trash-items">{{ trash }}</div>
              <svg class="timer-svg" id="timer2" viewBox="-40 -40 250.79 250.79" xmlns="http://www.w3.org/2000/svg">
                <circle cx="85.89" cy="85.89" r="84.89" stroke="#fff" stroke-linecap="round" stroke-width="10" />
                <circle cx="85.89" cy="85.89" fill="#eeefefff" r="84.89" stroke="#cca2e9ff" stroke-dasharray="533.1" stroke-dashoffset="533.1"
                        stroke-linecap="round" stroke-width="10" transform="rotate(-90 85.89 85.89)"
                >
                  <animate :dur="`${duration}s`" attributeName="stroke-dashoffset" begin="0s" calcMode="linear" fill="remove" values="0;-533.1" />
                </circle>
              </svg>
            </div>

            <div v-else @click="restartGame" class="restart">
              <img alt="restart" src="@/assets/reload.png">
              <span>Еще раз</span>
            </div>

            <div class="counter wrong">
              <img alt="wrong" src="@/assets/no.png">
              <span>{{ counterWrong }}</span>
            </div>
          </div>
        </div>

        <div class="boxes">
          <div v-for="box in boxes" :key="box.id" @click="cleanTrash(box.id)" class="box-item">
            <span :class="box.color">{{ box.name }}</span>
            <img :alt="`box ${box.color}`" :class="box.color" :src="require(`./assets/bucks/wastebox_${box.color}.min.png`)" class="box">
            <img :alt="`box ${box.color}`" :class="box.color" :src="require(`./assets/bucks/buck_top/wastetop_${box.color}.min.png`)" class="box-cap">
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
      playGame: true,
      isPlaying: false,
      isEndGame: false,
      counterRight: 0,
      counterWrong: 0,
      duration: 5,
      idx: 0,
      pileOfTrash: [
        { id: 0, name: 'пакет от молока', box: 0, },
        { id: 1, name: 'огрызок яблока', box: 2, },
        { id: 2, name: 'старый телефон', box: 1, },
        { id: 3, name: 'батарейка', box: 3, },
        { id: 4, name: 'газеты', box: 0, },
        { id: 5, name: 'лампочка', box: 1, },
        { id: 6, name: 'стул', box: 2, },
        { id: 7, name: 'градусник', box: 3, },
        { id: 8, name: 'памперс', box: 2, },
        { id: 9, name: 'шприц', box: 3, },
      ],
      boxes: [
        { id: 0, name: 'Вторсырьё', color: 'yellow' },
        { id: 1, name: 'Смешанные', color: 'green' },
        { id: 2, name: 'Бытовые', color: 'blue' },
        { id: 3, name: 'Опасные', color: 'orange' },
      ]
    }
  },
  methods: {
    close() {
      this.playGame = false
    },
    startGame() {
      this.isPlaying = !this.isPlaying
    },
    cleanTrash(box) {
      if (this.isPlaying && this.idx < (this.pileOfTrash.length - 1)) {
        if (this.pileOfTrash[this.idx].box === box) {
          ++this.counterRight
        } else {
          ++this.counterWrong
        }
        this.idx += 1
        console.log('idx', this.idx, 'id', (this.pileOfTrash[this.idx].id))
      }

      if (this.idx === (this.pileOfTrash.length - 1)) {
        this.isEndGame = true
      }
    },
    restartGame() {
      this.idx = 0
      this.counterRight = 0
      this.counterWrong = 0
      this.isEndGame = false
      this.isPlaying = false
    },
  },
  computed: {
    trash() {
      return this.pileOfTrash[this.idx].name
    },
    title() {
      return (
        this.isEndGame
          ? `Отлично! Ты набрал ${ this.counterRight } очков из ${ this.pileOfTrash.length }!`
          : 'Выбери правильный бак для сортировки мусора!'
      )
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
    justify-content: space-between;
    align-self: center;
    height: 500px;
    padding: 10px 20px;
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.5);

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

    .header-container {
      display: flex;
      flex-direction: column;
      align-content: center;

      .header {
        max-width: 60%;
        padding: 10px;
        font-size: 20px;
        font-weight: bolder;
        place-self: center;
      }
    }

    .main {
      display: flex;
      justify-content: center;
      min-height: 300px;

      .trash {
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;

        .trash-items {
          position: absolute;
          font-size: 24px;
          font-weight: bolder;
          background: transparent;
        }

        .btn-start {
          position: absolute;
          color: #fff;
          font-size: 42px;
          font-weight: bolder;
          background: transparent;
          cursor: pointer;

          &:hover {
            transform: scale(1.1);
          }
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
          height: 20px;
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

    .restart {
      display: flex;
      justify-content: space-between;
      align-self: center;
      width: 130px;
      margin: 0 auto;
      padding: 5px 15px;
      background: #aa5ce1;
      border-radius: 25px;

      & span {
        color: #fff;
        font-size: 24px;
        font-weight: bolder;
      }

      img {
        align-self: center;
        width: 20px;
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
        margin: 0 5px;
        cursor: pointer;

        .box {
          z-index: 5;
        }

        &:hover {
          .box-cap {
            display: block;
            transition: all 0.3s ease;
            transform: translateY(0);

            &.yellow {
              z-index: 1;
            }

            &.green {
              z-index: 2;
            }

            &.blue {
              z-index: 3;
            }

            &.orange {
              z-index: 4;
            }
          }
        }

        span {
          position: relative;
          margin-bottom: 10px;
          padding: 5px 12px;
          color: #fff;
          border-radius: 25px;

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
          width: 90px;
          margin-top: -20px;
          transform: translateY(60px);
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

@media (max-width: 640px) {
  .container {
    .game-container {
      height: 100%;
      overflow: auto;
      box-shadow: none;

      .main {
        display: grid;
        grid-template-areas:
                  "A C"
                  "B B ";
        justify-content: center;

        .trash {
          grid-area: B;
          margin-bottom: 30px;
        }

        .counter {
          &.right {
            grid-area: auto;
          }

          &.wrong {
            grid-area: auto;
          }
        }
      }

      .boxes {
        display: flex;
        justify-content: center;
        margin-right: 25px;

        .box-item {
          position: relative;
          width: 100px;
          margin-right: -25px;

          &:hover {
            .box-cap {
              transform: translateY(12px);
            }
          }

          span {
            margin-bottom: 30px;
            transform: rotate(-45deg);
          }

          .box-cap {
            width: 80px;
            margin-top: 5px;
            transform: translateY(60px);
          }

          .box {
            margin-top: 10px;

            &.yellow {
              z-index: 100;
            }

            &.green {
              z-index: 90;
            }

            &.blue {
              z-index: 80;
            }

            &.orange {
              z-index: 70;
            }
          }
        }
      }
    }
  }

}
</style>
