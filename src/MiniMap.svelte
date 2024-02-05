<script lang="ts">
  import { onMount } from "svelte";

  const clamp = (value, min, max) => Math.min(max, Math.max(min, value));

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

<div
  style="--x: {(100 - x).toFixed(2)}px; --y: {(100 - y).toFixed(2)}px; --a: {(
    180 -
    (45 - angle)
  ).toFixed(2)}deg"
>
  <span></span>
</div>

<style>
  div {
    display: flex;
    justify-content: center;
    align-items: center;
    user-select: none;
    position: absolute;
    bottom: 0.5rem;
    right: 0.5rem;
    width: 200px;
    height: 200px;
    border: 2px solid black;
    background-color: #12151a;
    background-image: url(/map-bg-ai.png);
    background-position-x: var(--x, 0px);
    background-position-y: var(--y, 0px);
    background-blend-mode: overlay;
    background-repeat: no-repeat;
  }

  span {
    width: 20px;
    height: 20px;
  }

  span::before {
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
