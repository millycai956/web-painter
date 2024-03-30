<script>
  import { onMount } from 'svelte';
  let canv, isDrawing = false, lineWidth = 5, color = '#000000', isErasing = false;

  onMount(() => {
    canv.getContext('2d').lineJoin = 'round';
    canv.getContext('2d').lineCap = 'round';
  });

  function startDraw(event) {
    const { offsetX, offsetY } = event;
    const ctx = canv.getContext('2d');
    isDrawing = true;
    ctx.beginPath();
    ctx.moveTo(offsetX, offsetY);
  }

  function draw(event) {
    if (!isDrawing) return;
    const { offsetX, offsetY } = event;
    const ctx = canv.getContext('2d');
    ctx.lineWidth = lineWidth;
    ctx.strokeStyle = isErasing ? 'white' : color;
    ctx.lineTo(offsetX, offsetY);
    ctx.stroke();
  }

  function stopDraw() {
    isDrawing = false;
  }

  function clrCanvas() {
    const ctx = canv.getContext('2d');
    ctx.clearRect(0, 0, canv.width, canv.height);
  }
</script>

<style>
  .controls {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 10px;
    margin-bottom: 20px;
  }

  canvas {
    display: block;
    border: 2px solid #333;
    margin-top: 20px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
</style>

<div class="controls">
  <button on:click={() => (isErasing = !isErasing)}>
    {isErasing ? 'Paint Mode' : 'Erase Mode'}
  </button>
  <button on:click={clrCanvas}>Clear Canvas</button>
  <div class="color-picker">
    <span>Color</span>
    <input input type="color" bind:value={color} />
    </div>
    <div class="stroke-weight">
      Brush Width
      <input type="range" min="1" max="50" bind:value={lineWidth} />
      </div>
</div>

<canvas id="canvas"
        bind:this={canv}
        width="800"
        height="600"
        tabindex="0"
        on:mousedown={startDraw}
        on:mousemove={draw}
        on:mouseup={stopDraw}
        on:mouseout={stopDraw}
        on:blur={stopDraw}>
</canvas>