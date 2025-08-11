<script lang="ts">
  import { page } from "$app/stores";

  $: url = $page.url;
  $: status = url.searchParams.get("status"); // 'success' or 'error'
  $: reason = url.searchParams.get("reason"); // optional
</script>

<h1 class="text-2xl font-bold text-center">Google OAuth Result</h1>

{#if status === "success"}
  <div class="flex flex-col gap-4 border-2 border-emerald-500 rounded-xl max-h-78 text-center p-4 m-2">
    <p class="text-gray-800">Login successful! You're all set.</p>
    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-12 mx-auto text-emerald-500">
      <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
    </svg>
    <!-- <a href="https://wa.me/15550437601" target="_blank" rel="noopener noreferrer" class="px-4 py-2 cursor-pointer hover:bg-emerald-400 ring-2 ring-emerald-400 bg-transparent rounded-md min-w-48 mx-auto block text-center">
      <span class="flex gap-3 items-center justify-center">
        <p>Redirect to WhatsApp</p>
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-5">
          <path stroke-linecap="round" stroke-linejoin="round" d="M6 12 3.269 3.125A59.769 59.769 0 0 1 21.485 12 59.768 59.768 0 0 1 3.27 20.875L5.999 12Zm0 0h7.5" />
        </svg>
      </span>
    </a> -->
    <a href="https://wa.me/15550437601" target="_blank" rel="noopener noreferrer" class="px-4 py-2 cursor-pointer hover:bg-emerald-400 ring-2 ring-emerald-400 bg-transparent rounded-md min-w-48 mx-auto block text-center">
      <span class="flex gap-3 items-center justify-center">
        <p>Dashboard</p>
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-5">
          <path stroke-linecap="round" stroke-linejoin="round" d="M6 12 3.269 3.125A59.769 59.769 0 0 1 21.485 12 59.768 59.768 0 0 1 3.27 20.875L5.999 12Zm0 0h7.5" />
        </svg>
      </span>
    </a>
  </div>
{:else if status === "error"}
  <p class="text-red-600">
    ❌ Login failed. Reason: {reason ?? "unknown error"}
  </p>
{:else}
  <p>Waiting for redirect result...</p>
{/if}
