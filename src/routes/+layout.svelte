<script>
  let { children } = $props();
  import "../app.css";
</script>

<div class="bg-emerald-50 overflow-x-hidden">
  <div class=" max-w-lg mx-auto py-8 min-h-screen">
    <main>
      {@render children()}
    </main>
  </div>
</div>
