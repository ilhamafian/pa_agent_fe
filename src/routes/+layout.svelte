<script>
  let { children } = $props();
  import "../app.css";
</script>

<div class="bg-emerald-50 overflow-x-hidden">
  <div class="w-full min-h-screen">
    <main>
      {@render children()}
    </main>
  </div>
</div>
