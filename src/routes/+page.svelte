<script>
	// The state for the game will consist of the word to be guessed,
	// the letters that have been guessed, and the number of incorrect
	// guesses that have been made.
	let word = '';
	let letters = [];
	let incorrectGuesses = 0;

	// The state for the game will also keep track of whether the game
	// is over or not. This will be either true or false.
	let gameOver = false;

	// This function will be called when the user makes a guess by
	// pressing a letter key. It will update the state of the game
	// based on whether the guess was correct or not.
	const handleKeyPress = (event) => {
		if (gameOver) return;
		if (!start) return;
		// Get the letter that was pressed.
		const letter = event.key;

		// Check if the letter has already been guessed.
		if (letters.includes(letter)) {
			// The letter has already been guessed, so do nothing.
			return;
		}

		// Add the letter to the list of letters that have been guessed.
		letters = [...letters, letter];

		// Check if the word contains the letter.
		if (word.includes(letter)) {
			// The letter is in the word, so the guess is correct.
			// We don't need to update the number of incorrect guesses.
			// Check if all of the letters in the word have been correctly
			// guessed. If they have, the player has won the game.
			if (word.split('').every((l) => letters.includes(l))) {
				// All of the letters have been correctly guessed, so the player
				// has won the game. Update the gameOver state.
				gameOver = true;
			}
		} else {
			// The letter is not in the word, so the guess is incorrect.
			// Increment the number of incorrect guesses.
			incorrectGuesses++;
		}
	};

	// This function will be called to render the word being guessed,
	// with underscores representing unguessed letters and correctly
	// guessed letters being shown in their correct positions.
	const renderWord = (lets, wrd) => {
		// Create an array of letters for the word, with underscores
		// representing unguessed letters.
		const wordLetters = wrd.split('').map((letter) => {
			if (lets.includes(letter)) {
				// This letter has been guessed, so show it.
				return letter;
			} else {
				// This letter has not been guessed, so show an underscore.
				return '_';
			}
		});

		// Return the word as a string, with spaces between each letter.
		console.log(wordLetters);
		return wordLetters.join(' ');
	};

	$: guessed = renderWord(letters, word);
	let start = false;
</script>

<!-- Listen for key presses on the document and call the handleKeyPress
       function when a letter key is pressed. -->
<svelte:window on:keypress={handleKeyPress} />

{#if !start}<input bind:value={word} />{/if}<button
	on:click={() => {
		start = true;
	}}>Start</button
>

<!-- Display the word being guessed. -->
<p>{guessed}</p>

<!-- Display the number of incorrect guesses. -->
<p>Incorrect guesses: {incorrectGuesses}</p>

{#if gameOver}You won.{/if}
