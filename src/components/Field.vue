<template>
	<div class="field-container">
		<div class="simon-container">
			<div
				class="button-blue"
				:class="{ 'click-button-blue': clickButton[1] }"
				@click="onClick(1)"
			></div>
			<div
				class="button-red"
				:class="{ 'click-button-red': clickButton[2] }"
				@click="onClick(2)"
			></div>
			<div
				class="button-yellow"
				:class="{ 'click-button-yellow': clickButton[3] }"
				@click="onClick(3)"
			></div>
			<div
				class="button-green"
				:class="{ 'click-button-green': clickButton[4] }"
				@click="onClick(4)"
			></div>
		</div>
		<div class="info-container">
			<div>
				{{
					level === 0
						? 'Click start'
						: level === -1
						? 'You loose :('
						: 'Level: ' + level
				}}
			</div>
			<button @click="onStart">Start</button>
			<div>Mode:</div>
			<div class="mode-options">
				<div class="option">
					<input type="radio" id="easy" v-model="selectMode" value="easy" />
					<label for="easy">Easy </label>
				</div>
				<div class="option">
					<input type="radio" id="normal" v-model="selectMode" value="normal" />
					<label for="normal">Normal</label>
				</div>
				<div class="option">
					<input type="radio" id="hard" v-model="selectMode" value="hard" />
					<label for="hard">Hard </label>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Field',
	data: function () {
		return {
			sounds: {
				1: new Audio(require('../../sounds/sounds_1.mp3')),
				2: new Audio(require('../../sounds/sounds_2.mp3')),
				3: new Audio(require('../../sounds/sounds_3.mp3')),
				4: new Audio(require('../../sounds/sounds_4.mp3')),
			},
			start: false,
			clickTime: false,
			level: 0,
			sequence: [],
			selectMode: 'easy',
			modes: {
				easy: 1500,
				normal: 1000,
				hard: 400,
			},
			numberClick: 0,
			clickButton: {
				1: false,
				2: false,
				3: false,
				4: false,
			},
		}
	},
	methods: {
		onClick: function (id) {
			if (!this.start) return
			if (this.clickTime) {
				this.clickButton[id] = true
				this.numberClick += 1
				this.sounds[id].play()
				setTimeout(() => (this.clickButton[id] = false), 300)
				if (id !== this.sequence[this.numberClick - 1]) {
					this.start = false
					this.sequence = []
					this.clickTime = false
					this.numberClick = 0
					this.level = -1
				} else {
					if (this.numberClick === this.sequence.length) {
						this.level += 1
						this.numberClick = 0
						this.clickTime = false
						setTimeout(() => this.determineSequence(), 1000)
					}
				}
			}
		},

		determineSequence: async function () {
			const id = Math.ceil(Math.random() * 4)
			this.sequence.push(id)

			for (let i = 0; i < this.sequence.length; i++) {
				let promiseTrue = new Promise(resolve => {
					setTimeout(
						() => resolve((this.clickButton[this.sequence[i]] = true)),
						300
					)
				})
				await promiseTrue
				this.sounds[this.sequence[i]].play()
				let promiseFalse = new Promise(resolve => {
					setTimeout(
						() => resolve((this.clickButton[this.sequence[i]] = false)),
						this.modes[this.selectMode]
					)
				})
				await promiseFalse
			}
			this.clickTime = true
		},
		onStart: function () {
			if (this.start) return
			this.start = true
			this.level = 1
			this.determineSequence()
		},
	},
}
</script>

<style>
.field-container {
	display: flex;
	justify-content: center;
	gap: 2rem;
}
.simon-container {
	display: flex;
	flex-flow: row wrap;
	width: 300px;
	border-radius: 100%;
	border: 0.8rem solid rgb(71, 71, 71);
}
.info-container {
	display: flex;
	flex-flow: column wrap;
	justify-content: center;
	gap: 1rem;
}
.mode-options {
	display: flex;
	flex-flow: column wrap;
	gap: 1rem;
}
.option {
	display: flex;
	justify-content: flex-start;
	width: 80px;
}
button {
	background-color: rgb(71, 71, 71);
	border: none;
	border-radius: 10px;
	color: white;
	padding: 10px 20px;
	text-align: center;
	text-decoration: none;
	font-size: 16px;
}
.button-blue {
	height: 150px;
	width: 150px;
	border-radius: 100% 0 0 0;
	background-color: rgb(1, 1, 161);
}
.click-button-blue {
	background-color: rgb(0, 0, 255);
}
.button-green {
	height: 150px;
	width: 150px;
	border-radius: 0 0 100% 0;
	background-color: rgb(0, 142, 0);
}
.click-button-green {
	background-color: rgb(0, 255, 0);
}
.button-red {
	height: 150px;
	width: 150px;
	border-radius: 0 100% 0 0;
	background-color: rgb(148, 0, 0);
}
.click-button-red {
	background-color: rgb(255, 0, 0);
}
.button-yellow {
	height: 150px;
	width: 150px;
	border-radius: 0 0 0 100%;
	background-color: rgb(159, 159, 0);
}
.click-button-yellow {
	background-color: rgb(255, 255, 0);
}
</style>
