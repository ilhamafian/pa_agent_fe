<script lang="ts">
  import { onDestroy } from "svelte";
  import { browser } from "$app/environment";
  import { goto } from "$app/navigation";
  import { page } from "$app/stores";

  let isMenuOpen = false;

  function toggleMenu() {
    isMenuOpen = !isMenuOpen;
  }

  function closeMenu() {
    isMenuOpen = false;
  }

  // Close menu when clicking outside
  function handleClickOutside(event: Event) {
    if (!browser) return;

    const target = event.target as Element;
    const menuButton = document.getElementById("menu-button");
    const menu = document.getElementById("mobile-menu");

    if (isMenuOpen && menuButton && menu) {
      if (!menuButton.contains(target) && !menu.contains(target)) {
        isMenuOpen = false;
      }
    }
  }

  // Add/remove event listener when menu state changes
  $: if (browser) {
    if (isMenuOpen) {
      document.addEventListener("click", handleClickOutside);
    } else {
      document.removeEventListener("click", handleClickOutside);
    }
  }

  // Logout function
  function logout() {
    // Clear all localStorage items
    if (browser) {
      localStorage.clear();
    }

    // Close the menu
    closeMenu();

    // Redirect to login page
    goto("/auth/login");
  }

  // Cleanup on component destroy
  onDestroy(() => {
    if (browser) {
      document.removeEventListener("click", handleClickOutside);
    }
  });
</script>

<!-- Header with Hamburger Menu -->
<header class="bg-white shadow-sm border-b border-emerald-100">
  <div class="px-4">
    <div class="flex justify-between items-center h-16">
      <!-- Logo/Brand -->
      <div class="flex items-center gap-1">
        <img src="/lofy-logo.png" alt="Lofy Logo" class="w-10 h-10" />
        <h1 class="text-xl font-bold text-emerald-600">Lofy Assistant</h1>
      </div>
      <div class="flex items-center space-x-4">
        <!-- Link to Whatsapp -->
        <a href="https://wa.me/60178230685" target="_blank" rel="noopener noreferrer" class="border-2 border-emerald-500 text-emerald-600 px-2 py-1 rounded-md">
          <span class="flex gap-2 items-center">
            <p class="text-xs">Chat with Lofy</p>
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-4">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M8.625 9.75a.375.375 0 1 1-.75 0 .375.375 0 0 1 .75 0Zm0 0H8.25m4.125 0a.375.375 0 1 1-.75 0 .375.375 0 0 1 .75 0Zm0 0H12m4.125 0a.375.375 0 1 1-.75 0 .375.375 0 0 1 .75 0Zm0 0h-.375m-13.5 3.01c0 1.6 1.123 2.994 2.707 3.227 1.087.16 2.185.283 3.293.369V21l4.184-4.183a1.14 1.14 0 0 1 .778-.332 48.294 48.294 0 0 0 5.83-.498c1.585-.233 2.708-1.626 2.708-3.228V6.741c0-1.602-1.123-2.995-2.707-3.228A48.394 48.394 0 0 0 12 3c-2.392 0-4.744.175-7.043.513C3.373 3.746 2.25 5.14 2.25 6.741v6.018Z"
              />
            </svg>
          </span>
        </a>
        <!-- Hamburger Menu Button -->
        <button id="menu-button" on:click={toggleMenu} class="inline-flex items-center justify-center p-2 rounded-md text-gray-700 hover:text-emerald-600 hover:bg-emerald-50 focus:outline-none focus:ring-2 focus:ring-emerald-500 transition-colors" aria-expanded={isMenuOpen}>
          <span class="sr-only">Open main menu</span>
          <!-- Hamburger Icon -->
          <svg class="h-6 w-6 transition-transform duration-200 {isMenuOpen ? 'rotate-90' : ''}" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            {#if !isMenuOpen}
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
            {:else}
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            {/if}
          </svg>
        </button>
      </div>
    </div>
  </div>

  <!-- Menu Overlay -->
  {#if isMenuOpen}
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div class="fixed inset-0 z-50 bg-black/50 transition-opacity" on:click={closeMenu}></div>
  {/if}

  <!-- Menu Slide-out -->
  <div id="mobile-menu" class="fixed top-0 right-0 z-50 h-full w-64 bg-white shadow-xl transform transition-transform duration-300 ease-in-out {isMenuOpen ? 'translate-x-0' : 'translate-x-full'}">
    <div class="flex flex-col h-full">
      <!-- Menu Header -->
      <div class="flex items-center justify-between p-4 border-b border-emerald-100">
        <h2 class="text-lg font-semibold text-emerald-600">Menu</h2>
        <button on:click={closeMenu} class="p-2 rounded-md text-gray-500 hover:text-emerald-600 hover:bg-emerald-50 transition-colors" aria-label="Close menu">
          <svg class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>

      <!-- Menu Items -->
      <nav class="flex-1 px-4 py-6 space-y-4">
        <a
          href="/dashboard"
          class={`flex items-center px-3 py-2 rounded-md transition-colors
          ${$page.url.pathname === "/dashboard" ? "bg-emerald-100 text-emerald-700 font-medium" : "text-gray-700 hover:bg-emerald-50 hover:text-emerald-600"}`}
          on:click={closeMenu}
        >
          <svg class="mr-3 h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 7v10a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2H5a2 2 0 00-2-2z" />
          </svg>
          Dashboard
        </a>

        <a
          href="/dashboard/integration"
          class={`flex items-center w-full px-3 py-2 rounded-md transition-colors text-left
          ${$page.url.pathname.startsWith("/dashboard/integration") ? "bg-emerald-100 text-emerald-700 font-medium" : "text-gray-700 hover:bg-emerald-50 hover:text-emerald-600"}`}
          on:click={closeMenu}
        >
          <svg class="mr-3 h-5 w-5" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" d="M13.19 8.688a4.5 4.5 0 0 1 1.242 7.244l-4.5 4.5a4.5 4.5 0 0 1-6.364-6.364l1.757-1.757m13.35-.622 1.757-1.757a4.5 4.5 0 0 0-6.364-6.364l-4.5 4.5a4.5 4.5 0 0 0 1.242 7.244" />
          </svg>
          Integration
        </a>

        <a
          href="/dashboard/settings"
          class={`flex items-center w-full px-3 py-2 rounded-md transition-colors text-left
          ${$page.url.pathname.startsWith("/dashboard/settings") ? "bg-emerald-100 text-emerald-700 font-medium" : "text-gray-700 hover:bg-emerald-50 hover:text-emerald-600"}`}
          on:click={closeMenu}
        >
          <svg class="mr-3 h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z"
            />
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
          </svg>
          Settings
        </a>

        <a
          href="/guide"
          class={`flex items-center w-full px-3 py-2 rounded-md transition-colors text-left
          ${$page.url.pathname.startsWith("/guide") ? "bg-emerald-100 text-emerald-700 font-medium" : "text-gray-700 hover:bg-emerald-50 hover:text-emerald-600"}`}
          on:click={closeMenu}
        >
          <svg class="mr-3 h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M12 6.042A8.967 8.967 0 0 0 6 3.75c-1.052 0-2.062.18-3 .512v14.25A8.987 8.987 0 0 1 6 18c2.305 0 4.408.867 6 2.292m0-14.25a8.966 8.966 0 0 1 6-2.292c1.052 0 2.062.18 3 .512v14.25A8.987 8.987 0 0 0 18 18a8.967 8.967 0 0 0-6 2.292m0-14.25v14.25"
            />
          </svg>

          Guide
        </a>

        <a
          href="/dashboard/premium"
          class={`flex items-center w-full px-3 py-2 rounded-md transition-colors text-left
          ${$page.url.pathname.startsWith("/dashboard/premium") ? "bg-emerald-100 text-emerald-700 font-medium" : "text-gray-700 hover:bg-emerald-50 hover:text-emerald-600"}`}
          on:click={closeMenu}
        >
          <svg class="mr-3 h-5 w-5" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" d="m3.75 13.5 10.5-11.25L12 10.5h8.25L9.75 21.75 12 13.5H3.75Z" />
          </svg>
          Premium
        </a>
      </nav>

      <!-- Logout at bottom -->
      <div class="px-4 pb-4">
        <!-- Divider -->
        <div class="border-t border-gray-200 mb-4"></div>

        <!-- Logout -->
        <button type="button" class="flex items-center w-full px-3 py-2 text-red-600 rounded-md hover:bg-red-50 hover:text-red-700 transition-colors text-left" on:click={logout}>
          <svg class="mr-3 h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" />
          </svg>
          Logout
        </button>
      </div>
    </div>
  </div>
</header>
