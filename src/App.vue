<template>
  <div id="app">
  <section class="row">
    <div class="small-6 columns">
      <h1 class="text-center">PLAYER</h1>
      <div class="wins">
        <div class="wins text-center" style="background-color: green; margin: 0; color: white;" :style="updateProgressBarPlayer">
          <p class="playerScore">{{ playerScore }}</p>
        </div>
      </div>
    </div>
    <div class="small-6 columns">
      <h1 class="text-center">COMPUTER</h1>
      <div class="wins">
        <div class="wins text-center" style="background-color: green; margin: 0; color: white;" :style="updateProgressBarComputer">
          <p class="computerScore">{{ computerScore }}</p>
        </div>
      </div>
    </div>
  </section>
  <section class="row controls" v-if="show">
    <div class="small-12 columns">
      <button id="start-game" v-if="show" @click="hide = !hide" v-on:click="show = !show">START NEW GAME</button>
    </div>
  </section>
  <section class="row controls" v-show="hide">
    <div class="small-12 columns">
      <button id="rock" class="playerChoice" data-choice="rock" v-on:click="handleChoiceClick">ROCK</button>
      <button id="paper" class="playerChoice" data-choice="paper" v-on:click="handleChoiceClick">PAPER</button>
      <button id="scissors" class="playerChoice" data-choice="scissors" v-on:click="handleChoiceClick">SCISSORS</button>
      <button id="restart" v-on:click="restartBTN">RESTART</button>
    </div>
  </section>
  <section v-if="turns.length > 0" class="row log">
    <div class="small-12 columns">
      <ul>
        <li v-for="turn in turns" :key="turn.id" class="{'player-turn': turn.isPlayer, 'computer-turn': !turn.isPlayer, 'tie-turn': turn.tie}">
          {{ turn.text }} 
        </li>
        
      </ul>
    </div>
  </section>
</div>
</template>

<script>
export default {
  data () {
    return {
    choices: ["rock", "paper", "scissors"],
    playerScore: 0,
    computerScore: 0,
    winner: "",
    show: true,
    hide: false,
    rocktie: "",
    turns: [],
    }
  },
  
  computed: {
    //update player progress bar
    updateProgressBarPlayer() {
      //get current width
      //multiply by 10 to current width percent if less than 100
      return {
      width: (this.playerScore * 10) + '%',
      }
    },

    //update computer progress bar
    updateProgressBarComputer() {
      //get current width
      //multiply by 10 to current width percent if less than 100
      return {
      width: (this.computerScore * 10) + '%',
      }
    },
  },

  methods: {
    

    handleChoiceClick (event) {
      this.play(event.currentTarget.getAttribute('data-choice'))
    },

    getComputerChoice() {
      return this.choices[Math.floor(Math.random() * this.choices.length)]
    },


    //main game
    play (playerChoice) {
      let computerChoice = this.getComputerChoice()

      if(computerChoice === 'rock') {
        if (playerChoice === 'paper') {
          this.turns.unshift({
            isPlayer: true,
            text: 'Computer Chose Rock | Paper Beats Rock | Player Wins!'
          });
          this.win();
        } else if(playerChoice === "scissors") {
          this.turns.unshift({
            isPlayer: false,
            text: 'Computer Chose Rock | Rock Beats Scissors | Computer Wins!'
          });
          this.lost();
        } else {
          this.turns.unshift({ 
            tie: true,
            text: 'Computer Chose Rock | You have Tied!' 
          });
        }
      }
      if(computerChoice === 'paper') {
        if(playerChoice === 'rock') {
          this.turns.unshift({ 
            isPlayer: false,
            text: 'Computer Chose Paper | Paper Beats Rock | Computer Wins!'
          });
          this.lost();
        } else if(playerChoice === 'scissors') {
            this.turns.unshift({ 
            isPlayer: true,
            text: 'Computer Chose Paper | Scissors Beats Paper | Player Wins!'
          });
          this.win();
        } else {
          this.turns.unshift({ 
            tie: true,
            text: 'Computer Chose Paper | You have Tied!' 
          });
        }
      }
      if(computerChoice === 'scissors') {
        if(playerChoice === 'rock') {
          this.turns.unshift({ 
            isPlayer: true,
            text: 'Computer Chose Scissors | Rock Beats Scissors | Player Wins!'
          });
          this.win();
        }
        else if(playerChoice === 'paper') {
          this.turns.unshift({ 
            isPlayer: false,
            text: 'Computer Chose Scissors | Rock Beats Scissors | Computer Wins!'
          });
          this.lost();
        } else {
          this.turns.unshift({ 
            tie: true,
            text: 'Computer Chose Scissors | You have Tied!' 
          });
        }
      } 

      //check whether computer player is at 10 and alerts the player and resets the game upon clicking 
      if(this.computerScore === 10) {
        if (confirm("You Lost! New Game?")) {
          window.location.reload();
        }
      }

      if(this.playerScore === 10) {
         if (confirm("You Won! New Game?")) {
          window.location.reload();
         }
      }
    },  

    //result functions

    win() {
      this.winner = 'won';
      this.playerScore++;
    },

    lost() {
      this.winner = 'lost';
      this.computerScore++;
    },
    
    //function to restart game
    restartBTN () {
      window.location.reload();
    },
  },
}
</script>

<style>
.text-center {
    text-align: center;
}

.wins {
    width: 80%;
    color: black;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 1000ms;
}

.controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
}

.turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
}

.log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
}

.log ul li {
    margin: 5px;
}

.log ul .player-turn {
    color: green;
    background-color: #aaffb0;
}

.log ul .computer-turn {
    color: red;
    background-color: #ffc0c1;
}

.log ul .tie-turn {
  color: blue;
  background-color: #e4e8ff;
}

button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
}

#start-game {
    background-color: #e4e8ff;
}

#start-game:hover {
  background-color: #687eff;
}

#rock {
    background-color: #ff7367;
}

#rock:hover {
    background-color: #ff3f43;
}

#paper {
    background-color: #ffaf4f;
}

#paper:hover {
    background-color: #ff9a2b;
}

#scissors {
    background-color: #aaffb0;
}

#scissors:hover {
    background-color: #76ff7e;
}

#restart {
    background-color: #ffffff;
}

#restart:hover {
    background-color: #c7c7c7;
}
</style>
