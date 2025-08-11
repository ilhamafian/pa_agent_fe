<script>
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";
  import { page } from "$app/stores";
  import HamburgerMenu from "$lib/components/hamburger/hamburger-menu.svelte";

  export let children;
  let childrenVisible = false;

  onMount(() => {
    const token = localStorage.getItem("token");
    if (!token) {
      goto("/auth/login");
    } else {
      childrenVisible = true;
    }
  });
</script>

<div class="bg-emerald-50 overflow-x-hidden">
  <div class="max-w-lg mx-auto min-h-screen">
    <HamburgerMenu />

    <main>
      {#if childrenVisible}
        {@render children()}
      {:else}
        <p class="text-center py-8 text-gray-400">Checking access...</p>
      {/if}
    </main>
  </div>
</div>
