<script>
  import allTollRoadData from "./../data/java-toll-data.json";

  import restArea from "./../data/rest-area.json";
  import contentData from "./../data/test-content-2.json";

  import { onMount, onDestroy } from "svelte";
  // import { Map, NavigationControl, Popup, FlyToOptions } from 'maplibre-gl';
  import maplibregl from "maplibre-gl";
  import "maplibre-gl/dist/maplibre-gl.css";
  let map;
  let mapContainer;
  export let index = 0;
  let slideContent;

  let allLayerId = ['rest-areas']

  onMount(() => {
    const initialState = { lng: 110.5, lat: -7.6, zoom: 6.5 };

    map = new maplibregl.Map({
      container: mapContainer,
      style: `https://api.maptiler.com/maps/dataviz/style.json?key=LWQbv5QsKtTV1ZJnSbQf`,
      center: [initialState.lng, initialState.lat],
      zoom: initialState.zoom,
    });

    map.on("load", function () {
      map.addSource("all_toll_roads", {
        type: "geojson",
        data: allTollRoadData,
      });

      map.addSource("rest-areas", {
        type: "geojson",
        data: restArea,
      });

      map.addLayer({
        id: "all_toll_roads",
        type: "line",
        source: "all_toll_roads",
        layout: {
          "line-join": "round",
          "line-cap": "round",
        },
        paint: {
          "line-color": "#06BCC1",
          "line-width": 3,
        },
      });
      map.addLayer({
        'id': 'rest-areas',
        'type': 'circle',
        'source': 'rest-areas',
        paint: {
          "circle-color": "#12263A",
          "circle-opacity": 0
        },
        });

    });
  });
  onDestroy(() => {
    map.remove();
  });

  $: if (index) {
    slideContent = contentData.filter((d) => d.slide_index === index)[0];
    map.flyTo({
      center: [slideContent.lng, slideContent.lat],
      zoom: slideContent.zoom,
      essential: true,
    });

    if (slideContent.tolls_filter) {

    map.setFilter('all_toll_roads', slideContent.tolls_filter)
    }

    if (slideContent.other_layers.length > 0) {

      allLayerId.forEach(layer => {
        if (slideContent.other_layers.includes(layer)){
          map.setPaintProperty(layer, 'circle-opacity', 1)
        } else {
        map.setPaintProperty(layer, 'circle-opacity', 0)
        }
      })
    } else {
      // remove all layers
      allLayerId.forEach(layer => {
        map.setPaintProperty(layer, 'circle-opacity', 0)
      })
    }
  }
</script>

<div class="map-wrap">
  <div class="map" id="map" bind:this={mapContainer} />
</div>

<style>
  .map-wrap {
    position: relative;
    width: 100%;
    height: calc(
      100vh - 77px
    ); /* calculate height of the screen minus the heading */
  }
  .map {
    position: absolute;
    width: 100%;
    height: 100%;
  }
</style>
