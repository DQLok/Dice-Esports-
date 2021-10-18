<template>
  <div id="app">
    <div class="wrapper clearfix">

      <players
      v-bind:isWinner="isWinner"
      v-bind:activePlayer="activePlayer"
      v-bind:curentScore="curentScore"
        v-bind:scoresPlayer='scoresPlayer'/>

      <controls
      v-bind:isPlaying="isPlaying"
      v-bind:finalScore="finalScore"
      v-on:handleChangeFinalScore="handleChangeFinalScore"
      v-on:handleHoldScore="handleHoldScore"
      v-on:handleNewGame="handleNewGame"
      v-on:handleRollDice="handleRollDice"/>

      <dices
      v-bind:dices="dices"/>

      <popup-rule
      v-on:handleConfirm='handleConfirm'
      v-bind:isOpenPopup="isOpenPopup"/>
    </div>
  </div>
</template>

<script>
import Controls from './components/Controls.vue'
import Dices from './components/Dices.vue'
import Players from './components/Players.vue'
import PopupRule from './components/PopupRule.vue'
export default {
  components: { Players, Controls, Dices, PopupRule },
  name: 'App',
  data () {
    return {
      isPlaying: false,
      isOpenPopup: false,
      activePlayer: 0,
      scoresPlayer: [20, 30],
      dices: [1, 6],
      curentScore: 0,
      finalScore: 10
    }
  },
  methods: {
    handleConfirm () {
      this.isPlaying = true
      this.isOpenPopup = false
      this.activePlayer = 0
      this.dices = [1, 1]
      this.scoresPlayer = [0, 0]
      this.curentScore = 0
    },
    handleNewGame () {
      this.isOpenPopup = true
    },
    handleRollDice () {
      if (this.isPlaying) {
        /* Math.random()==double 0->1 Math.round() == integer */
        var dice1 = Math.floor(Math.random() * 6) + 1
        var dice2 = Math.floor(Math.random() * 6) + 1
        this.dices = [dice1, dice2]
        if (dice1 === 1 || dice2 === 1) {
          let activePlayer = this.activePlayer
          setTimeout(() => {
            alert(`Người chơi Player ${activePlayer + 1} đã xoay vào 1 nút. Rất tiếc `)
          }, 10)
          /* 1 trong 2 con xuc xac 1 nút là mất điểm */
          this.nextPlayer()
        } else {
          this.curentScore = this.curentScore + dice1 + dice2
        }
      } else {
        alert('Vui lòng nhấn vào nút NewGame')
      }
    },
    nextPlayer () {
      this.activePlayer = this.activePlayer === 0 ? 1 : 0
      this.curentScore = 0
    },
    handleHoldScore () {
      if (this.isPlaying) {
        // this.scoresPlayer[this.activePlayer] = this.scoresPlayer
        //  [this.activePlayer] + this.curentScore
        let { scoresPlayer, activePlayer, curentScore } = this
        let scoreOld = scoresPlayer[activePlayer]

        // coppy mảng cũ vào mảng mới
        // let cloneScorePlayer = [...scoresPlayer]

        // cloneScorePlayer[activePlayer] = scoreOld + curentScore
        // this.scoresPlayer = cloneScorePlayer
        // this.scoresPlayer[activePlayer] = scoreOld + curentScore

        // cách 2 xử lý mảng
        this.$set(this.scoresPlayer, activePlayer, scoreOld + curentScore)
        if (!this.isWinner) {
          this.nextPlayer()
        } else {
          this.isPlaying = false
        }
      } else {
        alert('Vui lòng nhấn vào nút NewGame')
      }
    },
    handleChangeFinalScore (e) {
      var number = parseInt(e.target.value)
      if (isNaN(number)) {
        this.finalScore = ''
      } else {
        this.finalScore = number
      }
    }
  },
  computed: {
    isWinner () {
      let { scoresPlayer, finalScore } = this
      if (scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
        // Dừng cuộc chơi
        return true
      }
      return false
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.clearfix::after {
  content: "";
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(
      rgba(62, 20, 20, 0.4),
      rgba(62, 20, 20, 0.4)
    ),
    url("./assets/back.jpg");
  background-size: cover;
  background-position: center;
  font-family: Lato;
  font-weight: 300;
  position: relative;
  height: 100vh;
  color: #555;
}

.wrapper {
  width: 1000px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}

</style>
