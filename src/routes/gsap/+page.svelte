<script>
	import SplitType from 'split-type';
	import { gsap } from 'gsap';
	import { onMount } from 'svelte';

	let buttonWrap;
	let timeline;

	onMount(() => {
		// Split the text
		const heading = new SplitType('#heading', { types: 'words' });
		const paragraph = new SplitType('p', { types: 'lines' });
		const animateText = new SplitType('#animate-text', { types: 'words' });

		// Combine all elements into an array and animate with a stagger
		const heroElements = [...heading.words, ...paragraph.lines, buttonWrap];
		gsap.from(heroElements, {
			y: 24,
			opacity: 0,
			duration: 0.8,
			stagger: { amount: 1 },
			ease: 'ease'
		});

		// Create the animation sequence for the button click
		timeline = gsap.timeline();
		timeline
			.from('#animate-bg', { x: '100vw', duration: 0.5 })
			.from(animateText.words, { y: 16, opacity: 0, duration: 0.45, stagger: { amount: 0.1 } })
			.to(animateText.words, { y: -16, opacity: 0, duration: 0.45, stagger: { amount: 0.1 } })
			.to('#animate-bg', { x: '-100vw', duration: 0.5 }, '>-=0.1')
			.pause();
	});
</script>

<div class="w-screen h-screen p-20 flex items-center justify-center">
	<div class="max-w-3xl text-center flex flex-col items-center justify-center gap-6">
		<h1 id="heading" class="text-6xl text-slate-800">
			GSAP is a well known and extremely popular animation library in the web dev space
		</h1>
		<p class="text-slate-500">
			You can use it to create animated websites and UIs, and their extensive plugin library helps
			your extend its functionality even further! GSAP has multiple paid tiers to access some of
			their plugins, but a lot of functionality is available for free.
		</p>
		<div bind:this={buttonWrap}>
			<button
				on:click={() => timeline.restart()}
				class="rounded-full bg-blue-700 hover:bg-blue-600 active:bg-blue-800 active:translate-y-0 transition-transform hover:-translate-y-1 text-white px-5 py-3"
			>
				Animate something!
			</button>
		</div>
	</div>
	<div
		id="animate-bg"
		class="fixed inset-0 bg-blue-600 h-screen w-screen flex items-center justify-center"
	>
		<span id="animate-text" class="font-semibold text-2xl text-slate-200">Wow look at that!</span>
	</div>
</div>
