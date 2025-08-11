<!-- Dashboard Content -->
<script lang="ts">
  import { PUBLIC_BACKEND_URL } from "$env/static/public";
  import { onMount } from "svelte";
  import { browser } from "$app/environment";
  import { goto } from "$app/navigation";

  interface Event {
    title: string;
    date: string;
    time: string;
  }

  interface Task {
    task_id: string;
    title: string;
    description: string;
    priority: "high" | "medium" | "low";
    status: "pending" | "in_progress" | "completed";
    created_at: string;
  }

  interface DashboardData {
    events: Event[];
    tasks: Task[];
  }

  let dashboardData: DashboardData = { events: [], tasks: [] };
  let loading = true;

  // Reactive calculations for task statistics
  $: totalTasks = dashboardData.tasks.length;
  $: completedTasks = dashboardData.tasks.filter((task) => task.status === "completed").length;
  $: inProgressTasks = dashboardData.tasks.filter((task) => task.status === "in_progress").length;
  $: pendingTasks = dashboardData.tasks.filter((task) => task.status === "pending").length;

  // Get latest 3 events
  $: latestEvents = dashboardData.events
    .sort((a, b) => {
      // Parse DD-MM-YYYY format for proper sorting
      const parseDate = (dateStr: string) => {
        const [day, month, year] = dateStr.split("-");
        return new Date(parseInt(year), parseInt(month) - 1, parseInt(day));
      };
      return parseDate(a.date).getTime() - parseDate(b.date).getTime();
    })
    .slice(0, 3);

  // Format date for display
  function formatEventDate(dateStr: string): string {
    // Parse date string in DD-MM-YYYY format
    const [day, month, year] = dateStr.split("-");
    const date = new Date(parseInt(year), parseInt(month) - 1, parseInt(day));
    const options: Intl.DateTimeFormatOptions = {
      day: "numeric",
      month: "long",
      weekday: "long",
    };
    return date.toLocaleDateString("en-US", options);
  }

  onMount(async () => {
    if (!browser) return;

    const token = localStorage.getItem("token");
    if (!token) {
      return goto("/auth/login");
    }

    try {
      const res = await fetch(`${PUBLIC_BACKEND_URL}/get_dashboard_info`, {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${token}`,
        },
      });

      if (res.ok) {
        dashboardData = await res.json();
        console.log("Dashboard data:", dashboardData);
      } else {
        console.error("Failed to fetch dashboard data");
      }
    } catch (error) {
      console.error("Error fetching dashboard data:", error);
    } finally {
      loading = false;
    }
  });
</script>

<div class="px-4 py-6">
  <!-- Welcome Section -->
  <div class="mb-6">
    <h2 class="text-2xl font-bold text-gray-900 mb-2">Welcome back!</h2>
    <p class="text-gray-600">Here's what's happening with your tasks and events.</p>
  </div>

  <!-- Stats Cards -->
  <div class="grid grid-cols-2 gap-3 mb-6">
    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <div class="flex items-center">
        <div class="flex-shrink-0">
          <div class="w-8 h-8 bg-orange-100 rounded-full flex items-center justify-center">
            <svg class="w-4 h-4 text-orange-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v10a2 2 0 002 2h8a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2" />
            </svg>
          </div>
        </div>
        <div class="ml-3">
          <p class="text-xs font-medium text-gray-600">Total Tasks</p>
          <p class="text-xl font-bold text-gray-900">{loading ? "..." : totalTasks}</p>
        </div>
      </div>
    </div>

    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <div class="flex items-center">
        <div class="flex-shrink-0">
          <div class="w-8 h-8 bg-green-100 rounded-full flex items-center justify-center">
            <svg class="w-4 h-4 text-green-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
            </svg>
          </div>
        </div>
        <div class="ml-3">
          <p class="text-xs font-medium text-gray-600">Completed</p>
          <p class="text-xl font-bold text-gray-900">{loading ? "..." : completedTasks}</p>
        </div>
      </div>
    </div>

    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <div class="flex items-center">
        <div class="flex-shrink-0">
          <div class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center">
            <svg class="w-4 h-4 text-blue-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
          </div>
        </div>
        <div class="ml-3">
          <p class="text-xs font-medium text-gray-600">In Progress</p>
          <p class="text-xl font-bold text-gray-900">{loading ? "..." : inProgressTasks}</p>
        </div>
      </div>
    </div>

    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <div class="flex items-center">
        <div class="flex-shrink-0">
          <div class="w-8 h-8 bg-gray-100 rounded-full flex items-center justify-center">
            <svg class="w-4 h-4 text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
          </div>
        </div>
        <div class="ml-3">
          <p class="text-xs font-medium text-gray-600">Pending</p>
          <p class="text-xl font-bold text-gray-900">{loading ? "..." : pendingTasks}</p>
        </div>
      </div>
    </div>
  </div>

  <!-- Content Sections -->
  <div class="space-y-6">
    <!-- Recent Tasks -->
    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <h3 class="text-lg font-semibold text-gray-900 mb-3">Recent Tasks</h3>
      {#if loading}
        <div class="flex items-center justify-center py-8">
          <div class="text-gray-500">Loading tasks...</div>
        </div>
      {:else if dashboardData.tasks.length === 0}
        <div class="text-center py-8 text-gray-500">
          <p>No tasks yet. Create your first task to get started!</p>
        </div>
      {:else}
        <div class="space-y-3">
          {#each dashboardData.tasks as task}
            <div class="flex items-center p-3 bg-gray-50 rounded-md">
              {#if task.status === "completed"}
                <div class="flex-shrink-0 w-4 h-4 bg-emerald-500 rounded-full flex items-center justify-center">
                  <svg class="w-2.5 h-2.5 text-white" fill="currentColor" viewBox="0 0 20 20">
                    <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd" />
                  </svg>
                </div>
              {:else if task.status === "in_progress"}
                <div class="flex-shrink-0 w-4 h-4 bg-blue-500 rounded-full"></div>
              {:else}
                <div class="flex-shrink-0 w-4 h-4 border-2 border-gray-300 rounded-full"></div>
              {/if}
              <div class="ml-3 flex-1">
                <p class="text-sm font-medium text-gray-900">{task.title}</p>
                <div class="flex items-center space-x-2">
                  <span class="text-xs text-gray-600 capitalize">{task.priority} Priority</span>
                  <span class="text-xs text-gray-400">•</span>
                  <span class="text-xs text-gray-600 capitalize">{task.status.replace("_", " ")}</span>
                </div>
              </div>
            </div>
          {/each}
        </div>
      {/if}
    </div>

    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <h3 class="text-lg font-semibold text-gray-900 mb-3">Upcoming Events</h3>
      {#if loading}
        <div class="flex items-center justify-center py-8">
          <div class="text-gray-500">Loading events...</div>
        </div>
      {:else if latestEvents.length === 0}
        <div class="text-center py-8 text-gray-500">
          <p>No upcoming events. Schedule your first event!</p>
        </div>
      {:else}
        <div class="space-y-3">
          {#each latestEvents as event}
            <div class="flex items-center p-3 bg-gray-50 border-l-4 border-emerald-500">
              <div class="ml-3 flex-1">
                <p class="text-sm font-medium text-gray-900">{event.title}</p>
                <p class="text-xs text-gray-600">{formatEventDate(event.date)}, {event.time}</p>
              </div>
            </div>
          {/each}
        </div>
      {/if}
    </div>

    <!-- Quick Actions -->
    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <h3 class="text-lg font-semibold text-gray-900 mb-3">Quick Actions</h3>
      <div class="grid grid-cols-2 gap-2">
        <button class="flex flex-col items-center p-3 bg-emerald-50 rounded-lg active:bg-emerald-100 transition-colors">
          <svg class="w-5 h-5 text-emerald-600 mb-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6" />
          </svg>
          <span class="text-xs font-medium text-emerald-700">Add Tasks</span>
        </button>

        <button class="flex flex-col items-center p-3 bg-blue-50 rounded-lg active:bg-blue-100 transition-colors">
          <svg class="w-5 h-5 text-blue-600 mb-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
          </svg>
          <span class="text-xs font-medium text-blue-700">Add Events</span>
        </button>

        <button class="flex flex-col items-center p-3 bg-purple-50 rounded-lg active:bg-purple-100 transition-colors">
          <svg class="w-5 h-5 text-purple-600 mb-1" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
          </svg>
          <span class="text-xs font-medium text-purple-700">Add Reminders</span>
        </button>

        <button class="flex flex-col items-center p-3 bg-orange-50 rounded-lg active:bg-orange-100 transition-colors">
          <svg class="w-5 h-5 text-orange-600 mb-1" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M9 12h6m-6 4h6m-7-8h8a2 2 0 012 2v8a2 2 0 01-2 2H7a2 2 0 01-2-2v-8a2 2 0 012-2z" />
            <path d="M9 7h6a2 2 0 002-2v0a2 2 0 00-2-2H9a2 2 0 00-2 2v0a2 2 0 002 2z" />
          </svg>
          <span class="text-xs font-medium text-orange-700">Add Notes</span>
        </button>
      </div>
    </div>
  </div>
</div>
