<!-- Dashboard Content -->
<div class="px-4 py-6">
  <!-- Welcome Section -->
  <div class="mb-6">
    <h2 class="text-2xl font-bold text-gray-900 mb-2">Welcome back!</h2>
    <p class="text-gray-600">Here's what's happening with your tasks today.</p>
  </div>

  <!-- Stats Cards -->
  <div class="grid grid-cols-2 gap-3 mb-6">
    <div class="bg-white rounded-lg shadow-sm border border-emerald-100 p-4">
      <div class="flex items-center">
        <div class="flex-shrink-0">
          <div class="w-8 h-8 bg-emerald-100 rounded-full flex items-center justify-center">
            <svg class="w-4 h-4 text-emerald-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
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
          <div class="w-8 h-8 bg-yellow-100 rounded-full flex items-center justify-center">
            <svg class="w-4 h-4 text-yellow-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
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
          <div class="w-8 h-8 bg-red-100 rounded-full flex items-center justify-center">
            <svg class="w-4 h-4 text-red-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
          </div>
        </div>
        <div class="ml-3">
          <p class="text-xs font-medium text-gray-600">Overdue</p>
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
            <p class="text-xs text-gray-600">Due today</p>
          </div>
        </div>

        <div class="flex items-center p-3 bg-gray-50 rounded-md">
          <div class="flex-shrink-0 w-4 h-4 bg-yellow-500 rounded-full"></div>
          <div class="ml-3 flex-1">
            <p class="text-sm font-medium text-gray-900">Review team feedback</p>
            <p class="text-xs text-gray-600">Due tomorrow</p>
          </div>
        </div>

        <div class="flex items-center p-3 bg-gray-50 rounded-md">
          <div class="flex-shrink-0 w-4 h-4 border-2 border-gray-300 rounded-full"></div>
          <div class="ml-3 flex-1">
            <p class="text-sm font-medium text-gray-900">Schedule client meeting</p>
            <p class="text-xs text-gray-600">Due Friday</p>
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
          <span class="text-xs font-medium text-emerald-700">Add Task</span>
        </button>

        <button class="flex flex-col items-center p-3 bg-blue-50 rounded-lg active:bg-blue-100 transition-colors">
          <svg class="w-5 h-5 text-blue-600 mb-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
          </svg>
          <span class="text-xs font-medium text-blue-700">Schedule</span>
        </button>

        <button class="flex flex-col items-center p-3 bg-purple-50 rounded-lg active:bg-purple-100 transition-colors">
          <svg class="w-5 h-5 text-purple-600 mb-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z" />
          </svg>
          <span class="text-xs font-medium text-purple-700">Reports</span>
        </button>

        <button class="flex flex-col items-center p-3 bg-orange-50 rounded-lg active:bg-orange-100 transition-colors">
          <svg class="w-5 h-5 text-orange-600 mb-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"
            />
          </svg>
          <span class="text-xs font-medium text-orange-700">Team</span>
        </button>
      </div>
    </div>
  </div>
</div>
