<script lang="ts">
  import { goto } from "$app/navigation";
</script>

<svelte:head>
  <title>Terms of Service - Lofy Assistant</title>
  <meta name="description" content="Terms of Service for Lofy Assistant - Understand your rights and responsibilities when using our service." />
</svelte:head>

<!-- Top Navbar -->
<nav class="flex justify-between fixed top-0 left-0 right-0 z-50 items-center px-8 py-4 bg-emerald-200/20 backdrop-blur-xl border-b border-white/20 shadow-md">
  <div class="flex items-center gap-2">
    <img src="/lofy-logo.png" alt="Lofy" class="w-10 h-10" />
    <h1 class="text-2xl font-bold">Lofy</h1>
  </div>
  <div class="flex items-center gap-2">
    <button on:click={() => goto("/")} class="bg-transparent border-primary border-2 text-primary px-6 py-2 rounded-lg font-medium hover:bg-primary/10 transition"> Back to Home </button>
  </div>
</nav>

<!-- Main Content -->
<div class="min-h-screen pt-24 pb-16 px-4">
  <div class="max-w-4xl mx-auto">
    <!-- Header -->
    <div class="text-center mb-12">
      <h1 class="text-4xl font-bold text-foreground mb-4">Terms of Service</h1>
      <p class="text-lg text-muted-foreground">for Lofy Assistant</p>
      <p class="text-sm text-muted-foreground mt-2">Last updated: [Insert Date]</p>
    </div>

    <!-- Content -->
    <div class="bg-card border border-border rounded-lg p-8 shadow-sm">
      <div class="prose prose-emerald max-w-none">
        <p class="text-base text-foreground mb-6 leading-relaxed">Welcome to Lofy Assistant ("we," "our," or "us"). By using our services, you agree to the following Terms of Service. Please read them carefully.</p>

        <!-- Section 1 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">1</span>
            Acceptance of Terms
          </h2>
          <div class="ml-11">
            <p class="text-base text-muted-foreground leading-relaxed">By accessing or using Lofy Assistant, you agree to be bound by these Terms. If you do not agree, please discontinue use of the service.</p>
          </div>
        </div>

        <!-- Section 2 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">2</span>
            Our Services
          </h2>
          <div class="ml-11">
            <p class="text-base text-muted-foreground mb-4 leading-relaxed">Lofy Assistant is a personal WhatsApp assistant that helps you manage your daily activities, including Google Calendar integrations and other productivity tools.</p>
            <p class="text-base text-muted-foreground leading-relaxed">We reserve the right to modify, suspend, or discontinue our services at any time without prior notice.</p>
          </div>
        </div>

        <!-- Section 3 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">3</span>
            User Responsibilities
          </h2>
          <div class="ml-11">
            <p class="text-base text-foreground mb-4 leading-relaxed">By using Lofy Assistant, you agree to:</p>
            <ul class="space-y-3">
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">Provide accurate information (e.g., name, email, phone number).</span>
              </li>
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">Use the service only for lawful purposes.</span>
              </li>
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">Not attempt to disrupt, hack, or misuse the platform.</span>
              </li>
            </ul>
          </div>
        </div>

        <!-- Section 4 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">4</span>
            Privacy
          </h2>
          <div class="ml-11">
            <p class="text-base text-muted-foreground leading-relaxed">
              Your use of Lofy Assistant is also governed by our
              <a href="/privacy-policy" class="text-primary hover:underline font-medium">Privacy Policy</a>, which describes how we collect, use, and protect your data.
            </p>
          </div>
        </div>

        <!-- Section 5 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">5</span>
            Google API Services
          </h2>
          <div class="ml-11">
            <p class="text-base text-muted-foreground leading-relaxed">If you connect your Google Account, you agree that we may access and use Google Calendar data only as necessary to provide the service. You may revoke this access at any time in your Google Account settings.</p>
          </div>
        </div>

        <!-- Section 6 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">6</span>
            Intellectual Property
          </h2>
          <div class="ml-11">
            <p class="text-base text-muted-foreground leading-relaxed">All rights, trademarks, and content related to Lofy Assistant remain our property. You may not copy, modify, or distribute our materials without permission.</p>
          </div>
        </div>

        <!-- Section 7 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">7</span>
            Disclaimer of Warranties
          </h2>
          <div class="ml-11">
            <p class="text-base text-muted-foreground leading-relaxed">Lofy Assistant is provided "as is" and "as available." We do not guarantee that the service will always be error-free, secure, or available without interruption.</p>
          </div>
        </div>

        <!-- Section 8 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">8</span>
            Limitation of Liability
          </h2>
          <div class="ml-11">
            <p class="text-base text-muted-foreground leading-relaxed">To the maximum extent permitted by law, Lofy Assistant shall not be held liable for any indirect, incidental, or consequential damages arising from your use of the service.</p>
          </div>
        </div>

        <!-- Section 9 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">9</span>
            Termination
          </h2>
          <div class="ml-11">
            <p class="text-base text-muted-foreground leading-relaxed">We reserve the right to suspend or terminate your access if you violate these Terms or misuse the service.</p>
          </div>
        </div>

        <!-- Section 10 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">10</span>
            Governing Law
          </h2>
          <div class="ml-11">
            <p class="text-base text-muted-foreground leading-relaxed">These Terms shall be governed by and interpreted in accordance with the laws of [Insert Your Country], without regard to conflict of law principles.</p>
          </div>
        </div>

        <!-- Section 11 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">11</span>
            Contact Us
          </h2>
          <div class="ml-11">
            <p class="text-base text-foreground mb-4 leading-relaxed">If you have questions about these Terms, you can contact us at:</p>
            <div class="bg-card border border-border rounded-lg p-6">
              <div class="flex items-center gap-3">
                <span class="text-2xl">📧</span>
                <a href="mailto:ilhamghaz@gmail.com" class="text-primary hover:underline font-medium text-lg"> ilhamghaz@gmail.com </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Back to Home Button -->
    <div class="text-center mt-8">
      <button on:click={() => goto("/")} class="bg-primary text-primary-foreground px-8 py-3 rounded-lg font-semibold hover:bg-primary/90 transition-colors shadow-lg"> Back to Home </button>
    </div>
  </div>
</div>
