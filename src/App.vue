<template>
  <div id="app">
    <div class="wrapper clearfix">

        <players 
            v-bind:isWinner="isWinner"
            v-bind:activePlayer="activePlayer"
            v-bind:currentScore="currentScore"
            v-bind:scoresPlayer="scoresPlayer"
        />

        <controls 
            v-bind:isPlaying="isPlaying"
            v-bind:finalScore="finalScore"
            v-on:handleChangeFinalScore="handleChangeFinalScore"
            v-on:handleHoldScore="handleHoldScore"
            v-on:handleNewGame="handleNewGame"
            v-on:handleRollDice="handleRollDice"
        />

        <dices 
            v-bind:dices="dices"
        />

        <popup-rule 
            v-on:handleConfirm="handleConfirm"
            v-bind:isOpenPopup="isOpenPopup"
        />

    </div>
  </div>
</template>

<script>
import Players from './compnents/Players';
import Controls from './compnents/Controls';
import Dices from './compnents/Dices';
import PopupRule from './compnents/PopupRul'

export default {
  name: 'app',
  data () {
    return {
        isPlaying: false,
        isOpenPopup: false,
        dices: [2, 5],
        activePlayer: 0, //Ai là người chơi hiện tại
        scoresPlayer: [30, 12],
        currentScore: 24,
        finalScore: 10
    }
  },
  components: {
    Players,
    Controls,
    Dices,
    PopupRule
  },
  computed: {
    isWinner() {
        let { scoresPlayer, finalScore} = this;

        if(scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
            //Dừng cuộc chơi
            this.isPlaying = false;
            return true;
        }
        return false;
    }
  },
  methods: {
    handleChangeFinalScore(e) {
        var number = parseInt(e.target.value);
        if(isNaN(number)) {
            this.finalScore = '';
        } else {
            this.finalScore = number;
        }
        // console.log(parseInt(e.target.value));
    },
    handleHoldScore() {
        if(this.isPlaying) {
            // console.log('handleHoldScore');
            // activePlayer = 0 -> Người chơi 1
            // activePlayer = 1 -> người chơi 2
            // scoresPlayer -> array
            // scoresPlayer[0] = scoresPlayer[activePlayer]
            // scoresPlayer[1] = scoresPlayer[activePlayer]
            let { scoresPlayer, activePlayer, currentScore} = this;
            let scoreOld = scoresPlayer[activePlayer];
            // let cloneScorePlayer = [...scoresPlayer];
                // cloneScorePlayer[activePlayer] = scoreOld + currentScore;

            this.$set(this.scoresPlayer, activePlayer, scoreOld + currentScore);
            // this.scoresPlayer = cloneScorePlayer;

            if(!this.isWinner) {
                this.nextPlayer();
            }
            // this.scoresPlayer[this.activePlayer] = this.scoresPlayer[this.activePlayer] + this.currentScore;
            // this.scoresPlayer[activePlayer] = scoreOld + currentScore;
            // console.log(cloneScorePlayer);
        } else {
            alert('Vui lòng nhấn vào nút NewGame');
        }
    },
    nextPlayer() {
        //activePlayer = 0 -> 1 ======= 1 -> 0
        this.activePlayer = this.activePlayer === 0 ? 1 : 0;
        this.currentScore = 0;
    },
    handleNewGame() {
        // console.log('New Game App');
        //Hiển thị Popup -> show luật chơi
        this.isOpenPopup = true;
    },
    handleConfirm() {
        // console.log('confirm App');
        this.isPlaying = true;
        this.isOpenPopup = false;
        this.dices = [1, 1];
        this.activePlayer = 0;
        this.scoresPlayer = [0, 0];
        this.currentScore = 0;
    },
    handleRollDice() {
        // console.log('rollDice app');
        if(this.isPlaying) {
            // Xoay xúc xắc
            // Math.random(): 0 -> 1
            /*
            0 <= X <= 1
            0 * 6 <= X * 6 <= 1 * 6
            0 <= Y = X *6 <= 6
             */
            // Math.floor() : làm tròn xuống
            var dice1 = Math.floor(Math.random() * 6) + 1;
            var dice2 = Math.floor(Math.random() * 6) + 1;
            this.dices = [dice1, dice2];
            console.log(dice1, dice2);
            
            if(dice1 === 1 || dice2 === 1) {
                // Đổi lượt chơi

                // Reset điểm tạm thời về 0
                let activePlayer = this.activePlayer;
                setTimeout(function() {
                    alert(`Người chơi Player ${activePlayer + 1} đã quay trúng số 1. Rất tiếc`);
                }, 10)
                this.nextPlayer();
            } else {
                // Cộng dồn vào điểm tạm thời cho người chơi
                this.currentScore = this.currentScore + dice1 + dice2;
            }
        } else {
            alert('Vui lòng nhấn vào nút NewGame');
        }
    }
  },
  
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
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('/public/assets/back.jpg');
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
