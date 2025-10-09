<script>
  import ShineBorder from "$lib/components/shine-border/shine-border.svelte";
</script>

<div class="min-h-screen bg-gradient-to-b from-gray-50 to-white dark:from-gray-900 dark:to-gray-800">
  <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-6 sm:py-10 lg:py-12 max-w-5xl">
    <!-- Header -->
    <div class="mb-8 sm:mb-12">
      <h1 class="text-3xl sm:text-4xl lg:text-5xl font-bold mb-3 sm:mb-4 text-gray-900 dark:text-white">
        Lofy Assistant Guide
      </h1>
      <p class="text-base sm:text-lg text-gray-600 dark:text-gray-300 max-w-3xl">
        Learn how to use Lofy Assistant to manage tasks, calendar events, reminders, and notes with natural language commands.
      </p>
    </div>

    <div class="space-y-6 sm:space-y-8">
      <!-- Calendar Management -->
      <ShineBorder>
        <div class="p-4 sm:p-6 lg:p-8">
          <div class="flex items-start gap-3 sm:gap-4 mb-4">
            <div class="text-2xl sm:text-3xl">📅</div>
            <div class="flex-1">
              <h2 class="text-xl sm:text-2xl font-bold text-gray-900 dark:text-white mb-2">Calendar Management</h2>
              <p class="text-sm sm:text-base text-gray-600 dark:text-gray-300">Schedule and manage your events with natural language</p>
            </div>
          </div>
          
          <div class="space-y-4 sm:space-y-6">
            <div>
              <h3 class="text-base sm:text-lg font-semibold text-gray-800 dark:text-gray-200 mb-3">Example Commands</h3>
              <div class="space-y-2 bg-gray-50 dark:bg-gray-800/50 rounded-lg p-3 sm:p-4">
                <code class="block text-xs sm:text-sm text-gray-700 dark:text-gray-300">"Schedule a team meeting tomorrow at 2pm"</code>
                <code class="block text-xs sm:text-sm text-gray-700 dark:text-gray-300">"Add dentist appointment on Friday at 10am"</code>
                <code class="block text-xs sm:text-sm text-gray-700 dark:text-gray-300">"Book an all-day workshop next Monday"</code>
              </div>
            </div>

            <div class="border-l-4 border-emerald-500 pl-4 py-2 bg-emerald-50 dark:bg-emerald-900/20">
              <h4 class="text-sm sm:text-base font-semibold text-gray-800 dark:text-gray-200 mb-2">Key Features</h4>
              <ul class="space-y-1.5 text-xs sm:text-sm text-gray-600 dark:text-gray-400">
                <li>• Default duration: 1 hour (if end time not specified)</li>
                <li>• Supports all-day events</li>
                <li>• Handles booking templates</li>
              </ul>
            </div>
          </div>
        </div>
      </ShineBorder>

      <!-- Reminders -->
      <ShineBorder>
        <div class="p-4 sm:p-6 lg:p-8">
          <div class="flex items-start gap-3 sm:gap-4 mb-4">
            <div class="text-2xl sm:text-3xl">⏰</div>
            <div class="flex-1">
              <h2 class="text-xl sm:text-2xl font-bold text-gray-900 dark:text-white mb-2">Reminders</h2>
              <p class="text-sm sm:text-base text-gray-600 dark:text-gray-300">Get notified at the right time</p>
            </div>
          </div>

          <div class="grid sm:grid-cols-2 gap-4 sm:gap-6">
            <div class="bg-gray-50 dark:bg-gray-800/50 rounded-lg p-4">
              <h3 class="text-base sm:text-lg font-semibold text-gray-800 dark:text-gray-200 mb-3">Event Reminders</h3>
              <p class="text-xs sm:text-sm text-gray-600 dark:text-gray-400 mb-3">Get notified before calendar events</p>
              <code class="block text-xs sm:text-sm text-gray-700 dark:text-gray-300 bg-white dark:bg-gray-900 p-2 rounded">
                "Remind me 30 minutes before my meeting"
              </code>
            </div>

            <div class="bg-gray-50 dark:bg-gray-800/50 rounded-lg p-4">
              <h3 class="text-base sm:text-lg font-semibold text-gray-800 dark:text-gray-200 mb-3">Custom Reminders</h3>
              <p class="text-xs sm:text-sm text-gray-600 dark:text-gray-400 mb-3">Set one-time reminders</p>
              <div class="space-y-2">
                <code class="block text-xs sm:text-sm text-gray-700 dark:text-gray-300 bg-white dark:bg-gray-900 p-2 rounded">
                  "Remind me in 3 hours to check the laundry"
                </code>
              </div>
            </div>
          </div>
        </div>
      </ShineBorder>

      <!-- Task Management -->
      <ShineBorder>
        <div class="p-4 sm:p-6 lg:p-8">
          <div class="flex items-start gap-3 sm:gap-4 mb-4">
            <div class="text-2xl sm:text-3xl">✅</div>
            <div class="flex-1">
              <h2 class="text-xl sm:text-2xl font-bold text-gray-900 dark:text-white mb-2">Task Management</h2>
              <p class="text-sm sm:text-base text-gray-600 dark:text-gray-300">Organize your to-dos with priority levels</p>
            </div>
          </div>

          <div class="space-y-4 sm:space-y-6">
            <div>
              <h3 class="text-base sm:text-lg font-semibold text-gray-800 dark:text-gray-200 mb-3">Priority Levels</h3>
              <div class="grid sm:grid-cols-3 gap-3">
                <div class="flex items-center gap-2 bg-red-50 dark:bg-red-900/20 p-3 rounded-lg">
                  <span class="text-lg">🔴</span>
                  <div>
                    <div class="text-sm font-semibold text-gray-800 dark:text-gray-200">High</div>
                    <div class="text-xs text-gray-600 dark:text-gray-400">Urgent tasks</div>
                  </div>
                </div>
                <div class="flex items-center gap-2 bg-yellow-50 dark:bg-yellow-900/20 p-3 rounded-lg">
                  <span class="text-lg">🟡</span>
                  <div>
                    <div class="text-sm font-semibold text-gray-800 dark:text-gray-200">Medium</div>
                    <div class="text-xs text-gray-600 dark:text-gray-400">Default priority</div>
                  </div>
                </div>
                <div class="flex items-center gap-2 bg-green-50 dark:bg-green-900/20 p-3 rounded-lg">
                  <span class="text-lg">🟢</span>
                  <div>
                    <div class="text-sm font-semibold text-gray-800 dark:text-gray-200">Low</div>
                    <div class="text-xs text-gray-600 dark:text-gray-400">Nice to have</div>
                  </div>
                </div>
              </div>
            </div>

            <div>
              <h3 class="text-base sm:text-lg font-semibold text-gray-800 dark:text-gray-200 mb-3">Example Commands</h3>
              <div class="space-y-2 bg-gray-50 dark:bg-gray-800/50 rounded-lg p-3 sm:p-4">
                <code class="block text-xs sm:text-sm text-gray-700 dark:text-gray-300">"Add task: Submit project proposal"</code>
                <code class="block text-xs sm:text-sm text-gray-700 dark:text-gray-300">"Create high priority task: Call client"</code>
                <code class="block text-xs sm:text-sm text-gray-700 dark:text-gray-300">"Mark task 2 as completed"</code>
              </div>
            </div>
          </div>
        </div>
      </ShineBorder>

      <!-- Notes -->
      <ShineBorder>
        <div class="p-4 sm:p-6 lg:p-8">
          <div class="flex items-start gap-3 sm:gap-4 mb-4">
            <div class="text-2xl sm:text-3xl">📝</div>
            <div class="flex-1">
              <h2 class="text-xl sm:text-2xl font-bold text-gray-900 dark:text-white mb-2">Smart Notes</h2>
              <p class="text-sm sm:text-base text-gray-600 dark:text-gray-300">Your personal knowledge base</p>
            </div>
          </div>

          <div class="grid sm:grid-cols-2 gap-4 sm:gap-6">
            <div class="bg-gray-50 dark:bg-gray-800/50 rounded-lg p-4">
              <h3 class="text-base sm:text-lg font-semibold text-gray-800 dark:text-gray-200 mb-3">Create Notes</h3>
              <div class="space-y-2">
                <code class="block text-xs sm:text-sm text-gray-700 dark:text-gray-300 bg-white dark:bg-gray-900 p-2 rounded">
                  "Save this as a note: [content]"
                </code>
                <code class="block text-xs sm:text-sm text-gray-700 dark:text-gray-300 bg-white dark:bg-gray-900 p-2 rounded">
                  "Remember this: [content]"
                </code>
              </div>
            </div>

            <div class="bg-gray-50 dark:bg-gray-800/50 rounded-lg p-4">
              <h3 class="text-base sm:text-lg font-semibold text-gray-800 dark:text-gray-200 mb-3">Search Notes</h3>
              <div class="space-y-2">
                <code class="block text-xs sm:text-sm text-gray-700 dark:text-gray-300 bg-white dark:bg-gray-900 p-2 rounded">
                  "Find my note about the meeting"
                </code>
                <code class="block text-xs sm:text-sm text-gray-700 dark:text-gray-300 bg-white dark:bg-gray-900 p-2 rounded">
                  "Show me notes from last week"
                </code>
              </div>
            </div>
          </div>
        </div>
      </ShineBorder>

      <!-- Quick Tips -->
      <div class="bg-gradient-to-r from-emerald-50 to-indigo-50 dark:from-emerald-900/20 dark:to-indigo-900/20 rounded-xl p-4 sm:p-6 lg:p-8 border border-emerald-200 dark:border-emerald-800">
        <h2 class="text-xl sm:text-2xl font-bold text-gray-900 dark:text-white mb-4">Quick Tips</h2>
        <div class="grid sm:grid-cols-2 gap-3 sm:gap-4">
          <div class="flex gap-3">
            <span class="text-emerald-600 dark:text-emerald-400 mt-0.5">✓</span>
            <p class="text-xs sm:text-sm text-gray-700 dark:text-gray-300">Use natural language - talk as you would to a friend</p>
          </div>
          <div class="flex gap-3">
            <span class="text-emerald-600 dark:text-emerald-400 mt-0.5">✓</span>
            <p class="text-xs sm:text-sm text-gray-700 dark:text-gray-300">I'll confirm details before scheduling events</p>
          </div>
          <div class="flex gap-3">
            <span class="text-emerald-600 dark:text-emerald-400 mt-0.5">✓</span>
            <p class="text-xs sm:text-sm text-gray-700 dark:text-gray-300">Ask questions anytime - I'm here to help</p>
          </div>
          <div class="flex gap-3">
            <span class="text-emerald-600 dark:text-emerald-400 mt-0.5">✓</span>
            <p class="text-xs sm:text-sm text-gray-700 dark:text-gray-300">I suggest saving important info as notes automatically</p>
          </div>
        </div>
      </div>

      <!-- Get Started CTA -->
      <div class="text-center py-6 sm:py-8">
        <h2 class="text-2xl sm:text-3xl font-bold text-gray-900 dark:text-white mb-3 sm:mb-4">Ready to Get Started?</h2>
        <p class="text-sm sm:text-base text-gray-600 dark:text-gray-400 mb-4 sm:mb-6 max-w-2xl mx-auto">
          Start by saying "Hi" or ask me to help you with your first task. I'm here to make your day easier.
        </p>
        <a href="/dashboard" class="inline-block bg-emerald-500 hover:bg-emerald-600 text-white font-semibold px-6 sm:px-8 py-2.5 sm:py-3 rounded-lg transition-colors duration-200 text-sm sm:text-base">
          Go to Dashboard
        </a>
      </div>
    </div>
  </div>
</div>
