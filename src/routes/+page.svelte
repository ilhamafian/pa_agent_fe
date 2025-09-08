<script lang="ts">
  import { goto } from "$app/navigation";
  import * as Dialog from "$lib/components/dialog";
  import { PUBLIC_BACKEND_URL } from "$env/static/public";
  import { StickyScrollReveal } from "$lib/components/sticky-scroll";

  const content = [
    {
      title: "Smart Calendar Management",
      description: "Automatically schedule events, detect booking templates, and manage your calendar with natural language commands.",
    },
    {
      title: "Intelligent Task Management",
      description: "Create, organize, and track your todo tasks with automatic priority detection and smart categorization.",
    },
    {
      title: "Flexible Reminders",
      description: "Set reminders for calendar events or custom tasks with natural language time expressions.",
    },
    {
      title: "Template Detection",
      description: "Automatically recognize and process booking templates from freelancers and service providers.",
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

  function navigateSomewhere() {
    goto("/onboarding");
  }
</script>

<svelte:head>
  <title>Lofy Assistant</title>
  <meta name="description" content="Your intelligent virtual assistant for calendar management, task organization, and smart reminders with Google Calendar integration." />
</svelte:head>
<!-- Top Navbar -->
<nav class="flex justify-between fixed top-0 left-0 right-0 z-50 items-center px-8 py-4 bg-emerald-200/20 backdrop-blur-xl border-b border-white/20 shadow-md">
  <div class="flex items-center gap-2">
    <img src="/lofy-logo.png" alt="Lofy" class="w-10 h-10" />
    <h1 class="text-2xl font-bold">Lofy</h1>
  </div>
  <div class="flex items-center gap-2">
    <button on:click={handleJoinWaitlist} class="bg-transparent border-primary border-2 text-primary px-6 py-2 rounded-lg font-medium hover:bg-primary/10 transition"> Join Waitlist</button>
  </div>
</nav>
<!-- Hero Section -->
<div class="min-h-screen pt-18">
  <section class="relative overflow-hidden lg:h-screen">
    <div class="absolute inset-0 bg-radial-[at_50%_25%] from-emerald-300/50 to-teal-200/50"></div>
    <!-- Dot Pattern Background -->
    <svg aria-hidden="true" class="pointer-events-none absolute inset-0 h-full w-full z-10 [mask-image:radial-gradient(ellipse_at_top,white,transparent_80%)] text-primary opacity-80">
      <defs>
        <pattern id="dot-pattern" x="0" y="0" width="10" height="10" patternUnits="userSpaceOnUse">
          <circle cx="10" cy="10" r="2" fill="var(--color-emerald-600" />
        </pattern>
      </defs>
      <rect width="100%" height="100%" fill="url(#dot-pattern)" />
    </svg>
    <div class="relative px-4 pt-12 pb-8 z-20">
      <div class="text-center space-y-8">
        <div class="flex flex-col items-center">
          <p class="text-2xl sm:text-7xl font-bold bg-gradient-to-r from-emerald-600 via-teal-500 to-cyan-600 text-transparent bg-clip-text py-6">Lofy; AI Sidekick</p>
          <p class="text-2xl sm:text-4xl font-bold text-foreground leading-tight py-4">Your Chat Buddy Who Actually Remembers Stuff.</p>
          <p class="max-w-3xl text-md text-foreground leading-relaxed">
            Forget the hassle. Manage your schedule and tasks by simply chatting. <br />
            Lofy handles reminders, bookings and more directly through WhatsApp.
          </p>
        </div>

        <div class="flex flex-col max-w-md mx-auto text-center gap-3">
          <button on:click={handleJoinWaitlist} class="bg-primary text-primary-foreground px-6 py-3 rounded-lg font-semibold hover:bg-primary/90 transition-colors shadow-lg"> Join Waitlist</button>
          <button on:click={handleLearnMore} class="border border-foreground text-foreground px-6 py-3 rounded-lg font-semibold hover:bg-accent transition-colors"> Learn More </button>
        </div>
      </div>
    </div>
  </section>
  <!-- Features Section -->
  <div class="">
    <h2 class="text-3xl my-12 text-center font-bold text-foreground">Features</h2>
    <StickyScrollReveal {content} />
  </div>

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
            <h4 class="text-sm font-semibold text-card-foreground">Legal</h4>
            <ul class="space-y-1 text-xs text-muted-foreground">
              <li><a href="/privacy-policy" class="hover:text-foreground transition-colors">Privacy Policy</a></li>
              <li><a href="/terms-agreements" class="hover:text-foreground transition-colors">Terms of Service</a></li>
              <li><button type="button" class="hover:text-foreground transition-colors text-left">Contact</button></li>
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
