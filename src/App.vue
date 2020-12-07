<template>
  <div class="wrapper">
	<h1>Simon Says</h1>

	<h2 v-if="isGameOver">Вы проиграли, ваш счет: {{round}}</h2>
	<h2 v-else>Раунд: {{round}}</h2>

	<div class="game-options">
		<button class="start" @click="startGame">Новая игра</button>
		<GameMode :gameModes="gameModes" @mode="changeMode" />
	</div>
	<Board :isDisabled="isDisabled" :tiles="tiles" @tile-click="registerClick" />
	</div>
</template>

<script>
import Board from './components/Board.vue'
import GameMode from './components/GameMode.vue'

export default {
  name: 'App',
  components: {
	Board,
	GameMode
  },
  data() {
    return {
		sequence: [],
		copy: [],
		active: true,
		round: 0,
		delay: 1500,
		isDisabled: true,
		isGameOver: false,
		gameModes: [
			{text: 'Легкий', value: 1500, checked: true}, 
			{text: 'Средний', value: 1000},
			{text: 'Сложный', value: 400}
		],
		tiles:[
			{id: 0, color: 'red', isActive: false},
			{id: 1, color: 'blue', isActive: false},
			{id: 2, color: 'green', isActive: false},
			{id: 3, color: 'yellow', isActive: false}
		]
    }
  },
  methods: {
    startGame() {
		this.sequence = [];
		this.copy = [];
		this.active = true;
		this.isDisabled = true;
		this.round = 0;
		this.isGameOver = false,
		this.newRound();
    },
    newRound() {
		this.round++;
		this.sequence.push(this.randomNumber());
		this.copy = this.sequence.slice(0);
		this.animate(this.sequence);
	},
	registerClick(tile) {
		this.playSound(tile);
		let desiredResponse = this.copy.shift();
		let actualResponse = tile;
		this.active = (desiredResponse === actualResponse);
		this.checkLose();
	},
	checkLose() {
		if (this.copy.length === 0 && this.active) {
			this.deactivateSimonBoard();
			this.newRound();
		} else if (!this.active) { 
			this.deactivateSimonBoard();
			this.endGame();
		}
	},
	endGame() {
		this.isGameOver = true;
	},
	activateSimonBoard() {
		this.isDisabled = false;
	},
	deactivateSimonBoard() {
		this.isDisabled = true;
	},
    animate(sequence) {
		let i = 0;
		let interval = setInterval(() => {
			this.lightUp(sequence[i]);
			this.playSound(sequence[i]);

			i++;
			if (i >= sequence.length) {
				clearInterval(interval);
				this.activateSimonBoard();
			}
		}, this.delay);
    },
    lightUp(tile) {
		this.tiles[tile].isActive = true;
		window.setTimeout(() => this.tiles[tile].isActive = false, 300);
	},
	playSound(tile) {
		let audio = new Audio(require(`./assets/sounds/${tile}.ogg`));
		audio.play();
	},
    changeMode(value) {
		this.delay = value;
	},
    randomNumber() {
      return Math.floor((Math.random() * 4));
    }
  }
}
</script>

<style lang="scss">

	h1, h2 {
		margin: 0;
		padding: 0;
		text-align: center;
	}

	.wrapper {
		display: flex;
		flex-flow: column;
		align-items: center;
	}

	.start {
		font-size: 1rem;
		padding: 10px 20px;
		
	}

	.game-options {
		display: flex;
		justify-content: space-between;
		margin: 10px;
		width: 300px;
	}

</style>
