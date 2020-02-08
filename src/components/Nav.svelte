<script>
	export let segment;
</script>

<style>
	nav {
		display: flex;
		border-bottom: 1px solid rgba(100, 100, 100, 0.1);
		font-weight: 300;
		padding: 0 1em;
	}

	ul {
		margin: 0;
		padding: 0;
	}

	/* clearfix */
	ul::after {
		content: '';
		display: block;
		clear: both;
	}

	li {
		font-family: 'Roboto', sans-serif;
		font-weight: 700;
		display: block;
		float: left;
	}

	.selected {
		background-color: aliceblue;
		position: relative;
		display: inline-block;
	}

	a {
		text-decoration: none;
		padding: 1em 0.5em;
		display: block;
	}

	.site-nav {
		width: 50%;
	}

	.site-nav ul {
		margin-top:0.3em;
	}

	.links {
		width: 50%;
		padding-right: 1em;
	}

	.links ul {
		float: right;
	}

	.soundcloud {
		margin-top: 0.1em;
		margin-bottom: -0.6em;
		height: auto;
		width: 35px;
	}

	.instagram {
		margin-top: 0.4em;
		margin-bottom: -0.6em;
		margin-left: 0.5em;
		height: auto;
		width: 25px;
	}

</style>

<nav>
		<div class="site-nav">
			<ul>
				<li><a class:selected='{segment === undefined}' href='.'>home</a></li>
				<li><a class:selected='{segment === "voicemail"}' href='voicemail'>voicemail</a></li>
			</ul>
		</div>
		<!-- for the blog link, we're using rel=prefetch so that Sapper prefetches
				the blog data when we hover over the link or tap it on a touchscreen -->
		<!-- <li><a rel=prefetch class:selected='{segment === "blog"}' href='blog'>blog</a></li> -->


		<div class="links">
			<ul>
					<li><a href="https://soundcloud.com/extrawater"><img class="soundcloud" src='soundcloud.png' alt='soundcloud' /></a></li>
					<li><a href="https://instagram.com/extra.water"><img class="instagram" src='instagram.png' alt='instagram' /></a></li>
			</ul>
		</div>

</nav>
