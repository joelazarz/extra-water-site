<script>

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

</style>


<svelte:head>
	<title>Extra Water - Voicemail</title>
</svelte:head>

<div class="container">
	<h1>Voicemail</h1>
	<button on:click={recordAudio}>testing</button>
	<button on:click={logBlob}>Log Blob</button>
</div>

