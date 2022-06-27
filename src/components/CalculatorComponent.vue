<template>
	<div class="phone rounded-3xl shadow-md">
		<div class="header">
			<!-- <button @click="showHistory" class="history"></button>
			<button @click="deleteValue" class="delete"></button> -->
		</div>

		<!-- <ul v-if="isHistory" class="historyDetails">
			<li :key="operation.operations" v-for="operation in historyData" >
				<small>{{operation.operations}}</small>
				<h2>{{operation.sum}}</h2>
			</li>
		</ul> -->

		<div ref="line" class="line">
			<div>
				<h1 class="noselect" v-bind:class="currentLineClass" >{{currentLine}}</h1>
				<h2 class="noselect" v-bind:class="resultClass">{{result}}</h2>
			</div>
		</div>

		<hr>

		<div class="buttons">
			<button class="w-16 h-16 rounded-full operation noselect" @click="clear">AC</button>
			<button class="w-16 h-16 rounded-full operation noselect" @click="erase"><img class="backspace" src="../assets/backspace.svg"></button>
			<button class="w-16 h-16 rounded-full operation noselect" @click="percent"><img class="percent" src="../assets/percent.svg"></button>
			<button class="w-16 h-16 rounded-full operation noselect" @click="press('/')"><img class="divide" src="../assets/divide.svg"></button>

			<button class="w-16 h-16 rounded-full noselect"			  @click="press('7')">7</button>
			<button class="w-16 h-16 rounded-full noselect"			  @click="press('8')">8</button>
			<button class="w-16 h-16 rounded-full noselect"			  @click="press('9')">9</button>
			<button class="w-16 h-16 rounded-full operation noselect" @click="press('*')"><img class="multiply" src="../assets/multiply.svg"></button>

			<button class="w-16 h-16 rounded-full noselect"			  @click="press('4')">4</button>
			<button class="w-16 h-16 rounded-full noselect"			  @click="press('5')">5</button>
			<button class="w-16 h-16 rounded-full noselect"			  @click="press('6')">6</button>
			<button class="w-16 h-16 rounded-full operation noselect" @click="press('-')"><span class="minus">—</span></button>

			<button class="w-16 h-16 rounded-full noselect"			  @click="press('1')">1</button>
			<button class="w-16 h-16 rounded-full noselect"			  @click="press('2')">2</button>
			<button class="w-16 h-16 rounded-full noselect"			  @click="press('3')">3</button>
			<button class="w-16 h-16 rounded-full operation noselect" @click="press('+')">+</button>
			
			<button class="w-16 h-16 rounded-full operation noselect" @click="negative">+/-</button>
			<button class="w-16 h-16 rounded-full noselect"			  @click="press('0')">0</button>
			<button class="w-16 h-16 rounded-full operation noselect" @click="press('.')">.</button>
			<button class="w-16 h-16 rounded-full operation noselect" @click="showResult">=</button>
		</div>
	</div>
</template>

<script>
export default {
	name: 'CalculatorComponent',
	data () {
		return {
			currentLine: '',
			result: '',
			end: false,
			operations: ['+', '-', '*', '/', '%'],

			currentLineClass: 'main-text',
			resultClass: 'other-text'
		}
	},
	methods: {
		press (value) {
			if (this.end) {
				if (!this.operations.includes(value)) this.clear();
				else this.resume();
			}
			

			this.currentLine += value;
			this.calculate();
		},

		clear () {
			this.resume();
			this.currentLine = '';
			this.result = '';
			this.calculate();
		},

		percent () {
			this.resume();
			this.currentLine = '(' + this.currentLine + ')' + '/100';
			this.calculate();
		},

		negative () {
			this.resume();
			if (this.currentLine[0] == '-') {
				if (this.currentLine[1] == '(') this.currentLine = this.currentLine.slice(2, this.currentLine.length - 1);
				else this.currentLine.slice(1, this.currentLine.length);
			} else this.currentLine = '-(' + this.currentLine + ')';
			
			this.calculate();
		},

		resume () {
			if (this.end) {
				this.currentLine = this.result.slice(2);
				this.currentLineClass = 'main-text';
				this.resultClass = 'other-text';
				this.end = false;
			}
		},

		erase () {
			if (!this.end) {
				this.currentLine = this.currentLine.slice(0, this.currentLine.length - 1);
				this.calculate();
			}
		},

		showResult () {
			this.calculateStrict();
			this.currentLineClass = 'other-text';
			this.resultClass = 'main-text';
			this.end = true;
		},


		calculate () {
			try { this.currentLine == '' ? this.result = '' : this.result = '= ' + parseFloat(eval(this.currentLine).toFixed(10)); } catch {}
		},

		calculateStrict () {
			try { this.currentLine == '' ? this.result = '' : this.result = '= ' + parseFloat(eval(this.currentLine).toFixed(10)); } catch { this.result = 'Wrong input' }
		}
	}
}
</script>

<style scoped>
	* {
		font-family: 'Roboto';
	}

	.phone {
		background-color: #FFF;
		width: 20rem;
		margin: auto;
		position: relative;
	}


	.buttons {
		background-color: #FFF;
		padding: 1.4rem 1rem;
		border-radius: 2rem;
		font-size: 1.4rem;
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(4rem, 1fr));
		grid-gap: .5rem;
	}

	.buttons button {
		-webkit-transition: background-color 1s;
		transition: background-color 1s;
		padding-top: 13px;
		font-size: 1.65rem;
		display: flex;
		justify-content: center;
		align-content: center;
		color: #808285;
	}

	.buttons button:active {
		background-color: #E6E7E8;
		transition: 0s;
	}

	.buttons button.operation {
		color: #66B6AA;
	}

	.buttons button.operation .backspace {
		margin-right: 2px;
		width: 2.4rem;
	}

	.buttons button.operation .multiply {
		width: 2.4rem;
	}

	.buttons button.operation .divide {
		margin-top: 3px;
		width: 2.0rem;
	}

	.buttons button.operation .percent {
		margin-top: 3px;
		width: 2.0rem;
	}

	.buttons button.operation .minus {
		margin-top: -5px;
		font-size: 2rem;
	}





	.line {
		display: flex;
		justify-content: flex-end;
		align-items: flex-end;
		height: 12rem;

		margin-bottom: 10px;

		word-wrap: break-word;
	}

	.line div {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: flex-end;

		text-align: right;
		margin-left: 1rem;
		margin-right: 1.15rem;
		width: 94%;
	}

	.line div h1, .line div h2 {
		line-height: 2.5rem;
		width: 19rem;
		margin-right: -6px;
	}

	.line .main-text {
		color: black;
		font-size: 2.5rem;
		font-weight: 550;
	}

	.line .other-text {
		color: #808285;
		font-size: 1rem;
		font-weight: 400;
	}

	.noselect {
		-webkit-touch-callout: none;
		-webkit-user-select: none;
		-khtml-user-select: none;
		-moz-user-select: none;
		-ms-user-select: none;
		user-select: none;
	}

	.noselect img {
		pointer-events: none;
	}


	hr {
		margin: 0rem 1rem;
	}

	button:focus {
		outline: none;
	}	
</style>