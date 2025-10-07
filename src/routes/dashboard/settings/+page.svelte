<script lang="ts">
  import { goto } from "$app/navigation";
  import { PUBLIC_BACKEND_URL } from "$env/static/public";
  import { Switch } from "$lib/components/switch";
  import { onMount } from "svelte";

  // Settings state
  let profileData = {
    name: "",
    email: "",
    language: "",
  };
  let dailyBriefing = {
    enabled: false,
    time: "",
  };
  // Get settings info
  onMount(async () => {
    const token = localStorage.getItem("token");
    const user_id = localStorage.getItem("user_id");
    if (!token) {
      return goto("/auth/login");
    }

    const res = await fetch(`${PUBLIC_BACKEND_URL}/get_settings_info?user_id=${user_id}`, {
      method: "GET",
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${token}`,
      },
    });

    const data = await res.json();

    profileData = {
      name: data.name,
      email: data.email,
      language: data.language,
    };

    dailyBriefing = {
      enabled: data.daily_briefing.enabled,
      time: String(data.daily_briefing.time)
        .padStart(4, "0") // Ensure 4 digits
        .replace(/(\d{2})(\d{2})/, "$1:$2"), // Insert colon
    };
  });

  // Loading states
  let profileSaving = false;
  let notificationsSaving = false;
  let profileError = "";
  let notificationsError = "";
  let profileSuccess = false;
  let notificationsSuccess = false;

  async function saveProfile() {
    profileSaving = true;
    profileError = "";
    profileSuccess = false;

    try {
      const token = localStorage.getItem("token");
      const user_id = localStorage.getItem("user_id");

      if (!token) {
        goto("/auth/login");
        return;
      }

      const response = await fetch(`${PUBLIC_BACKEND_URL}/update_profile`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${token}`,
        },
        body: JSON.stringify({
          user_id,
          name: profileData.name,
          language: profileData.language,
        }),
      });

      if (!response.ok) {
        const errorData = await response.json();
        throw new Error(errorData.message || "Failed to save profile");
      }

      profileSuccess = true;
      setTimeout(() => {
        profileSuccess = false;
      }, 3000);
    } catch (error) {
      profileError = error instanceof Error ? error.message : "Failed to save profile";
    } finally {
      profileSaving = false;
    }
  }

  async function saveNotifications() {
    notificationsSaving = true;
    notificationsError = "";
    notificationsSuccess = false;

    try {
      const token = localStorage.getItem("token");
      const user_id = localStorage.getItem("user_id");

      if (!token) {
        goto("/auth/login");
        return;
      }
      // Convert time back to integer format (e.g., "09:30" -> 930)
      let timeInteger = 0;
      if (dailyBriefing.time) {
        timeInteger = parseInt(dailyBriefing.time.replace(":", ""));
      }

      const response = await fetch(`${PUBLIC_BACKEND_URL}/update_notifications`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${token}`,
        },
        body: JSON.stringify({
          user_id,
          daily_briefing: {
            enabled: dailyBriefing.enabled,
            time: timeInteger,
          },
        }),
      });

      if (!response.ok) {
        const errorData = await response.json();
        throw new Error(errorData.message || "Failed to save notification settings");
      }

      notificationsSuccess = true;
      setTimeout(() => {
        notificationsSuccess = false;
      }, 3000);
    } catch (error) {
      notificationsError = error instanceof Error ? error.message : "Failed to save notification settings";
    } finally {
      notificationsSaving = false;
    }
  }

  // PIN change state
  let showPinChange = false;
  let pinChangeData = {
    currentPin: "",
    newPin: "",
    confirmPin: "",
  };
  let pinChanging = false;
  let pinError = "";
  let pinSuccess = false;

  async function changePin() {
    if (pinChangeData.newPin !== pinChangeData.confirmPin) {
      pinError = "PINs don't match";
      return;
    }
    if (pinChangeData.newPin.length !== 6) {
      pinError = "PIN must be 6 digits";
      return;
    }

    pinChanging = true;
    pinError = "";
    pinSuccess = false;

    try {
      const token = localStorage.getItem("token");
      const user_id = localStorage.getItem("user_id");

      if (!token) {
        goto("/auth/login");
        return;
      }

      const response = await fetch(`${PUBLIC_BACKEND_URL}/change_pin`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${token}`,
        },
        body: JSON.stringify({
          user_id,
          current_pin: pinChangeData.currentPin,
          new_pin: pinChangeData.newPin,
        }),
      });

      if (!response.ok) {
        const errorData = await response.json();
        throw new Error(errorData.message || "Failed to change PIN");
      }

      pinSuccess = true;
      showPinChange = false;
      pinChangeData = { currentPin: "", newPin: "", confirmPin: "" };

      setTimeout(() => {
        pinSuccess = false;
      }, 3000);
    } catch (error) {
      pinError = error instanceof Error ? error.message : "Failed to change PIN";
    } finally {
      pinChanging = false;
    }
  }
</script>

<!-- Settings Content -->
<div class="px-4 py-6">
  <!-- Header -->
  <div class="mb-6">
    <h2 class="text-2xl font-bold text-gray-900 mb-2">Settings</h2>
    <p class="text-gray-600">Manage your account preferences and security settings.</p>
  </div>

  <!-- Settings Cards -->
  <div class="space-y-4">
    <!-- Profile Information Card -->
    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <div class="flex items-center mb-4">
        <div class="w-8 h-8 bg-emerald-100 rounded-full flex items-center justify-center mr-3">
          <svg class="w-4 h-4 text-emerald-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
          </svg>
        </div>
        <h3 class="text-lg font-semibold text-gray-900">Profile Information</h3>
      </div>

      <div class="space-y-4">
        <div>
          <label for="profile-name" class="block text-sm font-medium text-gray-700 mb-1">Name</label>
          <input id="profile-name" type="text" bind:value={profileData.name} class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-emerald-500" />
        </div>

        <div>
          <label for="profile-email" class="block text-sm font-medium text-gray-700 mb-1">Email</label>
          <input id="profile-email" type="email" bind:value={profileData.email} class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-emerald-500" />
        </div>

        <div>
          <label for="profile-language" class="block text-sm font-medium text-gray-700 mb-1">Language</label>
          <select id="profile-language" bind:value={profileData.language} class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-emerald-500">
            <option value="English">English</option>
            <option value="Spanish">Spanish</option>
            <option value="French">French</option>
            <option value="German">German</option>
          </select>
        </div>

        <!-- Profile Error/Success Messages -->
        {#if profileError}
          <div class="bg-red-50 border border-red-200 text-red-700 px-3 py-2 rounded-md text-sm">
            {profileError}
          </div>
        {/if}

        {#if profileSuccess}
          <div class="bg-green-50 border border-green-200 text-green-700 px-3 py-2 rounded-md text-sm">Profile saved successfully!</div>
        {/if}

        <button on:click={saveProfile} disabled={profileSaving} class="w-full px-4 py-2 bg-emerald-500 text-white rounded-md active:bg-emerald-600 transition-colors disabled:opacity-50 disabled:cursor-not-allowed">
          {profileSaving ? "Saving..." : "Save Profile"}
        </button>
      </div>
    </div>

    <!-- Notification Settings Card -->
    <!-- <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <div class="flex items-center mb-4">
        <div class="w-8 h-8 bg-orange-100 rounded-full flex items-center justify-center mr-3">
          <svg class="w-4 h-4 text-orange-600" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" d="M14.857 17.082a23.848 23.848 0 0 0 5.454-1.31A8.967 8.967 0 0 1 18 9.75V9A6 6 0 0 0 6 9v.75a8.967 8.967 0 0 1-2.312 6.022c1.733.64 3.56 1.085 5.455 1.31m5.714 0a24.255 24.255 0 0 1-5.714 0m5.714 0a3 3 0 1 1-5.714 0" />
          </svg>
        </div>
        <h3 class="text-lg font-semibold text-gray-900">Notification Settings</h3>
      </div>

      <div class="space-y-4">
        <div class="flex items-center justify-between">
          <div>
            <p class="text-sm font-medium text-gray-900">Daily briefing</p>
            <p class="text-xs text-gray-500">Get notified about upcoming events</p>
          </div>
          <Switch bind:checked={dailyBriefing.enabled} />
        </div>

        {#if dailyBriefing.enabled}
          <div class="bg-gray-100 rounded-lg p-4 space-y-4">
            <div>
              <label for="reminder-time" class="block text-sm font-medium text-gray-700 mb-2">Reminder Time</label>
              <input id="reminder-time" type="time" bind:value={dailyBriefing.time} class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-emerald-500 bg-white" />
              <p class="text-xs text-gray-500 mt-1">When to send daily calendar reminders</p>
            </div>
          </div>
        {/if}

        {#if notificationsError}
          <div class="bg-red-50 border border-red-200 text-red-700 px-3 py-2 rounded-md text-sm">
            {notificationsError}
          </div>
        {/if}

        {#if notificationsSuccess}
          <div class="bg-green-50 border border-green-200 text-green-700 px-3 py-2 rounded-md text-sm">Notification settings saved successfully!</div>
        {/if}

        <button on:click={saveNotifications} disabled={notificationsSaving} class="w-full px-4 py-2 bg-emerald-500 text-white rounded-md active:bg-emerald-600 transition-colors disabled:opacity-50 disabled:cursor-not-allowed">
          {notificationsSaving ? "Saving..." : "Save Notifications"}
        </button>
      </div>
    </div> -->

    <!-- PIN Change Card -->
    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <div class="flex items-center mb-4">
        <div class="w-8 h-8 bg-red-100 rounded-full flex items-center justify-center mr-3">
          <svg class="w-4 h-4 text-red-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" />
          </svg>
        </div>
        <h3 class="text-lg font-semibold text-gray-900">Security Settings</h3>
      </div>

      {#if !showPinChange}
        <div>
          <p class="text-sm text-gray-600 mb-4">Keep your account secure by updating your PIN regularly.</p>

          <!-- PIN Success Message -->
          {#if pinSuccess}
            <div class="bg-green-50 border border-green-200 text-green-700 px-3 py-2 rounded-md text-sm mb-4">PIN changed successfully!</div>
          {/if}

          <button on:click={() => (showPinChange = true)} class="w-full px-4 py-2 bg-red-500 text-white rounded-md active:bg-red-600 transition-colors"> Change PIN </button>
        </div>
      {:else}
        <div class="space-y-4">
          <div>
            <label for="current-pin" class="block text-sm font-medium text-gray-700 mb-1">Current PIN</label>
            <input id="current-pin" type="password" maxlength="6" inputmode="numeric" bind:value={pinChangeData.currentPin} class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-emerald-500" placeholder="Enter current PIN" />
          </div>

          <div>
            <label for="new-pin" class="block text-sm font-medium text-gray-700 mb-1">New PIN</label>
            <input id="new-pin" type="password" maxlength="6" inputmode="numeric" bind:value={pinChangeData.newPin} class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-emerald-500" placeholder="Enter new 6-digit PIN" />
          </div>

          <div>
            <label for="confirm-pin" class="block text-sm font-medium text-gray-700 mb-1">Confirm New PIN</label>
            <input id="confirm-pin" type="password" maxlength="6" inputmode="numeric" bind:value={pinChangeData.confirmPin} class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-emerald-500" placeholder="Confirm new PIN" />
          </div>

          <!-- PIN Error Message -->
          {#if pinError}
            <div class="bg-red-50 border border-red-200 text-red-700 px-3 py-2 rounded-md text-sm">
              {pinError}
            </div>
          {/if}

          <div class="flex gap-2">
            <button on:click={changePin} disabled={pinChanging} class="flex-1 px-4 py-2 bg-emerald-500 text-white rounded-md active:bg-emerald-600 transition-colors disabled:opacity-50 disabled:cursor-not-allowed">
              {pinChanging ? "Updating..." : "Update PIN"}
            </button>
            <button
              on:click={() => {
                showPinChange = false;
                pinChangeData = { currentPin: "", newPin: "", confirmPin: "" };
                pinError = "";
              }}
              disabled={pinChanging}
              class="px-4 py-2 border border-gray-300 text-gray-700 rounded-md active:bg-gray-50 transition-colors disabled:opacity-50 disabled:cursor-not-allowed"
            >
              Cancel
            </button>
          </div>
        </div>
      {/if}
    </div>
  </div>
</div>
