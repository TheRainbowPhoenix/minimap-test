<script lang="ts">
  import { onMount } from "svelte";

  const clamp = (value, min, max) => Math.min(max, Math.max(min, value));

  const pois = [
    {
      x: 597,
      y: 101,
      c: "#C114E7",
    },
    {
      x: 444,
      y: 496,
      c: "#16B422",
    },
    {
      x: 754,
      y: 690,
      c: "#8713D5",
    },
    {
      x: 694,
      y: 394,
      c: "#0E9BE7",
    },
    {
      x: 945,
      y: 490,
      c: "#C8530D",
    },
    {
      x: 389,
      y: 642,
      c: "#9F6422",
    },
    {
      x: 558,
      y: 666,
      c: "#DC50B9",
    },
    {
      x: 1416,
      y: 426,
      c: "#C70E12",
    },
  ];

  let x = 0;
  let y = 0;
  let z = 0;
  let angle = 0;

  const maxX = 1815;
  const maxY = 964;

  onMount(() => {
    window.addEventListener("LocalPlayerPos", (event) => {
      const customEvent = event as CustomEvent<number[]>;
      if (customEvent.detail != undefined) {
        // doArmorUpdate
        // console.log(customEvent);
        [x, y, z] = customEvent.detail;
      }
    });

    window.addEventListener("LocalPlayerAngles", (event) => {
      const customEvent = event as CustomEvent<number[]>;
      if (customEvent.detail != undefined) {
        // doArmorUpdate
        let _;
        [angle, _, _] = customEvent.detail;
      }
    });
  });
</script>

<div class="root" style="--a: {(180 - (45 - angle)).toFixed(2)}deg">
  <div class="container">
    <div
      class="map"
      style="
      width: {maxX}px;
      height: {maxY}px;
      --x: {(100 - x).toFixed(2)}px;
      --y: {(100 - y).toFixed(2)}px;"
    >
      {#each pois as poi}
        <div
          class="poi"
          style="
                transform: translate({poi.x}px, {poi.y}px);
                background-color: {poi.c};
                "
        ></div>
      {/each}
    </div>

    <span class="cursor"></span>
  </div>
</div>

<style>
  .root {
    position: absolute;
    bottom: 0.5rem;
    right: 0.5rem;
    width: 200px;
    height: 200px;
    border: 2px solid black;
    background-color: #12151a;
  }

  .container {
    position: relative;
    width: 100%;
    height: 100%;

    display: flex;
    justify-content: center;
    align-items: center;
    user-select: none;

    overflow: hidden;
  }

  .map {
    position: absolute;
    background-image: url(/map-bg-ai.png);
    left: 0;
    top: 0;
    transform: translate(var(--x, 0px), var(--y, 0px));
    background-blend-mode: overlay;
    background-repeat: no-repeat;
    background-color: #12151a;

    /* background-position-x: var(--x, 0px);
    background-position-y: var(--y, 0px);
     */
  }

  .poi {
    position: absolute;
    top: -5px;
    left: -5px;
    width: 10px;
    height: 10px;
    border: 2px solid black;
  }

  .cursor {
    width: 20px;
    height: 20px;
  }

  .cursor::before {
    content: "";
    display: block;
    width: 20px;
    height: 20px;
    border-radius: 0 50% 50% 50%;
    top: 0px;
    position: relative;
    left: 0px;
    transform: rotate(var(--a, 45deg));
    border: 1px solid yellow;
  }
</style>
