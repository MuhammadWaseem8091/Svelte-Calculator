<script>
	import { createEventDispatcher } from 'svelte';

	let value = '';
	let array = [];

	$: view = value ? value : '0';


	// const dispatch = createEventDispatcher();

	const select = num => () => {
		if(array.length==1)
		{
			value =num.toString();
			array=[];
		}else{
			value += num;
		}
	
	} 
	const clear  = () => {
		value = '';
	}
	const submit = () => {
		
		view =calculation_arr(convertStrToArray(value));
		
	}
	

	function convertStrToArray(v) {
		// let array = [];
		let previosOperatorIndex = 0;

		if (checkValidInput(v)) {
			alert("Invalid operator");
			return;
		} else {
			v.split("").forEach((element, index) => {
				if (checkIsOperators(element)) {
					array.push(
						parseInt(v.substring(previosOperatorIndex, index))
					);
					array.push(v.substring(index, index + 1));
					previosOperatorIndex = index + 1;
				}
			});
			array.push(parseInt(v.substring(previosOperatorIndex, v.length)));
			return array;
		}
	}
    
	function checkIsOperators(operator) {
		if (
			operator == "+" ||
			operator == "/" ||
			operator == "-" ||
			operator == "*"
		) {
			return true;
		} else {
			return false;
		}
	}

	function checkDoubleOperator(v) {
		let IsValid = false;
		let operators = ["+", "-", "/", "*"];
		operators.forEach((ops1) => {
			operators.forEach((ops2) => {
				if (v.includes(ops1 + ops2)) {
					IsValid = true;
				}
			});
		});
		return IsValid;
	}

	function checkValidInput(v) {
		if (checkIsOperators(v[0]) || checkIsOperators(v[v.length - 1])) {
			return true;
		}
		if (checkDoubleOperator(v)) {
			return true;
		}
		return false;
	}

	function calculations(x1, x2, operat) {
		switch (operat) {
			case "+":
				return x1 + x2;
			case "-":
				return x1 - x2;
			case "/":
				return x1 / x2;
			case "*":
				return x1 * x2;
		}
	}

	function calculation_arr(arr) {
		
		while (true) {
			if (arr.indexOf("/")>0 || arr.indexOf("*")>0) {
				let answer;
		        let index;
				const indexOfDivision = arr.indexOf("/");
				const indexOfMultiplication = arr.indexOf("*");
				if (indexOfDivision === -1) {
					answer = calculations(
						arr[arr.indexOf("*") - 1],
						arr[arr.indexOf("*") + 1],
						"*"
					);
					index = indexOfMultiplication;
				} else if (indexOfMultiplication === -1) {
					
					answer = calculations(
						arr[arr.indexOf("/") - 1],
						arr[arr.indexOf("/") + 1],
						"/"
					);
					index = indexOfDivision;
				} else if (indexOfDivision < indexOfMultiplication) {
					
					answer = calculations(
						arr[arr.indexOf("/") - 1],
						arr[arr.indexOf("/") + 1],
						"/"
					);
					index = indexOfDivision;
				} else {
					answer = calculations(
						arr[arr.indexOf("*") - 1],
						arr[arr.indexOf("*") + 1],
						"*"
					);
					index = indexOfMultiplication;
				}
				arr.splice(index - 1, 3, answer);
				console.log(arr);
			}
			else{
				let answer;
		        let index;
				const indexOfAddtion = arr.indexOf("+");
				const indexOfSubstruction = arr.indexOf("-");
				if (indexOfAddtion === -1) {
					answer = calculations(arr[arr.indexOf("-") - 1],arr[arr.indexOf("-") + 1],"-");
					index = indexOfSubstruction;
				} else if (indexOfSubstruction === -1) {
					answer = calculations(arr[arr.indexOf("+") - 1],arr[arr.indexOf("+") + 1],"+");
					index = indexOfAddtion;
				} else if (indexOfAddtion < indexOfSubstruction) {
					answer = calculations(arr[arr.indexOf("+") - 1],arr[arr.indexOf("+") + 1],"+");
					index = indexOfAddtion;
				} else {
					answer = calculations(arr[arr.indexOf("-") - 1],arr[arr.indexOf("-") + 1],"-");
					index = indexOfSubstruction;
				}
				arr.splice(index - 1, 3, answer);
				console.log(arr);
			}
			if (arr.length === 1){
				break;
			}
		}
		console.log(arr);
		return arr[0];
	}
	
</script>
<h1 style="color: {value ? '#333' : '#ccc'}">{view}</h1>
<div class="keypad">

	<button on:click={select(1)}>1</button>
	<button on:click={select(2)}>2</button>
	<button on:click={select(3)}>3</button>
    <button on:click={select('+')}>+</button>
	<button on:click={select(4)}>4</button>
	<button on:click={select(5)}>5</button>
	<button on:click={select(6)}>6</button>
    <button on:click={select('-')}>-</button>
	<button on:click={select(7)}>7</button>
	<button on:click={select(8)}>8</button>
	<button on:click={select(9)}>9</button>
    <button on:click={select('*')}>*</button>
	<button disabled={!value} on:click={clear}>clear</button>
	<button on:click={select(0)}>0</button>
	<button on:click={submit}>=</button>
    <button on:click={select("/")}>/</button>
    
</div>

<style>
	.keypad {
		display: grid;
		grid-template-columns: repeat(4, 5em);
		grid-template-rows: repeat(4, 3em);
		grid-gap: 0.5em;
        justify-content: center;
        align-items: center;
	}

	button {
		margin: 0
	}
</style>