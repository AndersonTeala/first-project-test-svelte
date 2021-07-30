<script>
	export let name;
	let dev = 'Anderson Teala'
	let src = 'https://media.tenor.com/images/7d9f437761b7564e26dcedbf26fb697f/tenor.gif'
	let count = 0;
	$: doubled = count * 2;
	$: if (count >= 10) {
		alert(`count is dangerously high!`);
		count = 9;
	}
	let m = { x: 0, y: 0 };

	function handleMousemove(event) {
		m.x = event.clientX;
		m.y = event.clientY;
	}

	function incrementCount() {
		count += 1
	}

	// CONGRATULATIONS
	import { onMount } from 'svelte';

	let characters = ['👽', '👻', '👾'];

	let confetti = new Array(100).fill()
		.map((_, i) => {
			return {
				character: characters[i % characters.length],
				x: Math.random() * 100,
				y: -20 - Math.random() * 100,
				r: 0.1 + Math.random() * 1
			};
		})
		.sort((a, b) => a.r - b.r);

	onMount(() => {
		let frame;

		function loop() {
			frame = requestAnimationFrame(loop);

			confetti = confetti.map(emoji => {
				emoji.y += 0.7 * emoji.r;
				if (emoji.y > 120) emoji.y = -20;
				return emoji;
			});
		}

		loop();

		return () => cancelAnimationFrame(frame);
	});
</script>

{#each confetti as c}
	<span style="left: {c.x}%; top: {c.y}%; transform: scale({c.r})">{c.character}</span>
{/each}

<main>
	<h1>Hello {name}!</h1>
	<br>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
	<br>
	<h5>{dev}</h5>
	<br>
	<img src={src}>
	<br>
	<button on:click="{incrementCount}" >
		Clicked {count} {count === 1 ? 'time' : 'times'}
	</button>
	<br>
	<p>{count} doubled is {doubled}</p>
	<br>
	<div on:mousemove={handleMousemove}>
		The mouse position is {m.x} x {m.y}
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
		/* background-color: #282a36; */
	}

	h1 {
		color: #8257E4;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

	h5 {
		color: #022850;
		font-size: 30px;
	}

	img {
		width: 300px;
	}

	:global(body) {
		overflow: hidden;
	}

	span {
		position: absolute;
		font-size: 5vw;
	}
</style>