<script>
	import SplitType from 'split-type';
	import { onMount } from 'svelte';
	let buttonWrap;

	onMount(() => {
		const heading = new SplitType('#heading', { types: 'words' });
		const paragraph = new SplitType('p', { types: 'lines' });
		const heroElements = [...heading.words, ...paragraph.lines, buttonWrap];

		const animateText = new SplitType('#animate-text', { types: 'words' });
	});
</script>

<div class="w-screen h-screen p-20 flex items-center justify-center">
	<div class="max-w-3xl text-center flex flex-col items-center justify-center gap-8">
		<h1 id="heading" class="text-6xl leading-tight text-slate-800">Lorem ipsum dolor sit amet.</h1>
		<p class="text-slate-500">
			Lorem ipsum dolor sit amet consectetur adipisicing elit. Quasi similique animi id, vel,
			perspiciatis repudiandae labore beatae voluptas vero maxime, numquam architecto odio
			voluptatum quam qui culpa neque assumenda mollitia!
		</p>
		<div bind:this={buttonWrap}>
			<button
				on:click={() => window.alert('clicked')}
				class="rounded-full bg-blue-700 hover:bg-blue-600 active:bg-blue-800 active:translate-y-0 transition-transform hover:-translate-y-1 text-white px-5 py-3"
			>
				Animate something!
			</button>
		</div>
	</div>
</div>
<div
	id="animate-bg"
	class="fixed inset-0 bg-blue-600 h-screen w-screen flex items-center justify-center translate-x-full"
>
	<span id="animate-text" class="font-semibold text-2xl text-slate-200">Wow look at that!</span>
</div>
