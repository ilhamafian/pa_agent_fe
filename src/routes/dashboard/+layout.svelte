<script>
  let { children } = $props();
  import HamburgerMenu from "$lib/components/HamburgerMenu.svelte";
</script>

<div class="bg-emerald-50">
  <div class=" max-w-lg mx-auto min-h-screen">
    <HamburgerMenu />
    <main>
      {@render children()}
    </main>
  </div>
</div>
