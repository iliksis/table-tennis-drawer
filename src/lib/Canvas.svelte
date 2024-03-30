<script lang="ts">
  import { onMount } from "svelte";
  import Drawer from "./Drawer.svelte";

  export let color = "#222";
  export let background = "transparent";

  let canvas;
  let context: CanvasRenderingContext2D;
  let isDrawing;
  let start;

  let t, l;

  onMount(() => {
    handleSize();
    context = canvas.getContext("2d");
    context.lineWidth = 3;
  });

  $: if (context) {
    console.log("test");
    context.strokeStyle = color;
  }

  const resetCanvas = () => {
    context.clearRect(0, 0, canvas.width, canvas.height);
  };

  const handleStart = ({ offsetX: x, offsetY: y }) => {
    isDrawing = true;
    start = { x, y };
  };

  const handleEnd = () => {
    isDrawing = false;
  };

  const handleMove = ({ offsetX: x1, offsetY: y1 }) => {
    if (!isDrawing) return;

    const { x, y } = start;
    context.beginPath();
    context.moveTo(x, y);
    context.lineTo(x1, y1);
    context.stroke();
    context.closePath();

    start = { x: x1, y: y1 };
  };

  const handleSize = () => {
    canvas.width = document.body.clientWidth;
    canvas.height = document.body.clientHeight;

    const { top, left } = canvas.getBoundingClientRect();
    t = top;
    l = left;
  };
</script>

<svelte:window on:resize={handleSize} />
<Drawer
  {resetCanvas}
  on:color={({ detail: { color: _color } }) => (color = _color)}
/>
<canvas
  style:background
  bind:this={canvas}
  on:mousedown={handleStart}
  on:touchstart={(e) => {
    const { clientX, clientY } = e.touches[0];
    handleStart({
      offsetX: clientX - l,
      offsetY: clientY - t,
    });
  }}
  on:mouseup={handleEnd}
  on:touchend={handleEnd}
  on:mouseleave={handleEnd}
  on:mousemove={handleMove}
  on:touchmove={(e) => {
    const { clientX, clientY } = e.touches[0];
    handleMove({
      offsetX: clientX - l,
      offsetY: clientY - t,
    });
  }}
/>

<style>
  canvas {
    position: absolute;
    z-index: 1;
  }
</style>
