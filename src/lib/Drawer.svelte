<script lang="ts">
  import expand from "../assets/fullscreen-expand-svgrepo-com.svg";
  import shrink from "../assets/fullscreen-shrink-svgrepo-com.svg";
  import reset from "../assets/reset-svgrepo-com.svg";

  export let resetCanvas = undefined;

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
</script>

<aside>
  <button on:click={onFullscreenChange}>
    {#if fullscreen}
      <img src={shrink} alt="Exit fullscreen view" />
    {:else}
      <img src={expand} alt="Enter fullscreen view" />
    {/if}
  </button>

  <button on:click={resetCanvas}>
    <img src={reset} alt="Reset canvas" />
  </button>
</aside>

<style>
  aside {
    position: absolute;
    left: 5px;
    top: 25%;
    width: 50px;
    height: 100px;
    z-index: 10;
    background-color: rgba(255, 255, 255, 0.5);
    backdrop-filter: blur(10px);
    border-radius: 5px;
    display: flex;
    align-items: center;
    flex-direction: column;
    flex-wrap: nowrap;
  }

  button {
    width: 100%;
    aspect-ratio: 1;
    border: none;
    background-color: transparent;
    cursor: pointer;
  }

  button:hover {
    background-color: rgba(0, 0, 0, 0.1);
  }

  button > img {
    width: 100%;
    height: 100%;
  }
</style>
