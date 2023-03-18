<script>
  import { onMount, onDestroy } from 'svelte'
  // import { Map, NavigationControl, Popup, FlyToOptions } from 'maplibre-gl';
  import maplibregl from 'maplibre-gl';
  import 'maplibre-gl/dist/maplibre-gl.css';
  let map;
  let mapContainer;
  export let index = 0;

  onMount(() => {
    const initialState = { lng: 110.5, lat: -7.6, zoom: 6.5 };
  
    map = new maplibregl.Map({
      container: mapContainer,
      style: `https://api.maptiler.com/maps/dataviz/style.json?key=LWQbv5QsKtTV1ZJnSbQf`,
      center: [initialState.lng, initialState.lat],
      zoom: initialState.zoom
    });

  });
  onDestroy(() => {
    map.remove();
  });

  
  $: if (index) {
    map.flyTo({
    center: [108.98364642359634, -6.905119328032197],
    zoom: index > 1 ? 15 : 7,
      essential: true // this animation is considered essential with respect to prefers-reduced-motion
      });
    }
      
</script>

<div class="map-wrap">
  <div class="map" id="map" bind:this={mapContainer}></div>
</div>

<style>
  .map-wrap {
    position: relative;
    width: 100%;
    height: calc(100vh - 77px); /* calculate height of the screen minus the heading */
  }
  .map {
    position: absolute;
    width: 100%;
    height: 100%;
  }

</style>