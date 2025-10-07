<script lang="ts">
  import { PUBLIC_BACKEND_URL } from "$env/static/public";
  import { goto } from "$app/navigation";
  import { page } from "$app/stores";
  import { onMount } from "svelte";
  import { browser } from "$app/environment";

  let urlPhoneNumber = "";

  onMount(async () => {
    if (!browser) return;

    const query = $page.url.searchParams;
    const phone_number = query.get("phone_number");
    const token = localStorage.getItem("token");

    // 🚫 No phone or token → send to login
    if (!phone_number) {
      console.log("No phone number");
      return goto("/auth/login");
    }

    // ✅ Already logged in → go to dashboard
    if (token) {
      console.log("Already logged in");
      return goto("/dashboard");
    }

    // Check if phone number is already onboarded
    const res = await fetch(`${PUBLIC_BACKEND_URL}/check_phone_number_exist`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ phone_number }),
    });

    const data = await res.json();

    // 🚫 Already onboarded → send to login
    if (data.exists) {
      console.log("Already onboarded");
      return goto("/auth/login");
    }

    // Extract phone number from URL query parameter
    urlPhoneNumber = phone_number;
    console.log("In urlPhoneNumber:", urlPhoneNumber);

    // ✅ Else, continue to onboarding page
  });

  async function onboarding() {
    if (currentStep < totalSteps) {
      if (validateStep(currentStep)) {
        currentStep++;
      }
    } else {
      if (validateStep(currentStep)) {
        // Structure the form data as requested
        const formOutput = {
          PIN: pinDigits.join(""),
          phone_number: urlPhoneNumber,
          nickname: formData.nickname,
          email: formData.email,
          language: formData.language,
          metadata: {
            q1: selectedTools,
            q2: formData.question_2,
            q3: formData.question_3,
            q4: formData.question_4,
          },
        };

        // Console log the structured data
        console.log("Form submission data:", formOutput);
        const response = await fetch(`${PUBLIC_BACKEND_URL}/user_onboarding`, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(formOutput),
        });

        const result = await response.json();
        console.log("Server replied:", result);

        // Navigate to dashboard
        goto("/dashboard");
      }
    }
  }

  let currentStep = 0; // 0 = welcome, 1-4 = steps
  const totalSteps = 4;

  // Multi-select state for productivity tools
  let selectedTools: string[] = [];
  let showDropdown = false;
  let othersInputValue = "";

  // Form data
  let formData = {
    nickname: "",
    email: "",
    language: "",
    question_1: [] as string[],
    question_2: "",
    question_3: "",
    question_4: "",
  };

  // PIN verification state
  let pinDigits = ["", "", "", "", "", ""];
  let pinInputs: HTMLInputElement[] = [];

  // Validation state
  let errors = {
    nickname: "",
    email: "",
    language: "",
    question_1: "",
    question_2: "",
    question_3: "",
    pin: "",
  };

  let touched = {
    nickname: false,
    email: false,
    language: false,
    question_1: false,
    question_2: false,
    question_3: false,
    pin: false,
  };

  const question_1: string[] = ["Notion", "Google Tasks", "Todoist", "Trello", "Asana", "Monday.com", "ClickUp", "Slack", "None"];

  const steps = [
    {
      title: "Personal Information",
    },
    {
      title: "Getting to Know You",
    },
    {
      title: "Security Verification",
    },
    {
      title: "Setup Complete",
    },
  ];

  function startOnboarding() {
    currentStep = 1;
  }

  function validateStep(step: number): boolean {
    let isValid = true;

    if (step === 1) {
      // Validate nickname
      if (!formData.nickname.trim()) {
        errors.nickname = "Please enter your preferred name";
        isValid = false;
      } else {
        errors.nickname = "";
      }

      // Validate email
      if (!formData.email.trim()) {
        errors.email = "Please enter your email";
        isValid = false;
      } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.email)) {
        errors.email = "Please enter a valid email address";
        isValid = false;
      } else {
        errors.email = "";
      }

      // Validate language
      if (!formData.language) {
        errors.language = "Please select a language";
        isValid = false;
      } else {
        errors.language = "";
      }

      // Mark fields as touched
      touched.nickname = true;
      touched.email = true;
      touched.language = true;
    }

    if (step === 2) {
      // Validate productivity tools (at least one selection)
      if (selectedTools.length === 0) {
        errors.question_1 = "Please select at least one option";
        isValid = false;
      } else {
        errors.question_1 = "";
      }

      // Validate question_2
      if (!formData.question_2) {
        errors.question_2 = "Please select your profession";
        isValid = false;
      } else {
        errors.question_2 = "";
      }

      // Validate question_3 textarea
      if (!formData.question_3.trim()) {
        errors.question_3 = "Please share what you struggle to keep track of";
        isValid = false;
      } else {
        errors.question_3 = "";
      }

      // Mark fields as touched
      touched.question_1 = true;
      touched.question_2 = true;
      touched.question_3 = true;
    }

    if (step === 3) {
      // Validate PIN (all 6 digits must be filled)
      if (pinDigits.some((digit) => digit === "")) {
        errors.pin = "Please enter all 6 digits";
        isValid = false;
      } else {
        errors.pin = "";
      }

      // Mark PIN as touched
      touched.pin = true;
    }

    return isValid;
  }

  function prevStep() {
    if (currentStep > 1) {
      currentStep--;
    } else {
      currentStep = 0; // Back to welcome
    }
  }

  function toggleTool(tool: string) {
    if (selectedTools.includes(tool)) {
      selectedTools = selectedTools.filter((t) => t !== tool);
    } else {
      selectedTools = [...selectedTools, tool];
    }
    formData.question_1 = selectedTools;

    // Clear validation error when user makes selection
    if (selectedTools.length > 0) {
      errors.question_1 = "";
    }
  }

  function removeTool(tool: string) {
    selectedTools = selectedTools.filter((t) => t !== tool);
    formData.question_1 = selectedTools;

    // Show validation error if no tools selected
    if (selectedTools.length === 0 && touched.question_1) {
      errors.question_1 = "Please select at least one option";
    }
  }

  function addOtherTool() {
    if (othersInputValue.trim() && !selectedTools.includes(othersInputValue.trim())) {
      selectedTools = [...selectedTools, othersInputValue.trim()];
      formData.question_1 = selectedTools;
      othersInputValue = "";

      // Clear validation error when user adds tool
      errors.question_1 = "";
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

  function toggleDropdown() {
    showDropdown = !showDropdown;
  }
</script>

<div class="p-4 max-w-lg mx-auto">
  {#if currentStep === 0}
    <!-- Welcome Screen -->
    <h1 class="text-2xl font-bold text-center mb-6">Welcome to Onboarding</h1>

    <div class="flex flex-col gap-4">
      <div class="border-2 border-emerald-500 rounded-xl p-4 text-center">
        <h2 class="text-xl font-semibold mb-4">Get Started</h2>
        <p class="text-gray-700 mb-4">Welcome! Let's help you get set up and ready to go.</p>

        <div class="flex flex-col gap-4">
          <button on:click={startOnboarding} class="px-6 py-3 bg-emerald-500 hover:bg-emerald-600 text-white rounded-md transition-colors"> Start Onboarding </button>
        </div>
      </div>
    </div>
  {:else}
    <!-- Steps Screen with Form -->
    <form on:submit|preventDefault={onboarding}>
      <!-- Steps Screen -->
      <div class="mb-6">
        <h1 class="text-2xl font-bold text-center mb-3">Onboarding Steps</h1>

        <!-- Progress Bar -->
        <div class="w-full bg-gray-200 rounded-full h-2 mb-4">
          <div class="bg-emerald-500 h-2 rounded-full transition-all duration-300" style="width: {(currentStep / totalSteps) * 100}%"></div>
        </div>
        <div class="text-center text-sm text-gray-600">
          Step {currentStep} of {totalSteps}
        </div>
      </div>

      <div class="border-2 border-emerald-500 rounded-xl p-4">
        <h2 class="text-xl font-semibold mb-6 text-center">
          {steps[currentStep - 1].title}
        </h2>

        <!-- Step Content -->
        {#if currentStep === 1}
          <div class="space-y-4">
            <div>
              <label for="nickname" class="block text-sm font-medium text-gray-700 mb-2">What should I call you?</label>
              <input
                id="nickname"
                type="text"
                bind:value={formData.nickname}
                class="w-full px-3 py-2 border rounded-md focus:outline-none focus:ring-2 transition-colors {errors.nickname && touched.nickname ? 'border-red-500 focus:ring-red-500' : 'border-gray-300 focus:ring-emerald-500'}"
                placeholder="Preferred nickname"
                on:blur={() => (touched.nickname = true)}
                on:input={() => {
                  if (formData.nickname.trim()) {
                    errors.nickname = "";
                  }
                }}
              />
              {#if errors.nickname && touched.nickname}
                <p class="mt-1 text-sm text-red-600">{errors.nickname}</p>
              {/if}
            </div>
            <div>
              <label for="email" class="block text-sm font-medium text-gray-700 mb-2">Email</label>
              <input
                id="email"
                type="email"
                bind:value={formData.email}
                class="w-full px-3 py-2 border rounded-md focus:outline-none focus:ring-2 transition-colors {errors.email && touched.email ? 'border-red-500 focus:ring-red-500' : 'border-gray-300 focus:ring-emerald-500'}"
                placeholder="Enter your email"
                on:blur={() => (touched.email = true)}
                on:input={() => {
                  if (formData.email.trim() && /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.email)) {
                    errors.email = "";
                  }
                }}
              />
              {#if errors.email && touched.email}
                <p class="mt-1 text-sm text-red-600">{errors.email}</p>
              {/if}
            </div>
            <div>
              <label for="language" class="block text-sm font-medium text-gray-700 mb-2">Language</label>
              <select
                id="language"
                bind:value={formData.language}
                class="w-full px-3 py-2 border rounded-md focus:outline-none focus:ring-2 transition-colors {errors.language && touched.language ? 'border-red-500 focus:ring-red-500' : 'border-gray-300 focus:ring-emerald-500'}"
                on:change={() => {
                  touched.language = true;
                  if (formData.language) {
                    errors.language = "";
                  }
                }}
              >
                <option value="">Select your language</option>
                <option value="Malay">Malay</option>
                <option value="English">English</option>
                <option value="Chinese">Chinese</option>
                <option value="Tamil">Tamil</option>
                <option value="Hindi">Hindi</option>
                <option value="Korean">Korean</option>
                <option value="Japanese">Japanese</option>
                <option value="Arabic">Arabic</option>
                <option value="Russian">Russian</option>
              </select>
              {#if errors.language && touched.language}
                <p class="mt-1 text-sm text-red-600">{errors.language}</p>
              {/if}
            </div>
          </div>
        {:else if currentStep === 2}
          <div class="space-y-4">
            <div>
              <label for="question-1" class="block text-sm font-medium text-gray-700 mb-2">Do you use other productivity tools?</label>
              <!-- Selected Tools Display -->
              {#if selectedTools.length > 0}
                <div class="flex flex-wrap gap-2 mb-3">
                  {#each selectedTools as tool}
                    <button class="inline-flex items-center px-3 py-1 rounded-full text-sm bg-emerald-200 text-emerald-800 transition-colors" on:click={() => removeTool(tool)}>
                      {tool}
                      <svg class="ml-1 h-4 w-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                      </svg>
                    </button>
                  {/each}
                </div>
              {/if}

              <!-- Custom Multi-Select Dropdown -->
              <div class="relative">
                <button
                  id="question-1"
                  type="button"
                  on:click={toggleDropdown}
                  class="w-full px-3 py-2 border rounded-md focus:outline-none focus:ring-2 bg-transparent text-left flex justify-between items-center transition-colors {errors.question_1 && touched.question_1 ? 'border-red-500 focus:ring-red-500' : 'border-gray-300 focus:ring-emerald-500'}"
                >
                  <span class="text-gray-500">
                    {selectedTools.length > 0 ? `${selectedTools.length} selected` : "Select tools"}
                  </span>
                  <svg class="h-4 w-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
                  </svg>
                </button>

                {#if showDropdown}
                  <div class="absolute z-10 w-full mt-1 bg-white border border-gray-300 rounded-md shadow-lg max-h-60 overflow-auto">
                    {#each question_1 as tool}
                      <button type="button" class="w-full px-3 py-2 text-left hover:bg-gray-100 flex items-center justify-between" on:click={() => toggleTool(tool)}>
                        <span>{tool}</span>
                        {#if selectedTools.includes(tool)}
                          <svg class="h-4 w-4 text-emerald-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                          </svg>
                        {/if}
                      </button>
                    {/each}

                    <!-- Add Other Input in Dropdown -->
                    <div class="border-t border-gray-200 p-2">
                      <div class="flex gap-2">
                        <input
                          type="text"
                          bind:value={othersInputValue}
                          placeholder="Add other tools"
                          class="flex-1 px-2 py-2 text-sm border border-gray-300 rounded focus:outline-none focus:ring-1 focus:ring-emerald-500"
                          on:keypress={(e) => {
                            if (e.key === "Enter") {
                              e.preventDefault();
                              addOtherTool();
                            }
                          }}
                          on:click={(e) => e.stopPropagation()}
                        />
                        <button
                          type="button"
                          on:click={(e) => {
                            e.stopPropagation();
                            addOtherTool();
                          }}
                          disabled={!othersInputValue.trim()}
                          class="px-3 py-1 text-sm bg-emerald-500 text-white rounded hover:bg-emerald-600 transition-colors disabled:opacity-50 disabled:cursor-not-allowed"
                        >
                          Add
                        </button>
                      </div>
                    </div>
                  </div>
                {/if}
              </div>
              {#if errors.question_1 && touched.question_1}
                <p class="mt-1 text-sm text-red-600">{errors.question_1}</p>
              {/if}
            </div>
            <div>
              <label for="question-2" class="block text-sm font-medium text-gray-700 mb-2">Are you a student or professional?</label>
              <select
                id="question-2"
                bind:value={formData.question_2}
                class="w-full px-3 py-2 border rounded-md focus:outline-none focus:ring-2 transition-colors {errors.question_2 && touched.question_2 ? 'border-red-500 focus:ring-red-500' : 'border-gray-300 focus:ring-emerald-500'}"
                on:change={() => {
                  touched.question_2 = true;
                  if (formData.question_2) {
                    errors.question_2 = "";
                  }
                }}
              >
                <option value="">Select your profession</option>
                <option value="Working Professional">Working Professional</option>
                <option value="Student">Student</option>
                <option value="Freelancer">Freelancer</option>
                <option value="Entrepreneur">Entrepreneur</option>
                <option value="Other">Other</option>
              </select>
              {#if errors.question_2 && touched.question_2}
                <p class="mt-1 text-sm text-red-600">{errors.question_2}</p>
              {/if}
            </div>
            <div>
              <label for="question-3" class="block text-sm font-medium text-gray-700 mb-2">What’s one thing you struggle to keep track of?</label>
              <textarea
                id="question-3"
                bind:value={formData.question_3}
                class="w-full px-3 py-2 border rounded-md focus:outline-none focus:ring-2 min-h-[80px] transition-colors {errors.question_3 && touched.question_3 ? 'border-red-500 focus:ring-red-500' : 'border-gray-300 focus:ring-emerald-500'}"
                placeholder="Events, tasks, appointments, etc."
                on:blur={() => (touched.question_3 = true)}
                on:input={() => {
                  if (formData.question_3.trim()) {
                    errors.question_3 = "";
                  }
                }}
              ></textarea>
              {#if errors.question_3 && touched.question_3}
                <p class="mt-1 text-sm text-red-600">{errors.question_3}</p>
              {/if}
            </div>
            <div>
              <label for="question-4" class="block text-sm font-medium text-gray-700 mb-2">Where did you find us? (Optional)</label>
              <select id="question-4" bind:value={formData.question_4} class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-emerald-500">
                <option value="">Select an option</option>
                <option value="Social Media">Social Media</option>
                <option value="Friend">Friend</option>
                <option value="Blog/Article">Blog/Article</option>
                <option value="Online Forum">Online Forum</option>
              </select>
            </div>
          </div>
        {:else if currentStep === 3}
          <!-- PIN Verification Step -->
          <div class="text-center">
            <div class="mb-6">
              <div class="w-12 h-12 bg-emerald-100 rounded-full flex items-center justify-center mx-auto mb-4">
                <svg class="w-6 h-6 text-emerald-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" />
                </svg>
              </div>
              <h3 class="text-lg font-semibold text-gray-900 mb-2">Secure Your Account</h3>
              <p class="text-sm text-gray-600 mb-6">Create a 6-digit PIN to secure your account</p>
            </div>

            <div class="">
              <div class="flex justify-center items-center space-x-2 mb-4">
                {#each pinDigits as digit, index}
                  <input
                    bind:this={pinInputs[index]}
                    type="text"
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

            <div class="text-center">
              <p class="text-xs text-gray-500">This PIN will be used to secure your account and verify important actions</p>
            </div>
          </div>
        {:else}
          <div class="text-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-16 mx-auto text-emerald-500 mb-4">
              <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
            </svg>
            <p class="text-lg text-gray-700">Great! Your account is now ready to use.</p>
          </div>
        {/if}

        <!-- Navigation Buttons -->
        <div class="flex justify-between mt-6">
          <button type="button" on:click={prevStep} class="px-6 py-2 border-2 border-gray-300 text-gray-700 rounded-md hover:border-gray-400 transition-colors">
            {currentStep === 1 ? "Back to Welcome" : "Previous"}
          </button>

          <button type="submit" class="px-6 py-2 bg-emerald-500 hover:bg-emerald-600 text-white rounded-md transition-colors">
            {currentStep === totalSteps ? "Complete" : "Next"}
          </button>
        </div>
      </div>
    </form>
  {/if}
</div>
