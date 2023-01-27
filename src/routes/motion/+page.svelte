<script>
	import { animate, stagger, timeline, inView, scroll } from 'motion';
	import SplitType from 'split-type';
	import { onMount } from 'svelte';

	let buttonWrap;
	let sequence;
	let scrollContainer;

	onMount(() => {
		/*
		 ** START ANIMATION
		 */
		// Split the text
		const firstHeading = new SplitType('#first-heading', { types: 'words' });
		const firstParagraph = new SplitType('#first-paragraph', { types: 'lines' });
		const animateText = new SplitType('#animate-text', { types: 'words' });

		// Combine all elements into an array and animate with a stagger
		console.log(firstParagraph.lines);
		const heroElements = [...firstHeading.words, ...firstParagraph.lines, buttonWrap];
		animate(heroElements, { y: [24, 0], opacity: [0, 1] }, { duration: 0.8, delay: stagger(0.05) });

		// Create the animation sequence for the button click
		sequence = [
			['#animate-bg', { x: ['100vw', '0vw'] }, { duration: 0.5 }],
			[
				animateText.words,
				{ y: [16, 0, -16], opacity: [0, 100, 0] },
				{ duration: 0.5, delay: stagger(0.05) }
			],
			['#animate-bg', { x: ['0vw', '-100vw'] }, { duration: 0.5, at: '-0.1' }]
		];

		/*
		 ** INVIEW ANIMATION
		 */
		// Split the text
		const secondHeading = new SplitType('#second-heading', { types: 'words' });
		const secondParagraph = new SplitType('#second-paragraph', { type: 'lines' });

		// Combine into an array and animate with stagger and inView
		const inViewElements = [...secondHeading.words, ...secondParagraph.lines];
		animate(inViewElements, { y: [24], opacity: [0] }, { duration: 0 });
		inView(
			'#second-paragraph',
			() => {
				animate(
					inViewElements,
					{ y: [0], opacity: [100] },
					{ duration: 0.8, delay: stagger(0.05) }
				);
			},
			{ margin: '-80px' }
		);

		/*
		 ** SCROLL ANIMATION
		 */
		scroll(animate('#progress-bar', { scaleX: [0, 1] }), {
			target: scrollContainer,
			offset: ['start', 'end']
		});
	});
</script>

<div class="w-screen h-screen flex items-center justify-center">
	<div class="max-w-3xl text-center flex flex-col items-center justify-center gap-6 p-6">
		<h1 id="first-heading" class="text-2xl md:text-6xl text-slate-800">
			Motion One is a new animation library built on top of the Web Animations API
		</h1>
		<p id="first-paragraph" class="text-slate-500">
			This library was created by Matt Perry who also created Framer Motion. It includes many of the
			same features that GSAP offers like a timeline, scroll trigger, and more, all for free! <br />
			Motion charges for their developer tools, which include a graphical timeline editor.
		</p>
		<div bind:this={buttonWrap}>
			<button
				on:click={() => timeline(sequence)}
				class="rounded-full bg-blue-700 hover:bg-blue-600 active:bg-blue-800 active:translate-y-0 transition-transform hover:-translate-y-1 text-white px-5 py-3"
			>
				Animate something!
			</button>
		</div>
	</div>
	<div
		id="animate-bg"
		class="fixed inset-0 bg-blue-600 h-screen w-screen flex items-center justify-center translate-x-full"
	>
		<span id="animate-text" class="font-semibold text-2xl text-slate-200">Wow look at that!</span>
	</div>
</div>

<div class="w-screen h-screen flex items-center justify-center">
	<div class="max-w-3xl text-center flex flex-col items-center justify-center gap-6 p-6">
		<h1 id="second-heading" class="text-2xl md:text-6xl text-slate-800">
			The inView function lets you to trigger animations when an element scrolls into view
		</h1>
		<p id="second-paragraph" class="text-slate-500">
			From what I can tell GSAP doesn't have this feature built in, although I find that hard to
			believe so I may have just missed something.
		</p>
	</div>
</div>

<div bind:this={scrollContainer} class="w-screen h-[300vh] flex items-start bg-slate-50 relative">
	<div class="w-screen h-4 bg-slate-200 sticky top-0">
		<div id="progress-bar" class="bg-blue-500 w-full h-4 origin-left" />
		<div class="p-2 absolute -bottom-10 text-slate-400">Keep on scrollin'</div>
	</div>
</div>
