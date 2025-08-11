<script lang="ts">
  import { Switch } from "$lib/components/switch";
  import * as Dialog from "$lib/components/dialog";
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";
  import { PUBLIC_BACKEND_URL } from "$env/static/public";

  const token = localStorage.getItem("token");
  const user_id = localStorage.getItem("user_id");

  let googleCalendarEnabled = false;
  let showGoogleCalendarDialog = false;

  onMount(async () => {
    if (!token) {
      return goto("/auth/login");
    }

    const res = await fetch(`${PUBLIC_BACKEND_URL}/get_integrations?user_id=${user_id}`, {
      method: "GET",
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${token}`,
      },
    });

    const data = await res.json();
    console.log(data);

    googleCalendarEnabled = data.google_calendar.enabled;
  });

  function handleSwitchToggle(integration: string, enabled: boolean) {
    switch (integration) {
      case "google-calendar":
        googleCalendarEnabled = enabled;
        if (enabled) showGoogleCalendarDialog = true;
        break;
    }
  }

  function handleDialogClose(open: boolean) {
    if (!open) {
      // Dialog was closed without completing setup
      googleCalendarEnabled = false;
      showGoogleCalendarDialog = false;
    }
  }

  async function handleConnect() {
    const res = await fetch(`${PUBLIC_BACKEND_URL}/google_auth_url`, {
      method: "GET", // You can use GET since no body is sent
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${token}`,
      },
    });

    const data = await res.json();
    window.location.href = data.auth_url;
    showGoogleCalendarDialog = false;
  }

  function handleCancel() {
    // User explicitly cancelled
    googleCalendarEnabled = false;
    showGoogleCalendarDialog = false;
  }
</script>

<!-- Dashboard Content -->
<div class="px-4 py-6">
  <!-- Welcome Section -->
  <div class="mb-6">
    <h2 class="text-2xl font-bold text-gray-900 mb-2">Tools Integration</h2>
    <p class="text-gray-600">Allow Lofy to connect to your other applications.</p>
  </div>

  <!-- Integration Cards -->
  <div class="flex flex-col gap-3 mb-6">
    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <div class="flex items-center">
        <div class="flex-shrink-0">
          <div class="w-8 h-8 bg-emerald-100 rounded-full flex items-center justify-center">
            <svg class="w-4 h-4 text-emerald-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v10a2 2 0 002 2h8a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2" />
            </svg>
          </div>
        </div>
        <div class="ml-3">
          <p class="text-sm font-medium text-gray-600">Google Calendar</p>
        </div>
        <div class="ml-auto">
          <Switch checked={googleCalendarEnabled} onCheckedChange={(checked) => handleSwitchToggle("google-calendar", checked)} />
        </div>
      </div>
    </div>

    <!-- <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <div class="flex items-center">
        <div class="flex-shrink-0">
          <div class="w-8 h-8 bg-green-100 rounded-full flex items-center justify-center">
            <svg class="w-4 h-4 text-green-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
            </svg>
          </div>
        </div>
        <div class="ml-3">
          <p class="text-sm font-medium text-gray-600">Task Management</p>
        </div>
        <div class="ml-auto">
          <Switch checked={taskManagementEnabled} onCheckedChange={(checked) => handleSwitchToggle("task-management", checked)} />
        </div>
      </div>
    </div>

    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <div class="flex items-center">
        <div class="flex-shrink-0">
          <div class="w-8 h-8 bg-yellow-100 rounded-full flex items-center justify-center">
            <svg class="w-4 h-4 text-yellow-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
          </div>
        </div>
        <div class="ml-3">
          <p class="text-sm font-medium text-gray-600">Password Keeper</p>
        </div>
        <div class="ml-auto">
          <Switch />
        </div>
      </div>
    </div> -->
  </div>
</div>

<!-- Google Calendar Dialog -->
<Dialog.Root bind:open={showGoogleCalendarDialog} onOpenChange={handleDialogClose}>
  <Dialog.Content>
    <Dialog.Header>
      <Dialog.Title class="text-lg font-semibold text-gray-900">Google Calendar Integration</Dialog.Title>
      <Dialog.Description class="text-sm text-gray-600">Connect your Google Calendar to sync events and schedules.</Dialog.Description>
    </Dialog.Header>
    <div class="py-4">
      <div class="space-y-4">
        <div class="flex items-center space-x-3">
          <div class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center">
            <svg class="w-4 h-4 text-blue-600" fill="currentColor" viewBox="0 0 24 24">
              <path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" />
              <path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" />
              <path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z" />
              <path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" />
            </svg>
          </div>
          <div>
            <p class="text-sm font-medium text-gray-900">Connect with Google</p>
            <p class="text-xs text-gray-500">Authorize calendar access</p>
          </div>
        </div>
      </div>
    </div>
    <Dialog.Footer class="flex gap-2">
      <button class="flex-1 px-4 py-2 text-sm bg-emerald-500 text-white rounded-md active:bg-emerald-600 transition-colors" on:click={handleConnect}> Connect </button>
      <button class="px-4 py-2 text-sm border border-gray-300 text-gray-700 rounded-md active:bg-gray-50 transition-colors" on:click={handleCancel}> Cancel </button>
    </Dialog.Footer>
  </Dialog.Content>
</Dialog.Root>

<!-- Task Management Dialog -->
<!-- <Dialog.Root bind:open={showTaskManagementDialog} onOpenChange={handleDialogClose}>
  <Dialog.Content>
    <Dialog.Header>
      <Dialog.Title class="text-lg font-semibold text-gray-900">Task Manager Integration</Dialog.Title>
      <Dialog.Description class="text-sm text-gray-600">Handle your tasks and projects with ease.</Dialog.Description>
    </Dialog.Header>
    <div class="py-4">
      <div class="space-y-4">
        <div class="flex items-center space-x-3">
          <div class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center">
            <svg class="w-4 h-4 text-blue-600" fill="currentColor" viewBox="0 0 24 24">
              <path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" />
              <path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" />
              <path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z" />
              <path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" />
            </svg>
          </div>
          <div>
            <p class="text-sm font-medium text-gray-900">Connect with Google</p>
            <p class="text-xs text-gray-500">Authorize calendar access</p>
          </div>
        </div>
      </div>
    </div>
    <Dialog.Footer class="flex gap-2">
      <button class="flex-1 px-4 py-2 text-sm bg-emerald-500 text-white rounded-md active:bg-emerald-600 transition-colors" on:click={handleConnect}> Connect </button>
      <button class="px-4 py-2 text-sm border border-gray-300 text-gray-700 rounded-md active:bg-gray-50 transition-colors" on:click={handleCancel}> Cancel </button>
    </Dialog.Footer>
  </Dialog.Content>
</Dialog.Root> -->
