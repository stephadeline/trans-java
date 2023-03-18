
<script>
  	import Map from './components/Map.svelte';

	import Scroller from '@sveltejs/svelte-scroller';
	// import LoremIpsum from './LoremIpsum.svelte';
	// import DraggableLabel from './DraggableLabel.svelte';
	
	let count;
	let index;
	let offset;
	let progress;
	let top = 0.1;
	let threshold = 0.5;
	let bottom = 0.9;
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
      <Map {index}/>
		</div>

		<div slot="foreground" style="padding: 0 0 0 50%;">
			<section><h2>Trans-Java Toll road</h2></section>
			<section><p>In 2014, we didn't have that many toll roads in Java.</p></section>
			<section><p>Since 2014, xx more km of toll roads have been operating.</p></section>
			<section><p>section 4</p></section>
			<section><p>section 5</p></section>
		</div>
	</Scroller>

	<!-- <LoremIpsum/>
	
	<DraggableLabel bind:value={top} label="top"/>
	<DraggableLabel bind:value={threshold} label="threshold"/>
	<DraggableLabel bind:value={bottom} label="bottom"/> -->
</div>

<style>
	/* .demo {
		padding: 0 100px 0 0;
	}
	
	[slot="background"] {
		background-color: rgba(255,62,0,0.05);
		border-top: 2px solid #ff3e00;
		border-bottom: 2px solid #ff3e00;
		font-size: 1.4em;
		overflow: hidden;
		padding: 1em;
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
		height: 120vh;
		/* background-color: rgba(0,0,0,0.5); */
		/* color: white; */
		padding: 1em;
		/* margin: 0 0 2em 0; */
	}

  p {
    background: white;
    padding: 10px;
  }
</style>