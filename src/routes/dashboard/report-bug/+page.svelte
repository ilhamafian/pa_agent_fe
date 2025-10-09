<script lang="ts">
  import * as Dialog from "$lib/components/dialog";
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";
  import { PUBLIC_BACKEND_URL } from "$env/static/public";

  const token = localStorage.getItem("token");
  const user_id = localStorage.getItem("user_id");

  let title = "";
  let description = "";
  let isSubmitting = false;
  let showSuccessDialog = false;
  let showErrorDialog = false;
  let errorMessage = "";

  onMount(async () => {
    if (!token) {
      return goto("/auth/login");
    }
  });

  async function handleSubmit() {
    if (!title.trim() || !description.trim()) {
      errorMessage = "Please fill in all fields";
      showErrorDialog = true;
      return;
    }

    isSubmitting = true;

    try {
      const res = await fetch(`${PUBLIC_BACKEND_URL}/report_bug`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${token}`,
        },
        body: JSON.stringify({
          user_id,
          title: title.trim(),
          description: description.trim(),
        }),
      });

      if (!res.ok) {
        throw new Error("Failed to submit bug report");
      }

      showSuccessDialog = true;
      title = "";
      description = "";
    } catch (error) {
      if (error instanceof Error) {
        errorMessage = error.message;
      } else {
        errorMessage = "An error occurred while submitting the bug report";
      }
      showErrorDialog = true;
    } finally {
      isSubmitting = false;
    }
  }

  function handleSuccessDialogClose() {
    showSuccessDialog = false;
  }

  function handleErrorDialogClose() {
    showErrorDialog = false;
  }
</script>

<div class="px-4 py-6">
  <!-- Header Section -->
  <div class="mb-6">
    <h2 class="text-2xl font-bold text-gray-900 mb-2">Report a Bug</h2>
    <p class="text-gray-600">Help us improve by reporting any issues you encounter.</p>
  </div>

  <div class="space-y-4">
    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <div class="space-y-4">
        <form on:submit|preventDefault={handleSubmit} class="space-y-6">
          <!-- Title Field -->
          <div>
            <label for="title" class="block text-md font-medium text-gray-700 mb-1">Title</label>
            <input type="text" id="title" bind:value={title} class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-emerald-500 focus:border-emerald-500" placeholder="Brief description of the issue" />
          </div>

          <!-- Description Field -->
          <div>
            <label for="description" class="block text-md font-medium text-gray-700 mb-1">Description</label>
            <textarea id="description" bind:value={description} rows="6" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-emerald-500 focus:border-emerald-500" placeholder="Please provide detailed steps to reproduce the issue and any relevant information"></textarea>
          </div>

          <!-- Submit Button -->
          <div>
            <button type="submit" class="w-full px-4 py-2 text-sm bg-emerald-500 text-white rounded-md hover:bg-emerald-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500 disabled:opacity-50 disabled:cursor-not-allowed transition-colors" disabled={isSubmitting}>
              {isSubmitting ? "Submitting..." : "Submit Bug Report"}
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</div>

<!-- Success Dialog -->
<Dialog.Root bind:open={showSuccessDialog} onOpenChange={handleSuccessDialogClose}>
  <Dialog.Content>
    <Dialog.Header>
      <Dialog.Title class="text-lg font-semibold text-gray-900">Bug Report Submitted</Dialog.Title>
      <Dialog.Description class="text-sm text-gray-600">Thank you for helping us improve our product. We'll review your report and take appropriate action.</Dialog.Description>
    </Dialog.Header>
    <Dialog.Footer>
      <button class="w-full px-4 py-2 text-sm bg-emerald-500 text-white rounded-md hover:bg-emerald-600 transition-colors" on:click={handleSuccessDialogClose}> Close </button>
    </Dialog.Footer>
  </Dialog.Content>
</Dialog.Root>

<!-- Error Dialog -->
<Dialog.Root bind:open={showErrorDialog} onOpenChange={handleErrorDialogClose}>
  <Dialog.Content>
    <Dialog.Header>
      <Dialog.Title class="text-lg font-semibold text-gray-900">Error</Dialog.Title>
      <Dialog.Description class="text-sm text-gray-600">
        {errorMessage}
      </Dialog.Description>
    </Dialog.Header>
    <Dialog.Footer>
      <button class="w-full px-4 py-2 text-sm bg-red-500 text-white rounded-md hover:bg-red-600 transition-colors" on:click={handleErrorDialogClose}> Close </button>
    </Dialog.Footer>
  </Dialog.Content>
</Dialog.Root>
