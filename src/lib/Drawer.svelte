<script lang="ts">
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  import { draggable } from "@neodrag/svelte";

  import expand from "../assets/fullscreen-expand-svgrepo-com.svg";
  import shrink from "../assets/fullscreen-shrink-svgrepo-com.svg";
  import reset from "../assets/reset-svgrepo-com.svg";

  export let resetCanvas = undefined;

  let hidden = false;
  let fullscreen = false;

  const onFullscreenChange = () => {
    if (!fullscreen && document.body.requestFullscreen) {
      fullscreen = true;
      document.body.requestFullscreen();
    } else if (fullscreen && document.exitFullscreen) {
      fullscreen = false;
      document.exitFullscreen();
    }
  };

  const colors = ["#222", "#c00", "#00c", "#cc0"];
  let selectedColorIndex = 0;

  const _onChangeColor = (color: string, index: number) => {
    dispatch("color", { color });
    selectedColorIndex = index;
  };
</script>

<aside use:draggable={{ handle: ".handle" }}>
  <div class="handle"></div>

  <div class={"divider"} />

  <button class:hidden on:click={onFullscreenChange}>
    {#if fullscreen}
      <img src={shrink} alt="Exit fullscreen view" />
    {:else}
      <img src={expand} alt="Enter fullscreen view" />
    {/if}
  </button>

  <button class:hidden on:click={resetCanvas}>
    <img src={reset} alt="Reset canvas" />
  </button>

  <div class={"divider"} class:hidden />

  {#each colors as color, index}
    <button
      class:selected={selectedColorIndex === index}
      class:hidden
      disabled={selectedColorIndex === index}
      on:click={() => _onChangeColor(color, index)}
    >
      <div class:color style="background-color: {color}" />
    </button>
  {/each}

  <div class={"divider"} class:hidden />

  <button
    class={"arrow"}
    class:hidden
    on:click={() => {
      hidden = !hidden;
    }}
  ></button>
</aside>

<style>
  div.handle {
    width: 16px;
    height: 8px;
    cursor: grab;
    margin: 10px 0;

    background-image: radial-gradient(black 40%, transparent 40%);
    background-size: 4px 4px;
    background-position: 0 100%;
    background-repeat: repeat;
  }

  div.handle:active {
    cursor: grabbing;
  }

  /* Drawer Menu */
  aside {
    display: flex;
    position: absolute;
    left: 5px;
    top: 25%;
    width: 50px;
    height: auto;
    z-index: 10;
    background-color: rgba(255, 255, 255, 0.5);
    backdrop-filter: blur(10px);
    border-radius: 5px;
    align-items: center;
    flex-direction: column;
    flex-wrap: nowrap;
  }

  aside > button {
    width: 100%;
    aspect-ratio: 1;
    border: none;
    background-color: transparent;
    cursor: pointer;
    padding: 6px;
  }

  aside > button:hover {
    background-color: rgba(0, 0, 0, 0.1);
  }

  aside > button.selected {
    background-color: rgba(0, 0, 0, 0.2);
  }

  aside > .hidden {
    display: none;
  }

  aside > button > img {
    width: 100%;
    height: 100%;
  }

  aside > button > .color {
    width: 100%;
    height: 100%;
    border-radius: 20px;
  }

  aside > div.divider {
    width: 100%;
    height: 2px;
    background-color: rgba(0, 0, 0, 0.5);
  }

  aside > button.arrow {
    box-sizing: border-box;
    width: 0;
    height: 0;
    padding: 0;
    margin: 9px;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    border-bottom: 10px solid rgba(0, 0, 0, 0.5);
  }

  aside > button.arrow.hidden {
    display: inline-block;
    margin: 9px 0 0 0;
    border-bottom: 10px solid transparent;
    border-top: 10px solid rgba(0, 0, 0, 0.5);
  }
</style>
