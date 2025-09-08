<script lang="ts">
  import { goto } from "$app/navigation";
</script>

<svelte:head>
  <title>Privacy Policy - Lofy Assistant</title>
  <meta name="description" content="Privacy Policy for Lofy Assistant - Learn how we collect, use, and protect your personal information." />
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
      <h1 class="text-4xl font-bold text-foreground mb-4">Privacy Policy</h1>
      <p class="text-lg text-muted-foreground">for Lofy Assistant</p>
      <p class="text-sm text-muted-foreground mt-2">Last updated: 08/09/2025</p>
    </div>

    <!-- Content -->
    <div class="bg-card border border-border rounded-lg p-8 shadow-sm">
      <div class="prose prose-emerald max-w-none">
        <p class="text-base text-foreground mb-6 leading-relaxed">Lofy Assistant ("we," "our," or "us") respects your privacy and is committed to protecting it. This Privacy Policy explains how we collect, use, and safeguard your information when you use our services.</p>

        <!-- Section 1 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">1</span>
            Information We Collect
          </h2>
          <div class="ml-11">
            <p class="text-base text-foreground mb-4 leading-relaxed">When you use Lofy Assistant, we may collect the following information:</p>
            <ul class="space-y-3">
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <div>
                  <strong class="text-foreground">Personal Information:</strong>
                  <span class="text-muted-foreground"> Your name, email address, and phone number.</span>
                </div>
              </li>
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <div>
                  <strong class="text-foreground">Google Account Data:</strong>
                  <span class="text-muted-foreground"> With your permission, we may access your Google Calendar and related information to help you manage events.</span>
                </div>
              </li>
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <div>
                  <strong class="text-foreground">Usage Data:</strong>
                  <span class="text-muted-foreground"> Information about how you interact with the app to improve our services.</span>
                </div>
              </li>
            </ul>
          </div>
        </div>

        <!-- Section 2 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">2</span>
            How We Use Your Information
          </h2>
          <div class="ml-11">
            <p class="text-base text-foreground mb-4 leading-relaxed">We use the information we collect only for the following purposes:</p>
            <ul class="space-y-3">
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">To provide and improve our personal assistant services.</span>
              </li>
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">To allow integration with Google Calendar and help you manage tasks/events.</span>
              </li>
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">To communicate with you about updates, support, and security notices.</span>
              </li>
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">To comply with legal obligations.</span>
              </li>
            </ul>
            <p class="text-base text-foreground mt-4 leading-relaxed">We do not sell or share your personal data with third parties, except with trusted service providers necessary to operate our app (e.g., hosting providers, databases).</p>
          </div>
        </div>

        <!-- Section 3 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">3</span>
            Data Storage & Security
          </h2>
          <div class="ml-11">
            <ul class="space-y-3">
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">Your data is stored securely on our servers using MongoDB and hosted backend infrastructure on Google Cloud Run.</span>
              </li>
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">The frontend is hosted on Vercel.</span>
              </li>
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">We apply industry-standard security measures to protect your data against unauthorized access, alteration, or disclosure.</span>
              </li>
            </ul>
          </div>
        </div>

        <!-- Section 4 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">4</span>
            Sharing with Third Parties
          </h2>
          <div class="ml-11">
            <p class="text-base text-foreground mb-4 leading-relaxed">We may share limited data with:</p>
            <ul class="space-y-3">
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <div>
                  <strong class="text-foreground">Google APIs:</strong>
                  <span class="text-muted-foreground"> To provide integrations (e.g., Google Calendar).</span>
                </div>
              </li>
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <div>
                  <strong class="text-foreground">Service Providers:</strong>
                  <span class="text-muted-foreground"> Such as hosting and database providers, only as needed to operate the app.</span>
                </div>
              </li>
            </ul>
            <p class="text-base text-foreground mt-4 leading-relaxed">We do not share your information with advertisers or unrelated third parties.</p>
          </div>
        </div>

        <!-- Section 5 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">5</span>
            User Rights
          </h2>
          <div class="ml-11">
            <p class="text-base text-foreground mb-4 leading-relaxed">You have the right to:</p>
            <ul class="space-y-3">
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">Request access to the personal data we hold about you.</span>
              </li>
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">Request correction or deletion of your data.</span>
              </li>
              <li class="flex items-start gap-3">
                <div class="w-2 h-2 bg-primary rounded-full mt-2 flex-shrink-0"></div>
                <span class="text-muted-foreground">Revoke permissions granted to Google APIs at any time from your Google Account settings.</span>
              </li>
            </ul>
            <div class="mt-4 p-4 bg-primary/10 rounded-lg border border-primary/20">
              <p class="text-sm text-foreground">
                <strong>Contact us:</strong> To exercise these rights, please contact us at
                <a href="mailto:ilhamghaz@gmail.com" class="text-primary hover:underline font-medium">ilhamghaz@gmail.com</a>
              </p>
            </div>
          </div>
        </div>

        <!-- Section 6 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">6</span>
            Children's Privacy
          </h2>
          <div class="ml-11">
            <p class="text-base text-muted-foreground leading-relaxed">Our services are not directed to individuals under the age of 13, and we do not knowingly collect data from them.</p>
          </div>
        </div>

        <!-- Section 7 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">7</span>
            Changes to This Privacy Policy
          </h2>
          <div class="ml-11">
            <p class="text-base text-muted-foreground leading-relaxed">We may update this Privacy Policy from time to time. Changes will be posted on this page with the updated "Last updated" date.</p>
          </div>
        </div>

        <!-- Section 8 -->
        <div class="mb-8">
          <h2 class="text-2xl font-bold text-foreground mb-4 flex items-center gap-3">
            <span class="w-8 h-8 bg-primary text-primary-foreground rounded-full flex items-center justify-center text-sm font-bold">8</span>
            Contact Us
          </h2>
          <div class="ml-11">
            <p class="text-base text-foreground mb-4 leading-relaxed">If you have any questions or requests about this Privacy Policy, you may contact us at:</p>
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
