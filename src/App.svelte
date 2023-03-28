
<script>
  	import Map from './components/Map.svelte';
	import contentData from "./data/test-content-2.json";
	import RangePlayback from './components/RangePlayback.svelte';
	import Switch from './components/Switch.svelte';


	import Scroller from '@sveltejs/svelte-scroller';
	// import LoremIpsum from './LoremIpsum.svelte';
	// import DraggableLabel from './DraggableLabel.svelte';
	
	let count;
	let index;
	let offset;
	let progress;
	let top = 0;
	let threshold = 0.5;
	let bottom = 0;
	let selectedYear = 2014;

	let rasterYear = 2020;
</script>


<div class='demo'>

	<Scroller
		{top}
		{threshold}
		{bottom}
		bind:count
		bind:index
		bind:offset
		bind:progress
	>
		<div slot="background">
			<!-- <p>current section: <strong>{index + 1}/{count}</strong></p>
			<progress value="{count ? (index + 1) / count : 0}"></progress>

			<p>offset in current section</p>
			<progress value={offset || 0}></progress>

			<p>total progress</p>
			<progress value={progress || 0}></progress> -->
      <Map {index} {selectedYear} {rasterYear}/>
		</div>

		<div slot="foreground">

			{#each contentData as content}
			<section class={content.class}>
				<div class="scroll-content">
				{#if content.class === "headline"}
				<h1 class="headline">{content.text}</h1>
				<p>By Stephanie Adeline</p>
				{:else}
				<p>{content.text}</p>

					{#if content.inline_image}
					<img class="inline-image" src={"/assets/" + content.inline_image }/>
					{/if}
					{/if}
				{#if content.slider === "TRUE"}
				<div class="year-controls">
					<div>{selectedYear}</div>
				<RangePlayback min={1978} max={2021} bind:value={selectedYear} />
				</div>
				{/if}
				{#if content.raster_slider === "TRUE"}
				<div class="year-controls">
				<Switch bind:value={rasterYear} design="multi"  label="Switch year" options={[content.raster_options[0], content.raster_options[1]]}/>
				</div>
				{/if}
			</div>
			</section>
			{/each}

		</div>
	</Scroller>

	<!-- <LoremIpsum/>
	
	<DraggableLabel bind:value={top} label="top"/>
	<DraggableLabel bind:value={threshold} label="threshold"/>
	<DraggableLabel bind:value={bottom} label="bottom"/> -->
</div>
<p>This is an explanation</p>


<style>
	/* .demo {
		padding: 0 100px 0 0;
	} */
	
	/* [slot="background"] {
		background-color: rgba(255,62,0,0.05);
		border-top: 2px solid #ff3e00;
		border-bottom: 2px solid #ff3e00;
		font-size: 1.4em;
		overflow: hidden;
	} */
	
	[slot="background"] p {
		margin: 0;
	}
	
	[slot="foreground"] {
		pointer-events: none;
	}
	
	[slot="foreground"] section {
		pointer-events: all;
	}
	
	section {
		margin: 0 0 0 auto;
		height: 120vh;
		/* background-color: rgba(0,0,0,0.5); */
		/* color: white; */
		padding: 1em;
		/* background: white; */

		/* margin: 0 0 2em 0; */
	}

	section.paragraph {
		max-width: 400px;

	}
	.inline-image {
		max-width: 400px;
		width: 100%;
	}

	section.headline {
		max-width: 700px;
	}
	div.scroll-content {
		padding: 20px;

	}

	section.paragraph div.scroll-content {
		background: rgba(255,255,255,0.8);
	}




	.year-controls {
		display: flex;
		align-items: center;
		justify-content: center;
		column-gap: 10px;
	}


  /* p {
    padding: 10px;
  } */
</style>