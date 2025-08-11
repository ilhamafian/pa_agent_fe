<!-- Dashboard Content -->
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
          <p class="text-xl font-bold text-gray-900">24</p>
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
          <p class="text-xl font-bold text-gray-900">18</p>
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
          <p class="text-xl font-bold text-gray-900">4</p>
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
          <p class="text-xl font-bold text-gray-900">2</p>
        </div>
      </div>
    </div>
  </div>

  <!-- Content Sections -->
  <div class="space-y-6">
    <!-- Recent Tasks -->
    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <h3 class="text-lg font-semibold text-gray-900 mb-3">Recent Tasks</h3>
      <div class="space-y-3">
        <div class="flex items-center p-3 bg-gray-50 rounded-md">
          <div class="flex-shrink-0 w-4 h-4 bg-emerald-500 rounded-full"></div>
          <div class="ml-3 flex-1">
            <p class="text-sm font-medium text-gray-900">Complete project proposal</p>
            <p class="text-xs text-gray-600">High Priority</p>
          </div>
        </div>

        <div class="flex items-center p-3 bg-gray-50 rounded-md">
          <div class="flex-shrink-0 w-4 h-4 bg-blue-500 rounded-full"></div>
          <div class="ml-3 flex-1">
            <p class="text-sm font-medium text-gray-900">Review team feedback</p>
            <p class="text-xs text-gray-600">Medium Priority</p>
          </div>
        </div>

        <div class="flex items-center p-3 bg-gray-50 rounded-md">
          <div class="flex-shrink-0 w-4 h-4 border-2 border-gray-300 rounded-full"></div>
          <div class="ml-3 flex-1">
            <p class="text-sm font-medium text-gray-900">Schedule client meeting</p>
            <p class="text-xs text-gray-600">Medium Priority</p>
          </div>
        </div>
      </div>
    </div>

    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <h3 class="text-lg font-semibold text-gray-900 mb-3">Upcoming Events</h3>
      <div class="space-y-3">
        <div class="flex items-center p-3 bg-gray-50 border-l-4 border-emerald-500">
          <div class="ml-3 flex-1">
            <p class="text-sm font-medium text-gray-900">Follow up with John Doe</p>
            <p class="text-xs text-gray-600">15th August (Friday), 10:00 AM until 12:00 PM</p>
          </div>
        </div>

        <div class="flex items-center p-3 bg-gray-50 border-l-4 border-emerald-500">
          <div class="ml-3 flex-1">
            <p class="text-sm font-medium text-gray-900">Anniversary Dinner Date w Wife</p>
            <p class="text-xs text-gray-600">16th August (Saturday), 6:00 PM until 10:00 PM</p>
          </div>
        </div>

        <div class="flex items-center p-3 bg-gray-50 border-l-4 border-emerald-500">
          <div class="ml-3 flex-1">
            <p class="text-sm font-medium text-gray-900">Badminton Session with Team</p>
            <p class="text-xs text-gray-600">18th August (Monday), 8:00 PM until 10:00 PM</p>
          </div>
        </div>
      </div>
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
