<script lang="ts">
  import { goto } from "$app/navigation";
  import { PUBLIC_BACKEND_URL } from "$env/static/public";
  import { Switch } from "$lib/components/ui/switch";
  import { onMount } from "svelte";

  onMount(async () => {
    const token = localStorage.getItem("token");
    if (!token) {
      return goto("/authentication/login");
    }

    const res = await fetch(`${PUBLIC_BACKEND_URL}/get_settings_info`, {
      method: "GET",
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${token}`,
      },
    });
  });

  //   Settings state
  let profileData = {
    name: "John Doe",
    email: "john@example.com",
    language: "English",
    profession: "Working Professional",
  };

  let notificationSettings = {
    taskReminders: false,
    calendarReminders: false,
  };

  // Task reminder settings
  let taskReminderSettings = {
    frequency: "daily", // "daily" or "weekly"
    time: "09:00",
    day: "monday",
  };

  let calendarReminderSettings = {
    frequency: "daily", // "daily" or "weekly"
    time: "09:00",
    day: "monday",
  };

  // PIN change state
  let showPinChange = false;
  let pinChangeData = {
    currentPin: "",
    newPin: "",
    confirmPin: "",
  };

  function saveProfile() {
    // Handle profile save
    console.log("Profile saved:", profileData);
  }

  function saveNotifications() {
    // Handle notification settings save
    console.log("Notifications saved:", notificationSettings);
  }

  function changePin() {
    if (pinChangeData.newPin !== pinChangeData.confirmPin) {
      alert("PINs don't match");
      return;
    }
    if (pinChangeData.newPin.length !== 6) {
      alert("PIN must be 6 digits");
      return;
    }
    // Handle PIN change
    console.log("PIN changed");
    showPinChange = false;
    pinChangeData = { currentPin: "", newPin: "", confirmPin: "" };
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

        <button on:click={saveProfile} class="w-full px-4 py-2 bg-emerald-500 text-white rounded-md active:bg-emerald-600 transition-colors"> Save Profile </button>
      </div>
    </div>

    <!-- Notification Settings Card -->
    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
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
            <p class="text-sm font-medium text-gray-900">Task Reminders</p>
            <p class="text-xs text-gray-500">Get notified about upcoming tasks</p>
          </div>
          <Switch bind:checked={notificationSettings.taskReminders} />
        </div>

        {#if notificationSettings.taskReminders}
          <div class="bg-gray-100 rounded-lg p-4 space-y-4">
            <div>
              <label for="reminder-frequency" class="block text-sm font-medium text-gray-700 mb-2">Reminder Frequency</label>
              <select id="reminder-frequency" bind:value={taskReminderSettings.frequency} class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-emerald-500 bg-white">
                <option value="daily">Daily Reminder</option>
                <option value="weekly">Weekly Reminder</option>
              </select>
            </div>

            {#if taskReminderSettings.frequency === "daily"}
              <div>
                <label for="reminder-time" class="block text-sm font-medium text-gray-700 mb-2">Reminder Time</label>
                <input id="reminder-time" type="time" bind:value={taskReminderSettings.time} class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-emerald-500 bg-white" />
                <p class="text-xs text-gray-500 mt-1">When to send daily task reminders</p>
              </div>
            {:else if taskReminderSettings.frequency === "weekly"}
              <div>
                <label for="reminder-day" class="block text-sm font-medium text-gray-700 mb-2">Reminder Day</label>
                <select id="reminder-day" bind:value={taskReminderSettings.day} class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-emerald-500 bg-white">
                  <option value="monday">Monday</option>
                  <option value="tuesday">Tuesday</option>
                  <option value="wednesday">Wednesday</option>
                  <option value="thursday">Thursday</option>
                  <option value="friday">Friday</option>
                  <option value="saturday">Saturday</option>
                  <option value="sunday">Sunday</option>
                </select>
                <p class="text-xs text-gray-500 mt-1">Which day to send weekly task summaries</p>
              </div>
            {/if}
          </div>
        {/if}

        <div class="flex items-center justify-between">
          <div>
            <p class="text-sm font-medium text-gray-900">Calendar Event Reminders</p>
            <p class="text-xs text-gray-500">Get notified about upcoming events</p>
          </div>
          <Switch bind:checked={notificationSettings.calendarReminders} />
        </div>

        {#if notificationSettings.calendarReminders}
          <div class="bg-gray-100 rounded-lg p-4 space-y-4">
            <div>
              <label for="reminder-time" class="block text-sm font-medium text-gray-700 mb-2">Reminder Time</label>
              <input id="reminder-time" type="time" bind:value={calendarReminderSettings.time} class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-emerald-500 bg-white" />
              <p class="text-xs text-gray-500 mt-1">When to send daily calendar reminders</p>
            </div>
          </div>
        {/if}

        <button on:click={saveNotifications} class="w-full px-4 py-2 bg-emerald-500 text-white rounded-md active:bg-emerald-600 transition-colors"> Save Notifications </button>
      </div>
    </div>

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

          <div class="flex gap-2">
            <button on:click={changePin} class="flex-1 px-4 py-2 bg-emerald-500 text-white rounded-md active:bg-emerald-600 transition-colors"> Update PIN </button>
            <button
              on:click={() => {
                showPinChange = false;
                pinChangeData = { currentPin: "", newPin: "", confirmPin: "" };
              }}
              class="px-4 py-2 border border-gray-300 text-gray-700 rounded-md active:bg-gray-50 transition-colors"
            >
              Cancel
            </button>
          </div>
        </div>
      {/if}
    </div>
  </div>
</div>
