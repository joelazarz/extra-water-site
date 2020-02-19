<script>
	import * as filestack from 'filestack-js';
	const client = filestack.init('AWdBfzQSjRIu5xTA3I5xQz')

	let blobName = null;
	let voiceBlob = null;
	let mediaRecorderTop = null;
	let timeCounter = 15;
	let t;

	const generateName = () => {
		let str = [];
		const arr = ['e','x','r','t','a','w','2','4','6','8','0'];
		for (let i = 0; i < 10; i++) {
			let char = arr[Math.floor(Math.random() * arr.length)];
			str.push(char);
		}
		blobName = str.join('') + '.wav';
	};

	const recordAudio = () => {
		if (voiceBlob !== null) { voiceBlob = null };

		timeCounter = 15;
		navigator.mediaDevices.getUserMedia({ audio: true })
		.then(stream => {
			let outgoingMessage = new Audio('outgoing.wav');
			outgoingMessage.play(); // plays outgoing message

			setTimeout(() => { // 4 second delay, waits for outgoing message to play
				const mediaRecorder = new MediaRecorder(stream);
				mediaRecorderTop = mediaRecorder; // for stop control
				mediaRecorder.start();

				const recordedChunks = [];
				mediaRecorder.addEventListener("dataavailable", e => {
					recordedChunks.push(e.data);
				});

				t = setInterval(function() { // timer 
					timeCounter--;
					console.log(timeCounter);
				}, 1000);

				// creates blob of recorded audio data once recording has stopped
				mediaRecorder.addEventListener("stop", () => {
					clearInterval(t);
					generateName();
					const mime = ['audio/wav', 'audio/mpeg', 'audio/webm', 'audio/ogg']
					.filter(MediaRecorder.isTypeSupported)[0];
					const audioBlob = new Blob(recordedChunks, {type: mime});
					voiceBlob = audioBlob;
				});

				setTimeout(() => { // stops recording after 15 seconds
					if (mediaRecorder.state === 'inactive') { return };
					mediaRecorder.stop();
				}, 15000);

			}, 4000); // end of timeout for outgoing message

		});
	};

	const stopRecorder = () => {
		if (mediaRecorderTop === null || mediaRecorderTop.state === 'inactive') { return };
		mediaRecorderTop.stop();
	};

	const playBlob = () => {
		if (voiceBlob === null) { return };

		const audioUrl = URL.createObjectURL(voiceBlob);
		const audio = new Audio(audioUrl);
		audio.play();
	};

	const deleteBlob = () => {
		blobName = null;
		voiceBlob = null;
		mediaRecorderTop = null;
		timeCounter = 15;
	};

	const logBlob = () => { 
		console.log(voiceBlob); 
		console.log(blobName);
	};

	const saveBlob = () => {
		if (voiceBlob === null) { return };

		client.upload(voiceBlob, { filename: blobName }) // add filename
		.then(res => {
			console.log(res);
			// set voiceBlob blobName mediaRecorderTop back to null
			deleteBlob();
		});
	};

</script>



<style>
	.container {
		text-align: center;
	}

	h1 {
		font-size: 4em;
		text-transform: uppercase;
		font-weight: 700;
		margin: 0 0 0.5em 0;
	}

	button:hover {
		cursor: pointer;
	}

	.voicemail-box {
		background-color: darkgray;
		display: inline-block;
		width: 20em;
		height: 20em;
		border: 2px solid black;
	}

	.large {
		font-size: 50px;
	}

	.display {
		font-family: 'VT323', monospace;
		font-size: 3em;
		background-color: rgb(200, 248, 200);
		border: 1px solid black;
		margin: 1%;
		width: 97%;
		height: 24%;
	}

	.record-button {
		color: red;
		background-color: lightgrey;
		border: 1px solid black;
		margin: 1%;
		width: 97%;
		height: 30%;
	}

	.controls {
		display: flex;
		height: 15%;
	}

	.controls button {
		background-color: lightgrey;
		border: 1px solid black;
		margin: 1% 2% 1% 2%;
		width: 46%;
		height: 90%;
	}

	.play {
		color: rgb(27, 184, 27);
	}

	.send-button {
		display: flex;
		justify-content: space-around;
		background-color: lightgrey;
		border: 1px solid black;
		margin: 1%;
		width: 97%;
		height: 24%;
	}

	.send-button span {
		font-size: 3em;
		margin-top: 0.2em;
	}

</style>



<svelte:head>
	<title>Extra Water - Voicemail</title>
	<script src="//static.filestackapi.com/filestack-js/3.x.x/filestack.min.js" crossorigin="anonymous"></script>
</svelte:head>

<div class="container">
	<h1>Voicemail</h1>
	<div class="voicemail-box">
		<div class="display">
		{#if timeCounter > 9}
			00:{timeCounter}
		{:else}
			00:0{timeCounter}
		{/if}
		</div>

		<button class="record-button" on:click={recordAudio}>
			<i class="large material-icons">fiber_manual_record</i>
		</button>

	<div class="controls">
		<button class="play" on:click={playBlob}>
			<i class="medium material-icons">play_arrow</i>
		</button>
		<button class="stop" on:click={stopRecorder}>
			<i class="medium material-icons">stop</i>
		</button>
		<button class="delete" on:click={deleteBlob}>
			<i class="medium material-icons">delete_forever</i>
		</button>
	</div>

		<button class="send-button" on:click={saveBlob}>
			<span>send</span>
			<i class="large material-icons">send</i>
		</button>

	</div>

</div>

