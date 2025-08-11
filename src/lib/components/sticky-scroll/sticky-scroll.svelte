<script lang="ts">
  import { onMount } from "svelte";
  import ShineBorder from "$lib/components/shine-border/shine-border.svelte";

  let activeCard = 0;
  let backgroundColors = ["var(--slate-900)", "var(--black)", "var(--neutral-900)"];
  let gifImages = ["/gif-assets/1.gif", "/gif-assets/2.gif", "/gif-assets/1.gif", "/gif-assets/2.gif"];

  export let content: { title: string; description: string }[] = [
    { title: "Title 1", description: "Description 1" },
    { title: "Title 2", description: "Description 2" },
    { title: "Title 3", description: "Description 3" },
    { title: "Title 4", description: "Description 4" },
    // Add more items as needed
  ];

  let ref: HTMLDivElement;

  // Simplified scroll function
  function handleScroll() {
    if (!ref) return;

    const scrollTop = ref.scrollTop;
    const scrollHeight = ref.scrollHeight;
    const clientHeight = ref.clientHeight;
    const maxScroll = scrollHeight - clientHeight;

    // Simple calculation: divide scroll into equal sections
    const cardHeight = maxScroll / content.length;
    const newActiveCard = Math.min(content.length - 1, Math.max(0, Math.floor(scrollTop / cardHeight)));

    if (newActiveCard !== activeCard) {
      activeCard = newActiveCard;
    }
  }

  // Auto-center the component when mouse enters
  function handleMouseEnter() {
    if (!ref) return;

    const rect = ref.getBoundingClientRect();
    const elementCenter = rect.top + window.pageYOffset + rect.height / 2;
    const viewportCenter = window.innerHeight / 2;
    const offsetPixels = 75; // Adjust this value as needed

    const targetScrollPosition = elementCenter - viewportCenter - offsetPixels;

    window.scrollTo({
      top: targetScrollPosition,
      behavior: "smooth",
    });
  }

  onMount(() => {
    if (!ref) return;

    ref.addEventListener("scroll", handleScroll, { passive: true });

    return () => {
      ref.removeEventListener("scroll", handleScroll);
    };
  });
</script>

<div
  bind:this={ref}
  on:mouseenter={handleMouseEnter}
  role="region"
  aria-label="Sticky scroll content"
  style="background-color: {backgroundColors[activeCard % backgroundColors.length]}"
  class="relative max-w-5xl mx-auto flex justify-items-center h-[50rem] justify-between space-x-10 overflow-y-auto rounded-md p-2 transition-all duration-700 ease-in-out scrollbar-hide"
>
  <div class="div relative flex items-start px-4">
    <div class="py-12">
      {#each content as item, index (item.title + index)}
        <div class="min-h-[30rem] flex flex-col justify-center mb-8">
          <ShineBorder class="p-8 w-fit" color={["oklch(69.6% 0.17 162.48)", "oklch(71.5% 0.143 215.221)", "oklch(68.5% 0.169 237.323)"]}>
            <h2 style="opacity: {activeCard === index ? 1 : 0.3}" class="text-2xl font-bold text-slate-700 transition-opacity duration-500 ease-in-out">
              {item.title}
            </h2>
            <p style="opacity: {activeCard === index ? 1 : 0.3}" class="text-base mt-4 max-w-sm text-slate-700 transition-opacity duration-500 ease-in-out">
              {item.description}
            </p>
          </ShineBorder>
        </div>
      {/each}
    </div>
  </div>

  <div class="sticky top-10 hidden h-[30rem] w-fit overflow-hidden rounded-md bg-white lg:block">
    {#key activeCard}
      <img src={gifImages[activeCard % gifImages.length]} alt="Dynamic content for card {activeCard + 1}" class="w-full h-full object-cover rounded-md transition-all duration-700 ease-in-out transform" style="animation: fadeInScale 0.7s ease-in-out;" />
    {/key}
  </div>
</div>

<style>
  .scrollbar-hide {
    -ms-overflow-style: none;
    scrollbar-width: none;
  }

  .scrollbar-hide::-webkit-scrollbar {
    display: none;
  }

  @keyframes fadeInScale {
    0% {
      opacity: 0;
      transform: scale(0.95);
    }
    100% {
      opacity: 1;
      transform: scale(1);
    }
  }
</style>
