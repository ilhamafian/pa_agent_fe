<script lang="ts">
  // Phone number state
  let selectedCountryCode = "+60";
  let phoneNumber = "";
  let showCountryDropdown = false;

  const countryCodes = [{ code: "+60" }, { code: "+65" }, { code: "+1" }, { code: "+44" }, { code: "+91" }, { code: "+86" }, { code: "+81" }, { code: "+82" }, { code: "+66" }, { code: "+84" }];

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
</script>

<div class="p-4">
  <form method="POST" action="/dashboard" class="border-2 border-emerald-500 rounded-xl p-4 flex flex-col space-y-3">
    <h1 class="text-xl font-bold text-center">Reset PIN</h1>
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

      <button type="submit" class="w-full mt-4 px-4 py-2 bg-emerald-500 text-white rounded-md active:bg-emerald-600 transition-colors"> Send New PIN </button>

      <div class="">
        <a href="/authentication/login" class="text-sm text-gray-500">Back to login</a>
      </div>
    </div>
  </form>
</div>
