<script lang="ts">
  import { goto } from "$app/navigation";
  import * as Dialog from "$lib/components/ui/dialog";
  import { PUBLIC_BACKEND_URL } from "$env/static/public";

  const features = [
    {
      title: "Smart Calendar Management",
      description: "Automatically schedule events, detect booking templates, and manage your calendar with natural language commands.",
      icon: "📅",
      examples: ["Schedule a meeting tomorrow at 2pm", "Book a makeup session for Yana on Friday"],
    },
    {
      title: "Intelligent Task Management",
      description: "Create, organize, and track your todo tasks with automatic priority detection and smart categorization.",
      icon: "✅",
      examples: ["Add urgent task: Submit project proposal", "Create task: Clean the house this weekend"],
    },
    {
      title: "Flexible Reminders",
      description: "Set reminders for calendar events or custom tasks with natural language time expressions.",
      icon: "⏰",
      examples: ["Remind me 30 minutes before my meeting", "Remind me in 3 hours to call mom"],
    },
    {
      title: "Template Detection",
      description: "Automatically recognize and process booking templates from freelancers and service providers.",
      icon: "📋",
      examples: ["Makeup artist booking forms", "Service provider templates", "Appointment confirmations"],
    },
  ];

  let showWaitlistDialog = false;
  let phoneNumber: string = "";
  let waitlistSubmitting = false;
  let waitlistError: string = "";
  let waitlistSuccess = false;

  function handleJoinWaitlist() {
    waitlistError = "";
    waitlistSuccess = false;
    phoneNumber = "";
    showWaitlistDialog = true;
  }

  async function handleWaitlistSubmit() {
    waitlistError = "";
    const trimmed = phoneNumber.trim();
    console.log("trimmed", trimmed);

    if (trimmed.length < 7) {
      waitlistError = "Please enter a valid phone number.";
      return;
    }
    try {
      waitlistSubmitting = true;
      // TODO: send to your backend/waitlist service
      const response = await fetch(`${PUBLIC_BACKEND_URL}/waitlist`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ phone_number: trimmed }),
      });
      const data = await response.json();
      console.log("data", data);
      await new Promise((r) => setTimeout(r, 500));
      waitlistSuccess = true;
    } catch (e) {
      waitlistError = "Something went wrong. Please try again.";
    } finally {
      waitlistSubmitting = false;
    }
  }

  function handleLearnMore() {
    document.getElementById("features")?.scrollIntoView({ behavior: "smooth" });
  }
</script>

<svelte:head>
  <title>Personal Assistant - Smart Calendar & Task Management</title>
  <meta name="description" content="Your intelligent virtual assistant for calendar management, task organization, and smart reminders with Google Calendar integration." />
</svelte:head>

<div class="min-h-screen">
  <!-- Hero Section -->
  <section class="relative overflow-hidden">
    <div class="absolute inset-0 bg-gradient-to-r from-primary/5 to-transparent"></div>
    <div class="relative px-4 pt-12 pb-8">
      <div class="text-center space-y-8">
        <div class="space-y-4">
          <h1 class="text-2xl sm:text-3xl font-bold text-foreground leading-tight">
            Your Smart
            <span class="text-primary block">Personal Assistant</span>
            Directly through WhatsApp
          </h1>
          <p class="text-base text-muted-foreground leading-relaxed">Effortlessly manage your calendar events and tasks with natural language. From booking templates to smart reminders, your AI assistant handles it all directly through WhatsApp.</p>
        </div>

        <div class="flex flex-col gap-3">
          <button on:click={handleJoinWaitlist} class="bg-primary text-primary-foreground px-6 py-3 rounded-lg font-semibold hover:bg-primary/90 transition-colors shadow-lg"> Join Waitlist</button>
          <button on:click={handleLearnMore} class="border border-border text-foreground px-6 py-3 rounded-lg font-semibold hover:bg-accent transition-colors"> Learn More </button>
        </div>
      </div>
    </div>
  </section>

  <!-- Features Section -->
  <section id="features" class="py-12 bg-card/50">
    <div class="px-4">
      <div class="text-center space-y-3 mb-8">
        <h2 class="text-xl font-bold text-foreground">Powerful Features</h2>
        <p class="text-sm text-muted-foreground">Everything you need to stay organized and productive</p>
      </div>

      <div class="space-y-6">
        {#each features as feature}
          <div class="bg-card border border-border rounded-lg p-4 hover:shadow-lg transition-shadow">
            <div class="flex items-center gap-3 mb-3">
              <div class="text-2xl">{feature.icon}</div>
              <h3 class="text-lg font-semibold text-card-foreground">
                {feature.title}
              </h3>
            </div>
            <p class="text-sm text-muted-foreground mb-3 leading-relaxed">
              {feature.description}
            </p>
            <div class="space-y-1">
              <p class="text-xs font-medium text-foreground">Examples:</p>
              {#each feature.examples as example}
                <p class="text-xs text-muted-foreground italic">"{example}"</p>
              {/each}
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section>

  <!-- How It Works Section -->
  <section class="py-12">
    <div class="px-4">
      <div class="text-center space-y-3 mb-8">
        <h2 class="text-xl font-bold text-foreground">How It Works</h2>
        <p class="text-sm text-muted-foreground">Simple, intuitive, and intelligent</p>
      </div>

      <div class="space-y-6">
        <div class="flex items-start gap-4 bg-card border border-border rounded-lg p-4">
          <div class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold flex-shrink-0">1</div>
          <div>
            <h3 class="text-base font-semibold text-foreground mb-2">Natural Communication</h3>
            <p class="text-sm text-muted-foreground leading-relaxed">Simply tell the assistant what you need using everyday language. No complex commands or syntax required.</p>
          </div>
        </div>

        <div class="flex items-start gap-4 bg-card border border-border rounded-lg p-4">
          <div class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold flex-shrink-0">2</div>
          <div>
            <h3 class="text-base font-semibold text-foreground mb-2">Smart Interpretation</h3>
            <p class="text-sm text-muted-foreground leading-relaxed">The assistant understands context, dates, times, and priorities, automatically filling in missing details.</p>
          </div>
        </div>

        <div class="flex items-start gap-4 bg-card border border-border rounded-lg p-4">
          <div class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold flex-shrink-0">3</div>
          <div>
            <h3 class="text-base font-semibold text-foreground mb-2">Seamless Integration</h3>
            <p class="text-sm text-muted-foreground leading-relaxed">Your events sync with Google Calendar and tasks are organized by priority and status automatically.</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Template Detection Showcase -->
  <section class="py-12 bg-card/50">
    <div class="px-4">
      <div class="space-y-6">
        <div class="space-y-4">
          <h2 class="text-xl font-bold text-foreground">Smart Template Detection</h2>
          <p class="text-sm text-muted-foreground leading-relaxed">The assistant automatically recognizes booking templates from freelancers and service providers, extracting all relevant information to create perfect calendar events.</p>
          <div class="space-y-3">
            <div class="flex items-start gap-2">
              <div class="w-4 h-4 bg-primary rounded-full flex items-center justify-center mt-0.5">
                <span class="text-primary-foreground text-xs">✓</span>
              </div>
              <div>
                <h4 class="text-sm font-semibold text-foreground">Automatic Field Detection</h4>
                <p class="text-xs text-muted-foreground">Recognizes names, dates, times, locations, and event types</p>
              </div>
            </div>
            <div class="flex items-start gap-2">
              <div class="w-4 h-4 bg-primary rounded-full flex items-center justify-center mt-0.5">
                <span class="text-primary-foreground text-xs">✓</span>
              </div>
              <div>
                <h4 class="text-sm font-semibold text-foreground">Smart Time Calculation</h4>
                <p class="text-xs text-muted-foreground">Calculates prep time and event duration automatically</p>
              </div>
            </div>
            <div class="flex items-start gap-2">
              <div class="w-4 h-4 bg-primary rounded-full flex items-center justify-center mt-0.5">
                <span class="text-primary-foreground text-xs">✓</span>
              </div>
              <div>
                <h4 class="text-sm font-semibold text-foreground">Comprehensive Details</h4>
                <p class="text-xs text-muted-foreground">Preserves all template information in event descriptions</p>
              </div>
            </div>
          </div>
        </div>

        <div class="bg-card border border-border rounded-lg p-4">
          <h3 class="text-base font-semibold text-card-foreground mb-3">📝 Makeup Artist Template Example</h3>
          <pre class="text-xs text-muted-foreground whitespace-pre-wrap bg-muted p-3 rounded border">Name: Yana Nazri
Event: Nikah&touchup sanding
Location of Makeup: Dewan Seri Endon, KLCC
Date: 15/12/2024
Time (need to be ready by): 8:00 AM
Time of event: 10:00 AM</pre>
          <div class="mt-3 p-3 bg-primary/10 rounded border border-primary/20">
            <p class="text-xs font-medium text-foreground mb-1">⚡ Automatically creates:</p>
            <p class="text-xs text-muted-foreground">
              <strong>Event:</strong> "Nikah&touchup sanding for Yana Nazri"<br />
              <strong>Date:</strong> December 15, 2024<br />
              <strong>Time:</strong> 8:00 AM - 10:00 AM<br />
              <strong>Location:</strong> Dewan Seri Endon, KLCC
            </p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Task Management Showcase -->
  <section class="py-12">
    <div class="px-4">
      <div class="text-center space-y-3 mb-8">
        <h2 class="text-xl font-bold text-foreground">Intelligent Task Management</h2>
        <p class="text-sm text-muted-foreground">Automatic priority detection and smart organization</p>
      </div>

      <div class="space-y-4">
        <div class="bg-card border border-border rounded-lg p-4">
          <div class="flex items-center gap-2 mb-3">
            <span class="text-lg">🔴</span>
            <h3 class="text-base font-semibold text-card-foreground">High Priority</h3>
          </div>
          <p class="text-sm text-muted-foreground mb-3">Urgent tasks with deadlines, meetings, or important actions</p>
          <div class="space-y-2">
            <div class="p-2 bg-destructive/10 rounded border border-destructive/20">
              <p class="text-xs font-medium">"Submit project proposal ASAP"</p>
            </div>
            <div class="p-2 bg-destructive/10 rounded border border-destructive/20">
              <p class="text-xs font-medium">"Call doctor for appointment"</p>
            </div>
          </div>
        </div>

        <div class="bg-card border border-border rounded-lg p-4">
          <div class="flex items-center gap-2 mb-3">
            <span class="text-lg">🟡</span>
            <h3 class="text-base font-semibold text-card-foreground">Medium Priority</h3>
          </div>
          <p class="text-sm text-muted-foreground mb-3">Regular tasks and general activities</p>
          <div class="space-y-2">
            <div class="p-2 bg-yellow-500/10 rounded border border-yellow-500/20">
              <p class="text-xs font-medium">"Buy groceries for the week"</p>
            </div>
            <div class="p-2 bg-yellow-500/10 rounded border border-yellow-500/20">
              <p class="text-xs font-medium">"Organize home office"</p>
            </div>
          </div>
        </div>

        <div class="bg-card border border-border rounded-lg p-4">
          <div class="flex items-center gap-2 mb-3">
            <span class="text-lg">🟢</span>
            <h3 class="text-base font-semibold text-card-foreground">Low Priority</h3>
          </div>
          <p class="text-sm text-muted-foreground mb-3">Nice-to-have tasks and optional activities</p>
          <div class="space-y-2">
            <div class="p-2 bg-green-500/10 rounded border border-green-500/20">
              <p class="text-xs font-medium">"Watch new movie series"</p>
            </div>
            <div class="p-2 bg-green-500/10 rounded border border-green-500/20">
              <p class="text-xs font-medium">"Learn new recipe"</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- CTA Section -->
  <section class="py-8 bg-primary text-primary-foreground">
    <div class="text-center px-4 space-y-4">
      <h2 class="text-xl font-bold">Ready to Get Organized?</h2>
      <p class="text-sm text-primary-foreground/90">Start using your intelligent personal assistant today and experience the future of productivity.</p>
      <div class="flex flex-col gap-3">
        <button on:click={handleJoinWaitlist} class="bg-primary-foreground text-primary px-6 py-3 rounded-lg font-semibold hover:bg-primary-foreground/90 transition-colors shadow-lg"> Join Waitlist Now </button>
        <!-- <a href="/dashboard" class="border border-primary-foreground/20 text-primary-foreground px-6 py-3 rounded-lg font-semibold hover:bg-primary-foreground/10 transition-colors"> View Dashboard </a> -->
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="bg-card border-t border-border py-8">
    <div class="px-4">
      <div class="space-y-6">
        <div class="text-center space-y-3">
          <h3 class="text-base font-semibold text-card-foreground">Personal Assistant</h3>
          <p class="text-sm text-muted-foreground">Your intelligent companion for calendar management and task organization.</p>
        </div>

        <div class="grid grid-cols-2 gap-4 text-center">
          <div class="space-y-2">
            <!-- <h4 class="text-sm font-semibold text-card-foreground">Quick Links</h4> -->
            <ul class="space-y-1 text-xs text-muted-foreground">
              <!-- <li><a href="/dashboard" class="hover:text-foreground transition-colors">Dashboard</a></li> -->
              <!-- <li><a href="/onboarding" class="hover:text-foreground transition-colors">Get Started</a></li> -->
              <!-- <li><a href="/dashboard/settings" class="hover:text-foreground transition-colors">Settings</a></li> -->
            </ul>
          </div>

          <div class="space-y-2">
            <h4 class="text-sm font-semibold text-card-foreground">Features</h4>
            <ul class="space-y-1 text-xs text-muted-foreground">
              <li><button type="button" class="hover:text-foreground transition-colors text-left">Calendar</button></li>
              <li><button type="button" class="hover:text-foreground transition-colors text-left">Tasks</button></li>
              <li><button type="button" class="hover:text-foreground transition-colors text-left">Reminders</button></li>
            </ul>
          </div>
        </div>
      </div>

      <div class="border-t border-border mt-6 pt-4 text-center text-muted-foreground">
        <p class="text-xs">&copy; 2025 Lofy Assistant. All rights reserved.</p>
      </div>
    </div>
  </footer>
</div>

<!-- Waitlist Dialog -->
<Dialog.Root bind:open={showWaitlistDialog}>
  <Dialog.Content>
    <Dialog.Header>
      <Dialog.Title>Join the Waitlist</Dialog.Title>
      <Dialog.Description>Enter your phone number and we'll notify you when it's ready.</Dialog.Description>
    </Dialog.Header>

    {#if waitlistSuccess}
      <div class="mt-2 rounded-md border border-emerald-200 bg-emerald-50 p-3 text-sm text-emerald-700">Thanks! You're on the waitlist. We'll text you with updates.</div>
      <Dialog.Footer class="mt-4">
        <Dialog.Close class="px-4 py-2 rounded-md bg-primary text-primary-foreground">Close</Dialog.Close>
      </Dialog.Footer>
    {:else}
      <form class="mt-4 space-y-3" on:submit|preventDefault={handleWaitlistSubmit}>
        <div class="space-y-1">
          <label for="phone" class="text-sm font-medium">Phone number</label>
          <input id="phone" name="phone" type="tel" class="w-full rounded-md border border-border bg-background px-3 py-2 text-sm outline-none focus:ring-2 focus:ring-ring" placeholder="e.g. 0179800424" bind:value={phoneNumber} inputmode="tel" autocomplete="tel" required />
        </div>

        {#if waitlistError}
          <p class="text-sm text-destructive">{waitlistError}</p>
        {/if}

        <Dialog.Footer class="mt-2 gap-2">
          <Dialog.Close class="px-4 py-2 text-sm rounded-md border">Cancel</Dialog.Close>
          <button type="submit" class="px-4 py-2 text-sm rounded-md bg-primary text-primary-foreground disabled:opacity-50" disabled={waitlistSubmitting}>
            {waitlistSubmitting ? "Submitting..." : "Join Waitlist"}
          </button>
        </Dialog.Footer>
      </form>
    {/if}
  </Dialog.Content>
</Dialog.Root>
