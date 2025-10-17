<script lang="ts">
  import { PUBLIC_BACKEND_URL } from "$env/static/public";
  import { goto } from "$app/navigation";
  import { page } from "$app/stores";

  // Get URL parameters
  let message = $page.url.searchParams.get("message");
  let urlPhoneNumber = $page.url.searchParams.get("phone_number");

  // Phone number state
  // Initialize phone number state
  const countryCodes = [{ code: "+60" }, { code: "+65" }, { code: "+1" }, { code: "+44" }, { code: "+91" }, { code: "+86" }, { code: "+81" }, { code: "+82" }, { code: "+66" }, { code: "+84" }];
  let showCountryDropdown = false;

  // Parse phone number from URL if present
  let selectedCountryCode = "+60";
  let phoneNumber = "";

  if (urlPhoneNumber) {
    // Try to match the phone number with available country codes
    const matchedCode = countryCodes
      .map((c) => c.code.replace("+", ""))
      .sort((a, b) => b.length - a.length) // Sort by length to match longer codes first
      .find((code) => urlPhoneNumber.startsWith(code));

    if (matchedCode) {
      selectedCountryCode = "+" + matchedCode;
      phoneNumber = urlPhoneNumber.slice(matchedCode.length);
    } else {
      // If no country code matches, use default and full number
      phoneNumber = urlPhoneNumber;
    }
  }

  // PIN verification state
  let pinDigits = ["", "", "", "", "", ""];
  let pinInputs: HTMLInputElement[] = [];
  let errors = {
    phone: "",
    pin: "",
  };
  let touched = {
    phone: false,
    pin: false,
  };

  // Phone number handling
  function selectCountryCode(code: string) {
    selectedCountryCode = code;
    showCountryDropdown = false;
    if (errors.phone) {
      errors.phone = "";
    }
  }

  function handlePhoneInput(event: Event) {
    const target = event.target as HTMLInputElement;
    let value = target.value;

    // Only allow numeric characters
    const numericValue = value.replace(/[^0-9]/g, "");
    target.value = numericValue;
    phoneNumber = numericValue;

    // Clear validation error when user starts typing
    if (phoneNumber && errors.phone) {
      errors.phone = "";
    }
  }

  // PIN input handling
  function handlePinInput(index: number, event: Event) {
    const target = event.target as HTMLInputElement;
    let value = target.value;

    // Only allow numeric characters
    const numericValue = value.replace(/[^0-9]/g, "");

    // Only allow single digits
    if (numericValue.length > 1) {
      target.value = numericValue.slice(-1);
      value = target.value;
    } else {
      target.value = numericValue;
      value = numericValue;
    }

    // Update pin digit
    pinDigits[index] = value;

    // Clear validation error when user starts typing
    if (value && errors.pin) {
      errors.pin = "";
    }

    // Auto-focus next input
    if (value && index < 5) {
      const nextInput = pinInputs[index + 1];
      if (nextInput) {
        nextInput.focus();
      }
    }
  }

  function handlePinKeydown(index: number, event: KeyboardEvent) {
    // Handle backspace
    if (event.key === "Backspace" && !pinDigits[index] && index > 0) {
      const prevInput = pinInputs[index - 1];
      if (prevInput) {
        prevInput.focus();
      }
    }
  }

  function handlePinPaste(event: ClipboardEvent) {
    event.preventDefault();
    const pastedData = event.clipboardData?.getData("text") || "";
    const digits = pastedData.replace(/\D/g, "").split("").slice(0, 6);

    for (let i = 0; i < 6; i++) {
      pinDigits[i] = digits[i] || "";
      if (pinInputs[i]) {
        pinInputs[i].value = pinDigits[i];
      }
    }

    // Focus the next empty input or the last one
    const nextEmptyIndex = pinDigits.findIndex((digit) => digit === "");
    const focusIndex = nextEmptyIndex === -1 ? 5 : nextEmptyIndex;
    if (pinInputs[focusIndex]) {
      pinInputs[focusIndex].focus();
    }

    // Clear validation error
    if (errors.pin) {
      errors.pin = "";
    }
  }

  async function login() {
    try {
      const response = await fetch(`${PUBLIC_BACKEND_URL}/login`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          phone_number: selectedCountryCode.replace("+", "") + phoneNumber,
          PIN: pinDigits.join(""),
        }),
      });

      const result = await response.json();

      if (response.ok) {
        localStorage.setItem("token", result.token);
        localStorage.setItem("user_id", result.user_id);
        localStorage.setItem("nickname", result.nickname);
        localStorage.setItem("phone_number", result.phone_number);

        goto("/dashboard");
      } else {
        alert(result.detail || result.message || "Login failed");
      }
    } catch (error) {
      console.error("Login error:", error);
      alert("Something went wrong during login");
    }
  }
</script>

<div class="p-4 max-w-lg mx-auto">
  {#if message}
    <div class="mb-4 p-4 bg-amber-50 border border-amber-200 rounded-lg text-amber-700">
      {message}
    </div>
  {/if}
  <form on:submit|preventDefault={login} class="border-2 border-emerald-500 rounded-xl p-4 flex flex-col space-y-3">
    <h1 class="text-xl font-bold text-center">Login</h1>
    <!-- Phone Number Input -->
    <div class="space-y-2">
      <label for="phone" class="block text-sm font-medium text-gray-700">Phone Number</label>
      <div class="flex space-x-2">
        <!-- Country Code Dropdown (1/5 ratio) -->
        <div class="relative w-1/5">
          <button
            type="button"
            class="w-full h-12 px-2 text-left border-2 rounded-lg focus:outline-none focus:ring-2 transition-colors flex items-center justify-between {errors.phone && touched.phone ? 'border-red-500 focus:ring-red-500' : 'border-gray-300 focus:ring-emerald-500 focus:border-emerald-500'}"
            on:click={() => (showCountryDropdown = !showCountryDropdown)}
          >
            <span class="font-medium">{selectedCountryCode}</span>
            <svg class="w-4 h-4 transform transition-transform {showCountryDropdown ? 'rotate-180' : ''}" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
            </svg>
          </button>

          {#if showCountryDropdown}
            <div class="absolute z-10 w-full mt-1 bg-white border border-gray-300 rounded-lg shadow-lg max-h-60 overflow-y-auto">
              {#each countryCodes as country}
                <button type="button" class="w-full px-3 py-2 text-left hover:bg-gray-50 focus:bg-gray-50 focus:outline-none flex items-center space-x-2" on:click={() => selectCountryCode(country.code)}>
                  <span class="font-medium">{country.code}</span>
                </button>
              {/each}
            </div>
          {/if}
        </div>

        <!-- Phone Number Input (4/5 ratio) -->
        <div class="w-4/5">
          <input
            type="tel"
            id="phone"
            bind:value={phoneNumber}
            placeholder="Enter your phone number"
            class="w-full h-12 px-3 border-2 rounded-lg focus:outline-none focus:ring-2 transition-colors {errors.phone && touched.phone ? 'border-red-500 focus:ring-red-500' : 'border-gray-300 focus:ring-emerald-500 focus:border-emerald-500'}"
            on:input={(e) => handlePhoneInput(e)}
            on:blur={() => (touched.phone = true)}
          />
        </div>
      </div>

      {#if errors.phone && touched.phone}
        <p class="text-sm text-red-600">{errors.phone}</p>
      {/if}
    </div>

    <!-- PIN Input -->
    <div class="space-y-2">
      <label for="pin" class="block text-sm font-medium text-gray-700">PIN</label>
      <div class="flex justify-between items-center space-x-2">
        {#each pinDigits as digit, index}
          <input
            bind:this={pinInputs[index]}
            id="pin"
            type="password"
            inputmode="numeric"
            pattern="[0-9]"
            maxlength="1"
            value={digit}
            class="w-10 h-10 text-center text-xl font-semibold border-2 rounded-lg focus:outline-none focus:ring-2 transition-colors {errors.pin && touched.pin ? 'border-red-500 focus:ring-red-500' : 'border-gray-300 focus:ring-emerald-500 focus:border-emerald-500'}"
            on:input={(e) => handlePinInput(index, e)}
            on:keydown={(e) => handlePinKeydown(index, e)}
            on:paste={handlePinPaste}
            on:blur={() => (touched.pin = true)}
          />
          {#if index < 5}
            <div class="w-2 h-0.5 bg-gray-300"></div>
          {/if}
        {/each}
      </div>

      {#if errors.pin && touched.pin}
        <p class="text-sm text-red-600">{errors.pin}</p>
      {/if}
    </div>

    <button type="submit" class="w-full mt-4 px-4 py-2 bg-emerald-500 text-white rounded-md active:bg-emerald-600 transition-colors"> Login </button>

    <div class="">
      <a href="/auth/forgot-pin" class="text-sm text-gray-500">Forgot pin?</a>
    </div>
  </form>
</div>

<!-- Click outside to close dropdown -->
<svelte:window
  on:click={(e) => {
    const target = e.target as HTMLElement;
    if (!target.closest(".relative")) {
      showCountryDropdown = false;
    }
  }}
/>
