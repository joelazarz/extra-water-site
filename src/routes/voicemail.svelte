<script>
	// import * as filestack from 'filestack-js';
	// const client = filestack.init(process.env.FILESTACK_SECRET)

	let voiceBlob = null;

	const logBlob = () => console.log(voiceBlob)

	const recordAudio = () => {
		navigator.mediaDevices.getUserMedia({ audio: true })
		.then(stream => {
			const mediaRecorder = new MediaRecorder(stream);
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
				
				// const audioUrl = URL.createObjectURL(audioBlob);
				// const audio = new Audio(audioUrl);
				// audio.play();
			});
		
			setTimeout(() => {
				mediaRecorder.stop();
			}, 3000);

		});
	};

	const saveBlob = () => {
		if (voiceBlob === null) { return };

		client.upload(voiceBlob) // add filename
		.then(res => console.log(res));
	}

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
		background-color: lightgrey;
		display: inline-block;
		width: 25em;
		height: 20em;
	}

	.record-button {
		width: 100%;
		height: 20%;
	}

	.send-button {
		width: 100%;
		height: 20%;
	}

	.play-button {
		width: 100%;
		height: 20%;
	}

	.stop-button {
		width: 100%;
		height: 20%;
	}

</style>


<svelte:head>
	<title>Extra Water - Voicemail</title>
	<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
	<script src="//static.filestackapi.com/filestack-js/3.x.x/filestack.min.js" crossorigin="anonymous"></script>
</svelte:head>

<div class="container">
	<h1>Voicemail</h1>
	<!-- <button on:click={saveBlob}>Save Blob</button> -->
	<div class="voicemail-box">
		<button class="record-button" on:click={recordAudio}>
			<i class="large material-icons">fiber_manual_record</i>
		</button>
		<button class="play-button">
			<i class="large material-icons">play_arrow</i>
		</button>
		<button class="stop-button">
			<i class="large material-icons">stop</i>
		</button>
		<button class="send-button" on:click={logBlob}>
			<i class="large material-icons">send</i>
		</button>
	</div>


</div>

