<script lang="ts">
	import { each } from 'svelte/internal';

	let rows = [
		['7', '8', '9', '/'],
		['4', '5', '6', 'X'],
		['1', '2', '3', '-'],
		['0', 'C', '+', '=']
	];

	let displayArray = [''];
	let displayString = '0';

	function updateDisplayString() {
		displayString = '';
		displayArray.forEach((element) => {
			displayString += element + ' ';
		});
	}

	function buttonClicked(keyString: string) {
		let lastElemIndex = displayArray.length - 1;
		if (!Number.isNaN(Number(keyString))) {
			displayArray[lastElemIndex] = displayArray[lastElemIndex] + keyString;
		} else {
			switch (keyString) {
				case 'C':
					displayArray = [''];
					break;
				case '=':
					calculate();
					break;
				default:
					displayArray.push(keyString);
					// Empty element so that the next write will be on an empty element
					displayArray.push('');
					break;
			}
		}

		function calculate() {
			// check if first or third element is a number
			if (Number.isNaN(Number(displayArray[0])) || Number.isNaN(Number(displayArray[2]))) {
				alert('Error: Alternating elements must be numeric');
				displayArray = [''];
				return;
			}

			let first_element = Number(displayArray[0]);
			let operator = displayArray[1].trim();
			let second_element = Number(displayArray[2]);

			// delete first 2 elements
			displayArray.shift();
			displayArray.shift();

			switch (operator) {
				case '+':
					displayArray[0] = (first_element + second_element).toString();
					break;

				case 'X':
					displayArray[0] = (first_element * second_element).toString();
					break;

				case '-':
					displayArray[0] = (first_element - second_element).toString();
					break;

				case '/':
					displayArray[0] = (first_element / second_element).toString();
					break;
				default:
					alert('Error: Unkown Character!');
					displayArray = [''];
					return;
			}
		}

		updateDisplayString();
	}
</script>

<h1 class="text-2xl text-center p-10">Boring Calculator</h1>

<h2 class="text-xl bg-gray-400 rounded-lg p-10 m-10 font-bold" id="text-display">
	{displayString}
</h2>

<div class="grid grid-cols-4 grid-flow-row">
	{#each rows as row}
		{#each row as elem}
			<button
				class="bg-blue-700 m-5 text-white rounded-md py-5"
				on:click={() => buttonClicked(elem)}>{elem}</button
			>
		{/each}
	{/each}
</div>
