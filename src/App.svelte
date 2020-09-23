<script lang="ts">
	import { gsap } from 'gsap'

	let x: number;
	let y: number;
	let screen_h: number;
	let screen_w: number;

	let end_y = 350;
	let figure_el: HTMLElement;
	let mac_el: HTMLImageElement;
	let moutain_el: HTMLDivElement;
	let tl: gsap.core.Timeline;

	$: is_mobile = screen_w < 400
	$: mac_screen_width = is_mobile ? 0.80 : 0.80;
	$: mac_screen_height = is_mobile ? 0.82 : 0.50;
	$: rotateX = is_mobile ? -78 : -70;
	$: progress = y / end_y;

	$: if (tl) {
		tl.progress(progress)
	}

	async function loaded() {
		const img: HTMLImageElement = mac_el
		const timeline = gsap.timeline({ paused: true })
		timeline
			// .to(moutain_el, { scale: 0.6, y: 200, duration: 1 })
			.to(moutain_el, {
				rotateX: rotateX,
				y: figure_el.offsetTop + img.clientHeight * 0.1,
				scaleX: img.clientWidth / moutain_el.clientWidth * mac_screen_width,
				scaleY: img.clientHeight / moutain_el.clientHeight * mac_screen_height,
				ease: "power2.out",
				duration: 2
			})
		if (tl) {
			timeline.progress(tl.progress())
		}
		tl = timeline
	}
</script>

<svelte:window 
	bind:scrollY={y} 
	bind:scrollX={x} 
	bind:innerHeight={screen_h}
	bind:innerWidth={screen_w}
	on:resize={loaded}
/>

<!-- <section style='height: 50vh'></section> -->
<main class='relative'>
	<div id='grid' bind:this={moutain_el}>
		Krist
	</div>
	<!-- <div style='height: 100px'></div> -->
	<div class="container">
		<figure bind:this={figure_el} style='margin: 0'>
			<img 
				style='position: block; width: 100%' 
				on:load={loaded} 
				bind:this={mac_el} 
				src="/img/mac.jpeg" 
				alt="mac"
			>
		</figure>
	</div>
</main>
<section style='height: 50vh'></section>

<style>
	main {
		perspective-origin: center bottom;
    perspective: 2000px;
	}

	#grid {
		height: 100vh;
		width: 100vw;
		top: 0;
		background-image: url(/img/grid.png);
		background-image: url(/img/mountain.jpg);
		background-repeat: no-repeat;
		background-size: cover;
		margin: auto;
		transform-origin: top center;
		/* transform: translate(-50%, -50%); */
	}
	.container {
		max-width: 700px;
		margin: auto;
	}
	.relative {
		position: relative;
	}
	figure {
		z-index: -1;
	}
</style>