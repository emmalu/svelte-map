<script>
  import { loadModules } from "esri-loader";
  import { onMount } from "svelte";

  //export let title;
  export let centerText;
  const webmapitems = [
    { id: "10241dbd23104e7d9f48b35dc69cf42b", label: "gadot" },
    { id: "68a51fb34e9245698bf60c3c546f90e1", label: "scdot" }
  ];

  let viewDiv; // this is set using "bind:this" down below in the HTML.

  onMount(async () => {
    // Use esri-loader to load the EsriMap and MapView modules
    const esriLoaderOptions = { css: true };
    const [MapView, WebMap, Search] = await loadModules(
      ["esri/views/MapView", "esri/WebMap", "esri/widgets/Search"],
      esriLoaderOptions
    );

    // Create the map(s)
    var webmaps = webmapitems.map(function (webmap) {
      return new WebMap({
        portalItem: {
          // autocasts as new PortalItem()
          id: webmap.id
        }
      });
    });

    // Create the mapView from the map
    const view = new MapView({
      container: viewDiv,
      map: webmaps[0],
      //map: map,
      //zoom: 8,
      //center: [-79.5, 32.5] // Charleston
    });

    view.watch("center", center => {
      const { latitude, longitude } = center;
      centerText = `<span class='font-hairline'>Lat<span>: ${latitude.toFixed(2)} | Lon: ${longitude.toFixed(2)}`;
    });

    document
      .querySelector(".map-buttons")
      .addEventListener("click", function (event) {
        /************************************************************
         * On a button click, change the map of the View
         ************************************************************/
        var id = event.target.getAttribute("data-id");
        if (id) {
          var webmap = webmaps[id];
          view.map = webmap;
          var nodes = document.querySelectorAll(".map-button");
          nodes.forEach(function(node){
            var mapIndex = node.getAttribute("data-id");
            if (mapIndex === id) {
              node.classList.add("bg-blue-200");
              node.classList.remove("bg-transparent");
            }
            else {
              node.classList.remove("bg-blue-200");
              node.classList.add("bg-transparent");
            }
          });
        }
      });

      /* widgets */
      const searchWidget = new Search({
        view: view
      });
      // Adds the search widget below other elements in
      // the top left corner of the view
      view.ui.add(searchWidget, {
        position: "top-left",
        index: 0
      });
  });
</script>

<style>
  .view {
    height: 70vh;
    width: 100%;
  }
</style>

<!-- <h1 class="uppercase">{document.title}</h1> -->
<div class="my-1 map-buttons">
  {#each  webmapitems as webmapitem, i}
    {#if i == 0}
      <button class="bg-blue-200 hover:bg-blue-700 text-blue-700 font-semibold hover:text-white py-2 px-4 border border-blue-200 hover:border-transparent rounded-full uppercase map-button" data-id="{i}">{webmapitem.label}</button>
    {:else}
      <button class="bg-transparent hover:bg-blue-700 text-blue-700 font-semibold hover:text-white py-2 px-4 border border-blue-200 hover:border-transparent rounded-full uppercase map-button" data-id="{i}">{webmapitem.label}</button>
    {/if}
  {/each}
</div>
<div class="view mt-2 pt-2" bind:this={viewDiv} />

{#if centerText}
  <p>{@html centerText}</p>
{/if}
