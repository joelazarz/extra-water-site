<script>
	// import * as filestack from 'filestack-js';
	// const client = filestack.init(process.env.FILESTACK_SECRET)

	let voiceBlob = null;

	const recordAudio = () => {
		if (voiceBlob !== null) { voiceBlob = null };

		navigator.mediaDevices.getUserMedia({ audio: true })
		.then(stream => {
			// play outgoing message
			const mediaRecorder = new MediaRecorder(stream);
			// delay this for as long as the outgoing message plays
			mediaRecorder.start();

			const recordedChunks = [];
			mediaRecorder.addEventListener("dataavailable", e => {
				recordedChunks.push(e.data);
			});

			mediaRecorder.addEventListener("stop", () => {
				const mime = ['audio/wav', 'audio/mpeg', 'audio/webm', 'audio/ogg']
				.filter(MediaRecorder.isTypeSupported)[0];
				const audioBlob = new Blob(recordedChunks, {type: mime});
				voiceBlob = audioBlob;
				console.log('recording stopped')

				// const audioUrl = URL.createObjectURL(audioBlob);
				// const audio = new Audio(audioUrl);
				// audio.play();
			});
		
			setTimeout(() => {
				mediaRecorder.stop();
			}, 3000);

		});
	};

	const playMessage = () => {
		const audioUrl = URL.createObjectURL(voiceBlob);
		const audio = new Audio(audioUrl);
		audio.play();
	};

	const deleteMessage = () => voiceBlob = null;

	const logBlob = () => console.log(voiceBlob);

	const saveBlob = () => {
		if (voiceBlob === null) { return };

		client.upload(voiceBlob) // add filename
		.then(res => console.log(res));
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

	.play-button {
		color: rgb(27, 184, 27);
		background-color: lightgrey;
		border: 1px solid black;
		margin: 1%;
		width: 48%;
		height: 90%;
	}

	.stop-button {
		background-color: lightgrey;
		border: 1px solid black;
		margin: 1%;
		width: 48%;
		height: 90%;
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
	<link href="https://fonts.googleapis.com/css?family=VT323&display=swap" rel="stylesheet">
	<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
	<script src="//static.filestackapi.com/filestack-js/3.x.x/filestack.min.js" crossorigin="anonymous"></script>
</svelte:head>

<div class="container">
	<h1>Voicemail</h1>
	<div class="voicemail-box">
		<div class="display">00:00</div>

		<button class="record-button" on:click={recordAudio}>
			<i class="large material-icons">fiber_manual_record</i>
		</button>

	<div class="controls">
		<button class="play-button" on:click={playMessage}>
			<i class="medium material-icons">play_arrow</i>
		</button>
		<button class="stop-button">
			<i class="medium material-icons">stop</i>
		</button>
		<button class="stop-button" on:click={deleteMessage}>
			<i class="medium material-icons">delete_forever</i>
		</button>
	</div>

		<button class="send-button" on:click={logBlob}>
			<span>send</span>
			<i class="large material-icons">send</i>
		</button>

	</div>


</div>

