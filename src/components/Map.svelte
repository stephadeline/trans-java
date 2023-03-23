<script>
  import allTollRoadData from "./../data/java-toll-data.json";

  import restArea from "./../data/rest-area.json";

  import tollGates from "./../data/gerbang-tol.json";

  import underConstruction from "./../data/konstruksi.json";
  import contentData from "./../data/test-content-2.json";

  import { onMount, onDestroy } from "svelte";
  // import { Map, NavigationControl, Popup, FlyToOptions } from 'maplibre-gl';
  import maplibregl from "maplibre-gl";
  import "maplibre-gl/dist/maplibre-gl.css";

  export let selectedYear = 2014;
  export let index = 0;

  let map;
  let mapContainer;
  let slideContent;

  let allLayerId = ["rest-areas", "toll-gates"];
  let allLinesId = ["construction"]
  let allRaster = ["brebes-2014", "brebes-2020"]


  onMount(() => {
    const initialState = { lng: 110.5, lat: -6.8, zoom: 6.5 };

    map = new maplibregl.Map({
      container: mapContainer,
      style: `https://api.maptiler.com/maps/bright-v2/style.json?key=LWQbv5QsKtTV1ZJnSbQf`,
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

      map.addSource("toll-gates", {
        type: "geojson",
        data: tollGates,
      });

      map.addSource("construction", {
        type: "geojson",
        data: underConstruction,
      });

      map.addSource("brebes-2014", {
        "type": "image",
        "url": "assets/brebes2014.jpg",
        "coordinates": [
          [ 108.296246230602279, -6.626304532490591 ],[ 109.394879043102264, -6.626304532490591 ], [ 109.394879043102264, -7.096013863041794 ], [ 108.296246230602279, -7.096013863041794 ],
        ]
    });

    map.addSource("brebes-2020", {
        "type": "image",
        "url": "assets/brebes-2020.jpg",
        "coordinates": [
           [ 108.3480128645897, -6.6200220929638665 ],
           [ 109.4466456770897, -6.6200220929638665 ],
          [ 109.4466456770897, -7.128575515704274 ], [ 108.3480128645897, -7.128575515704274 ],
        ]
    });

    map.addLayer({
    "id": "brebes-2014",
    "source": "brebes-2014",
    "type": "raster",
    "paint": {
        "raster-opacity": 0
    }
});

map.addLayer({
    "id": "brebes-2020",
    "source": "brebes-2020",
    "type": "raster",
    "paint": {
        "raster-opacity": 0
    }
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
          "line-color": "#FF8200",
          "line-width": 3,
        },
      });
      map.addLayer({
        id: "rest-areas",
        type: "circle",
        source: "rest-areas",
        paint: {
          "circle-color": "#06BCC1",
          "circle-opacity": 0,
        },
      });

      map.addLayer({
        id: "toll-gates",
        type: "circle",
        source: "toll-gates",
        paint: {
          "circle-color": "#ef4123",
          "circle-opacity": 0,
        },
      });

      map.addLayer({
        id: "construction",
        type: "line",
        source: "construction",
        layout: {
          "line-join": "round",
          "line-cap": "round",
        },
        paint: {
          "line-color": "#FF8200",
          "line-width": 3,
          "line-dasharray": [0.1, 2],
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
      map.setFilter("all_toll_roads", slideContent.tolls_filter);
    } else {
      map.setFilter("all_toll_roads", ["has", "yearnum"]);
    }

    if (slideContent.show_tolls === "FALSE") {
      map.setPaintProperty("all_toll_roads", "line-opacity", 0)
    } else {
      map.setPaintProperty("all_toll_roads", "line-opacity", 1)
    }

    if (slideContent.other_layers.length > 0) {
      allLayerId.forEach((layer) => {
        if (slideContent.other_layers.includes(layer)) {
          map.setPaintProperty(layer, "circle-opacity", 1);
        } else {
          map.setPaintProperty(layer, "circle-opacity", 0);
        }
      });
    } else {
      // remove all layers
      allLayerId.forEach((layer) => {
        map.setPaintProperty(layer, "circle-opacity", 0);
      });
    }

    if (slideContent.other_line_layers.length > 0) {
      allLinesId.forEach((layer) => {
        if (slideContent.other_line_layers.includes(layer)) {
          map.setPaintProperty(layer, "line-opacity", 1);
        } else {
          map.setPaintProperty(layer, "line-opacity", 0);
        }
      });
    } else {
      // remove all layers
      allLinesId.forEach((layer) => {
        map.setPaintProperty(layer, "line-opacity", 0);
      });
    }

    if (slideContent.raster_layers.length > 0) {
      allRaster.forEach((layer) => {
        if (slideContent.raster_layers.includes(layer)) {
          map.setPaintProperty(layer, "raster-opacity", 1);
        } else {
          map.setPaintProperty(layer, "raster-opacity", 0);
        }
      });
    } else {
      // remove all layers
      allRaster.forEach((layer) => {
        map.setPaintProperty(layer, "raster-opacity", 0);
      });
    }

    if (slideContent.style) {
      map.setPaintProperty(
        slideContent.style_layer,
        slideContent.style_property,
        slideContent.style
      );
    }
    if (selectedYear) {
      if (slideContent.slider === "TRUE") {
        map.setFilter("all_toll_roads", ["<=", "yearnum", selectedYear]);
      }
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
    height: 100vh; /* calculate height of the screen minus the heading */
  }
  .map {
    position: absolute;
    width: 100%;
    height: 100%;
  }
</style>
