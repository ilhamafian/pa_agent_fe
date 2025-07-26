<script>
  let { children } = $props();
  import "../app.css";
</script>

<section class="bg-emerald-100">
  <div class="max-w-xl mx-auto min-h-screen p-6">
    {@render children()}
  </div>
</section>
