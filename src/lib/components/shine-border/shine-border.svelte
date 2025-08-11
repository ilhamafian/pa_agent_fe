<script lang="ts">
  import { cn } from "$lib/utils";

  type TColorProp = string | string[];

  export let borderRadius: number = 12;
  export let borderWidth: number = 1.5;
  export let duration: number = 14;
  export let color: TColorProp = ["#4FF9FF"];
  let className: string = "";
  export { className as class };
</script>

<div style="--border-radius: {borderRadius}px;" class={cn("relative w-fit rounded-[var(--border-radius)] bg-transparent p-3", className)}>
  <div
    style="
      --border-width: {borderWidth}px;
      --border-radius: {borderRadius}px;
      --shine-pulse-duration: {duration}s;
      --mask-linear-gradient: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
      --background-radial-gradient: radial-gradient(transparent, transparent, {Array.isArray(color) ? color.join(',') : color}, transparent, transparent);
    "
    class="shine-border before:absolute before:inset-0 before:aspect-square before:size-full before:rounded-[var(--border-radius)] before:p-[var(--border-width)] before:will-change-[background-position] before:content-[''] before:![-webkit-mask-composite:xor] before:![mask-composite:exclude] before:[background-image:var(--background-radial-gradient)] before:[background-size:300%_300%] before:[mask:var(--mask-linear-gradient)]"
  ></div>
  <!-- This is Default Slot -->
  <slot>Default</slot>
</div>

<style>
  @keyframes shine-pulse {
    0% {
      background-position: 0% 0%;
    }
    50% {
      background-position: 100% 100%;
    }
    100% {
      background-position: 0% 0%;
    }
  }

  .shine-border::before {
    animation: shine-pulse var(--shine-pulse-duration) infinite linear;
  }
</style>
